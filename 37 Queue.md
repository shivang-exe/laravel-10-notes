##                                       LARAVEL QUEUE TO ADVANCE

Connect Laravel with SQS- `https://medium.com/@jogarcia/how-to-configure-laravel-queues-and-aws-sqs-35fcef3f343d`

=> When we perform any task we expect its response in just for couple of seconds. However if a request(endpoint) takes longer time than that then program think there might be a problem and can throw errors. so its better to execute task asyncronously which is taking longer time to execute. 

=> To achieve asyncronous execution laravel provides us a Queue Mechanism. [ Jobs and Workers ]

  > Create a Job - `php artisan make:job JobName`
  > location - App\Jobs\JobName.php


=> To make a Job execute in Controller we dispatch it to the appropriate place.

  > use App\Jobs\JobName;
  > JobName::dispatch();

  Add a delay
  > JobName::dispatch()->delay(5);

=> To execute a job we need a worker. A worker is responsible for checking jobs in Job store and execute them one by one.

   > Create a worker - php artisan queue:work



# Key Features

# -> We can add setTimeout in jobs: If a job takes more than provided time then worker will automatically terminate the job.

Q- How can we set the time ?
A- Add a public property in your job class named timeout and set its value as no. of seconds;
   > ex: public $timeout = 1;



# -> We can set no of tries a job will execute if it fails previously.

Q- How ?
A- Add a public property in your job class named tries and set its value as no. of tries;
   > ex: public $tries = 1;

   What if we assign a -ve value. (will try for unlimited times)




# -> We can also set a Timer: Try reexecute untill 1 minute.

Q- How ?
A-  Add a function retryUntil() and return time;
{{
    function retryUntil() {
       return now()->addminute(1);
    }
}}



# -> Add a delay between the retries Jobs.
> public $backoff = 2;
// Now worker will wait for 2 secs each time it will retry a job.

Suppose there are 3 # of tries, then we can pass a array also which is defining the delay after each try.
{{
    public $tries = 3;
    public $backoff = [ 1, 3, 0.5 ];
}} 




`NOTE: We know that each worker can execute one job at a time. If we want to speed up things then we need to start more worker at a glance.[ Multiple workers -> Multiple jobs execute at a same time ]`




# Set a Priority of Jobs

We know that Queue executes in FIFO manner. But what if we have a Job having higher priority than all.
In this case we use ordering to execute jobs on the basis of names.

By default all jobs has a default name called   `default`.
Can change it while dispatching by
{{
        JobName::dispatach()->onQueue('payments');     
}}

Now we can execute worker by providing the order of execution of jobs on the basis of thier names.
> ex: php artisan queue:work  --queue=payments,default
 



# What happens when a job fails ?

A failed job is automatically store under failed_jobs table for the purpose of retry later. If we want to manually retry a failed job, then we can use a cmd `php artisan queue:retry [job_uuid]`. This cmd will again push this same job to the queue.




# Rate limiting in Jobs

While performing an operation if you want to rate limit the jobs execution / sec then we can use release() method.
{{
    // Class properties and methods \App\Jobs\ExampleJob.php
    public $tries = 3;
    public $maxExceptions = 2; // If count of exception thrown by a job is grater than 2 then Job will no more be tried again

    public function handle()
    {
        // In every two seconds this will execute only once
        return $this->release(2); // back job to queue after 2 seconds
    }

}}



# Something to do if a job fails ?
Laravel provide a handy failed method that expects a execption in the parameter and will run everytime a job will fail.
{{

    public $tries = 10;
    public $maxExceptions = 2;
    public function handle()
    {
        throw new \Exception();
        return $this->release();
    }

    public function failed($e) //this method execute when job will fail
    {

        // can see the logs under storage -> logs -> laravel.log
        Illuminate\Support\Facades\Log::info('Failed');
    }
}}



# What if I have to run multiple jobs or chain of jobs one by one or parallely

There we have Bus Facade which helps us to do so.

`Bus::chain()` => enable us to execute multiple jobs one by one.  
Cons:
   If any of one job breaks for some reason, further remaining jobs will also not execute.
   They depends on each other

{{

    $chain = [                                                                                              $
        new \App\Jobs\PullRepo(),
        new \App\Jobs\RunTests(),
        new \App\Jobs\Deploy() 
    ];

    \Illuminate\Support\Facades\Bus::chain($chain)->dispatch();

}}

To remove this dependency we can utilize a batch(). It will execute all sepaerately that means there is no dependency on one another. Before using batch we have to create a batch table( iff we are using Database for execution )
`Bus::batch()` => enable us to execute multiple jobs parallely.  

{{

    // create a batch
     `php artisan queue:batches-table
      php artisan migrate`



    // Add a trait to Job class which you want to be batched
    use Illuminate\Bus\Batchable\Batchable;



    // use batch
    $batch = [                                                                                              $
        new \App\Jobs\PullRepo(),
        new \App\Jobs\RunTests(),
        new \App\Jobs\Deploy() 
    ];
    \Illuminate\Support\Facades\Bus::batch($chain)->dispatch();



    // You can add this in handle method to Job Class if the batch has been cancelled. it will also make a job cancel
    public function handle()
    {
        if ($this->batch()->cancelled()) {
            return;
        }
    }


    // If you don't want to make this job auto cancellation then we can simply use
    \Illuminate\Support\Facades\Bus::batch($chain)->allowFailures()->dispatch();

}}




# Standardization

{{

    Bus::batch([
        new PullRepo('laracasts/project1'),
        new PullRepo('laracasts/project2'),
        new PullRepo('laracasts/project3')
    ])
        ->allowFailures() // a job failure does not automatically mark the batch as cancelled

        ->catch(function (Batch $batch, Throwable $e) {

            // execute if any one of the above jobs failed

        })->then(function (Batch $batch) {
            
            // execute when all jobs executed successfully...

        })->finally(function (Batch $batch) {]

            // executes when batch has finished executing even some failures (At least one job must success)...

        })
        ->onQueue('deployment') // specify the queue
        ->onConnection('database') // specify the connection
        ->dispatch();


}}



# Workflows

-> Chain inside a batch 
{{
    $batch = [
        [
            new PullRepo('laracasts/project1'),
            new RunTests('laracasts/project1'),
            new Deploy('laracasts/project1')
        ],

        // first ^ will execute then go for v

        [
            new PullRepo('laracasts/project2'),
            new RunTests('laracasts/project2'),
            new Deploy('laracasts/project2')
        ]
    ];
    Bus::batch($batch)
        ->allowFailures()
        ->dispatch();

}}

-> Batch inside a chain

{{
    \Illuminate\Support\Facades\Bus::chain([
        new \App\Jobs\PullRepo(),
        function () {
            \Illuminate\Support\Facades\Bus::batch([...])->dispatch()
        }
    ])->dispatch();

}}




# Understanding Race conditions

A situation when two or more processes are trying to make changes on a same resource at a same time.
Similarly this case can happen with jobs. Two same jobs can execute by a two different workers at a same time are accessing the same resource at a same time. `how we will tackle this ?`

> 1st approach to solve this is using Cache::lock()

{{

    // MakePayment.php Job

  public function handle(): void
    {
        // Cache -> Global Cache facade 
        // lock  -> create a lock of name payments, 
        // block -> This method will keep try to acquire a lock. Try for provided no. of seconds and still if not get lock then it will throw LockTimeoutException, but if it gets lock within time then it will invoke the passed closure.

        Cache::lock('payments')->block(1, function() {
            info('Payment has started');
            sleep(5);
            info('Payment is done');
        });
    }

}}


> 2nd approach to solve this is using Redis::funnel()
{{

    // Class methods \App\Jobs\Example.php
    public function handle()
    {
        Redis::throttle('key_name')
            ->limit(10)               // limit only 10 jobs can run concurrently  
            ->block(10)               // wait for 10 secs and try acquire lock
            ->then(function () {

                Log::info('Started Deploying...');
                sleep(5);
                Log::info('Finished Deploying...');

            });			
    }

}}

> 3rd approach to solve this is using Redis::throttle()
{{

    // Class methods \App\Jobs\Example.php
    public function handle()
    {
        Redis::throttle('key') 
            ->allow(10)                               // allow only 10 jobs can run 
            ->every(60)                               // allow only 10 jobs can run in 60 seconds
            ->block(10)                               // wait for 10 secs and try acquire lock
            ->then(function () {                      // once acquired start executing closure
                Log::info('Started Deploying...');
                sleep(5);
                Log::info('Finished Deploying...');
            });			
    }

}}


> 4th Approach is to just using a simple middleware
This middleware will do not block any job instead it just release the job again to the queue. we can add a delay also in the second parameter of constructor.

{{
    public function middleware()
    {
        return [new WithoutOverlapping('key_name', 10)];
    }
}}