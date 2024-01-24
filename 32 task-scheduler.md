#                                   TASK SCHEDULER

laravel also gives us a phenomenal feature i.e. Task Scheduler. With its help we can schedule our tasks in a certain time cycle. like: daily, every-minute, every-second etc

TASK - How can we define our tasks.

# Step 1: Create custom artisan command
yes, create your custom artisan cmd and in its handle function define your task.

cmd: `php artisan make:command CommandName`

# Step 2: Config your cmd
Go to App\Console\Kernel.php inside the schedule function mention your created command and then use methods to assign the time cycle.

{{

    protected function schedule(Schedule $schedule): void
    {
        $schedule->command('app:custom-command')->daily();$
    }
    
}}

cmd: `php artisan schedule:work`