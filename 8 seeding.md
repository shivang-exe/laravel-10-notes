###                                              SEEDING 

What do we mean by seeding? 
Sometimes we need an excess amount of data to add in our database just for testing purpose. The process of inserting such amount of data into database in just few lines of code is called seeding.

<a- Let's start seeding

# Step 1
Create seeder file 

run cmd: `php artisan make:seeder SeederName`


# Step 2
Insert Data and execute

{{

    use Illuminate\Support\Facades\DB;

    DB::table('TableName')->insert([
        'key1' => 'value1',
        'key2' => 'value2',
        'key3' => 'value3',
    ]);

}}

run cmd: `php artisan db:seed --class=StudentSeeder`


    (although we can run `php artisan db:seed` that executes the main seeder file(DatabaseSeeder) and in that file we can call other seeder files )

{{
public function run(){

        $this->call([                                                                $                   
            SeederName::class,
            SeederName::class,
        ])

 }
}} 



# Using faker to insert fake data

{{

    use Faker\Factory as Faker;
    use Illuminati\Support\Facades\DB;

    public function run()
    {

        $faker = Faker::create();                                                                          $
        
        // Will insert 10 fake data for us
        foreach( range(1,10) as $value )
        {
            DB::table('TableName')->insert([

                'name' => faker()->name(),
                'email'=> faker()->email(),
                'password' => faker()->password()
            ]);
        }
    }
}}


