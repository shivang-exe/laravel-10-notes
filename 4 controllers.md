#                               Controllers? 
Controllers are collection of fxns that encapsulates the buisness logic in itself.

By hitting some routes we want to execute some sort of statements we may code those statements in the routes file itself but following the seperation of cocern we should write our buisness logic seperately there we code all of the  buisness logic inside in the controllers.

Controller are like mediator between Routes and Views, Routes -> Controllers -> Views.

We do it just because to ensures simplicity in our code, by seperating all the complex part from routes files to controller files.

In routes file, it only tells that on this particular URI that specific Controller will execute.

{{
-----------------> Creating a Controller class

run cmd      :-       $` php artisan make:controller controller_name`
example cmd  :-       $` php artisan make:controller homeController`    

// the above cmd will create a controller for you with some pre written code.



// ------------------> Creating a function in controller

function function_name()
{
    return view( 'about', ['key' => 'value' ] );
}

ex: 

function show()
{
    return view( 'home', ['roll_no' => 21 ] );
}



// --------------------> using the controller in web.php route file



use App\Http\Controllers\controller_name;

Route:: get( 'URI', [controller_name::class, 'function_name' ] );

ex: 
Route:: get( '/home', [homeController::class, 'show' ] );





// ---------------------> Passing values through routes

Route:: get( '/student/{id}' , [controller_name::class, 'showId' ] );

now in the showId method, it get one parameter which we write in the URI in place of id;

                method definition:

                function showId( $id )
                {
                    return 'Your ID is '.$id;
                }




// ---------------------------> Single Action Controller

`if we had to make a controller that contains only single method then we don't need to follow the previous process instead follow this new proccess:`

run cmd  ->         $` php artisan make:controller singleActionController --invokable`

`it automatically crate a controller for you, you just has to write your business logic only.
At the route, you also not need to specify the method name.`

{{
    use App\Http\Controllers\SingleControllerName;
    
    Route::get('/', SingleControllerName::class); 
}}






// -------------------------> Taking to the phenomenal: Rescource Controller

Do you know why we are creating our routes with a specific naming like : `index`, `show`, `edit`, `update`, `create`, `store`, `destroy`.

Because Php use this special naming convention and along with them if we use the rescource controller then we dont need to specify all those routes. and even laravel will create all controllers for us. We have to just write our logic only

cmd: `php artisan make:controller ControllerName --rescource`


Verb 	    URI 	                   ControllerName	    Route Name

GET	        /photos	                    index	            photos.index
GET	        /photos/create	            create	            photos.create
POST	    /photos	                    store	            photos.store
GET	        /photos/{photo}	            show	            photos.show
GET	        /photos/{photo}/edit	    edit	            photos.edit
PUT/PATCH	/photos/{photo}	            update	            photos.update
DELETE	    /photos/{photo}	            destroy	            photos.destroy

}}



Set the route: 
{{
    // will create all the routes and controllers automatically
    Route::resource('/photos', ControllerName::class);

    // If we want to create some specific routes only then we can simple use only() method to enable them
    Route::rescource('/photos', ControllerName::class)->only( 'index', 'show' );

    // Nested routes using scoped
    Route:rescource('photos.persons', ControllerName::clas )->scoped(['photo'=>'person']) 

    
}}





#  API CONTROLLERS ONLY

cmd: `php artisan make:controller ControllerName --api`

inside api.php

Route: `Route::apiResource('photos', ControllerName::class );`
