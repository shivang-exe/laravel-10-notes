##                              Middleware 

Middleware is exactly what we had studied in Express.

create:  `php artisan make:middleware Middleware_name`

we get our middleware in app/http/middleware/middleware_name.


`NOTE` To create all the laravel error blade files automatically
cmd: `php artisan vendor:publish --tag=laravel-errors`


  request            |              |
-------------->      |  Middleware  |      -----------> Process
                     |              | 


here we have three types of middleware:

1. Global Middleware                          -> This middleware works globally on every request.
2. Assigning Middleware to Routes             -> This middleware is applied to specific routes only.
3. Middleware Groups                          -> This is applied for cluster of routes.

We have to register each type in a different manner.




# ----------------------> Let see the Global Middleware:

    In the app/http/kernel.php file -> we have to add our middleware inside a middleware array to apply it globally.





# ----------------------> Assigning Middleware to a specific route.

    In the App/http/kernel.php file -> we got an array named routeMiddleware, so we had to list our middleware in this array.

    How to use that middleware for a specific route ?

    When we are assigning that middleware in the list meanwhile we also assign a particular 'key' with rerspect to it. now in future we use that middleware using that key only. 
    ex: ->middleware('key');
{{
    Route:: get( '/profile', [ Controller_name::class, 'showProfile' ] )->middleware('middleware_key');
}}




# ----------------------> Grouping Middleware

    When we had to execute multiple middlewares on a single specific key, in such a situation we use grouping middleware technique.

       ex: 'key' => [ \middleware1::class,  \middleware2::class,  ..... ]

    In the same file Kernel.php we will get another array named `middlewareGroups`, under this array list your own middlewares with a common key.


    and use that key while creating the routes;
 {{
    Route:: get('/data', [Controller::class, 'fn'] )->middleware('name');
}}






# -----------------------> What if for multiple routes we had to execute only one middleware.

    instead of writing -> middleware('same_key') again and again just use different approach
{{
    Route:: middleware(['construction'])->group( function(){

        Route:: get( '/', ....  );

        Route:: get( '/profile', .... );

    }); 
}}
    In this way we can avoid writing the same code again and again.


    


