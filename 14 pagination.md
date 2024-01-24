#                             PAGINATION

Pagination can be one of the simplest task that laravel provides us.
We just have to call couple of functions to use inbuilt pagination of laravel.
Simplicity Level: 100


{{
    

    // Controller File

    use App\Models\Task;
    function showAll()
    {
        // instead of using Task::all() use paginate()
        return view( 'index', [ 'tasks' => Task::latest()->paginate() ]);
    }




    // Blade file

    @if($tasks->count() > 0 )
      <div>
          {{ $tasks->links() }}
      </div>
    @endif

}}


//Add bootstrap in your pagination

Locate to App\Providers\AppServiceProvider.php

{{
    use Illuminate\Pagination\Paginator;

    public function boot()
    {
        Paginator::useBootstrap();
    }

    // Now add bootstrap file in blade file and check your pagination has automatically been applied


    $ To customise your styles $
    // run - php artisan vendor:publish --tag=laravel-pagination

    // output file in -> rescources/views/vendor/pagination
}}

