##                                          ROUTES

# Note: 
The 4 pillars of laravel are `Routes`, `Controllers`,`Views` and `Models`.
.^. To master `laravel`, you should have good command in these four.



### Routes:

    We can see in our Routes folder we get a class named Route( in web.php ), using ::(scope resolution) operator we call the inbuilt static function of routes. like:- get, post etc

    ------> syntax: Route::method( 'URI', callback )

{{
    ex: Route::get('/', function(){
        return '<h1>huehuehue</h1>'
    })
}}




#    ------> How to access the value from URI ?

{{
    Route::get( '/user/{_id}', function( $id ){
        return $id;
    });
}}





#    ------> How to access multiple values from URI ?
    
{{
    Route::get( '/user/{_id}/product/{p_id}', function( $id, $pid ){
        return $id . $pid;
    });
}}





#    ------> Optional Parameters. Providing value in the Parameters in URI is not necessary.


{{
    Route::get( '/user/{ name? }', function( $name='dogesh' ){                      
    // Here if we don't provide any param in URI then it takes 'dogesh' as its default value

        return "Hello".$name ;

    });
}}


`NOTE: If any URI matches with two or more routes then that route will execute which will trigger first`


#    ------> Restricting the parameters input by using Regular Expression. 

    SYNTAX: 
    
        Route::get( '/user/{name}', function($name){

            return $name;

        })->where('name', 'REGEX' );
    
    EXAMPLE 1: 
{{
            Route::get( '/user/{name}', function($n){

                return $n;

            })->where( 'name', '[A-Za-z]+' );
}}


    EXAMPLE 2: 
    
{{
            Route::get( '/user/{id}/{name}', function( $id,$n){

                return $n;

            })->where(['id'=>'[0-9]+','name'=>'[a-z]+'] );
}}

`+ represents multiple characters`    


#  -----> Add a DRY code on every id 
If you have to add a specific regex to every routes that containing route parameter called id, then locate AppServiceProvider.php and add:

{{

    Route::pattern('id', [0-9]+ );
    // Now you dont have to specifically code on each route for id.
    
}}



#   ------> How to redirect Routes ?    
       
`NOTE: By default when we do redirect it returns a status code of 302.`    

{{    
    // SYNTAX:
    Route:: redirect( '/from', '/to' );

    // EX: 
    Route:: redirect( '.login', '/home', 301 );  //explicitly returned 301 status code. ( not necessary )

    // 2nd Approach: Instead use redirect() function
    Route:: get('user', function(){
        return redirect('/');
    }); 

}}


#    ------> Redirect Routes ?
     when anyone hits any route which do not defined then what should be done will tell by fallback callback.
       
 {{   
        Route::get('/', function() {
            return redirect('/posts');
        })
        //  Redirect to last page
        Route::get('/', function() {
            return redirect()->back();
        })
        // Redirect to another domain
        Route::get('/', function() {
            return redirect()->away('https://www.google.com');
        })
        // Redirect to another route
        Route::get('/', function() {
            return redirect()->route('posts.index');
        })
}}



#    ------> Fallback Routes ?
     when anyone hits any route which do not defined then what should be done will tell by fallback callback.
       
 {{   
        Route:: fallback( function(){
                return '<h1> ERROR 69 </h1>'
        });
}}




#    ------> Multiple methods same function ?    
    when we want to execute same function on a route with different methods, USE "match"
       

{{   
        Route:: match( ['get', 'post'], '/hue' function(){
                return '<h1> huehuehue </h1>'
        });
}}




    
#    ------> Any method call (get, post, put, patch, delete) one function only ?  USE "any"
       
{{   
        Route:: any( '/', function(){
                return '<h1> ERROR 69 </h1>'
        });
}}




#    ------> Creating a route for view
       ( suppose a view is already created for contact that having name contact.blade.php )

{{   
        Route:: get( '/contact', function(){
                return view( 'contact' );
        });
}}



         if( we have to just return an view only then we can also write it as :- )  
               {{      
                    Route:: view( '/contact', 'contact' );      
               }}



    

#        ------> Passing data from Routes to views 
       ( yeah! we can do server side rendering also, as we can see we can pass our data from routes to views in the form of key value pairs  )
    
 {{
        Route:: get( '/contact', function(){
                return view( 'contact', [ 'name' => 'dogesh', 'roll' => 21 ] );
        });
}}

         if( we have to just return an view only then we can also write it as :- )  
           {{
               Route:: view( '/contact', 'contact' , [ 'name' => 'dogesh', 'roll' => 21  ]);
           }}
        


`Note: If we want to pass only one pair then we can use an inbuilt function ->with( 'key', 'value' );`

{{

         Route:: get( '/contact', function(){
            return view( 'contact')->with( 'name', 'dogesh' );
         });
}}


#         -------> Access the data in view file
          
            <h1> hello {{ $name }} My Roll is {{ $roll }} </h1>6
    



## NAMED ROUTES

In this we learn :

    1. how to assign a unique name to a route.
    2. next we see, how we use that particular name in routes.


1) while creating the route, use a method called `name()` and in the parameter pass name value.

ex:-
{{
Route:: get( 'home', function(){
            return view('home'); 
        })->name( 'homeRoute' );
}}



2) Now we see how to use that named route using the name.

<ul>
    <li> <a  href={{ route('homeRoute')}} > Home </a> 
</ul>




-----------------------------------------------> Pass a value to the named routes 

Route:: get( 'contact/{_id}', function( $id ) {
    return view( 'contact' , ['cId' => $id ] );
})->name('contactRoute');

                | 
                |      ( By this way we can pass the data ) 
                v  
            
<ul>
    <li> <a  href={{ route('homeRoute', ['_id' => 69 ] )}} > Contact </a> 
</ul>




# Responses



# ----> Returning a response with status code, headers and cookie

{{

    Route::get('/posts', function() {
                     // response Text, status code
        return response( $posts, 201 );
    });

    Route::get('/posts', function() {
                     
        return response( $posts, 201 )
        ->header('Content-Type', 'application/json')         // set response header
        ->cookie('my_token', 'huehuehue', 3600 );            // set cookie details

    });

  `learn more about status codes :`  // https://developer.mozilla.org/en-US/docs/Web/HTTP/Status

}}


# ----> Returning a response with json data.

{{
    
    Route::get('/posts', function() {
        // sent json data with response
        return response()->json($posts);
    });

}}


# ----> Returning a response with a downloadable file.

{{
    Route::get('/posts/download', function() {

        // First of all put that file in the public folder then put the filepath in param of download method
        return response()->download(public_path('/filename.txt'));
    });
}}




# Grouping routes and add a prefix

{{

    Route::prefix('/posts')->name('post.')->group(function() {

        // create post routes here...

        Route::get('/', [PostController::class, 'index'] )->name('index'); 
        // route name = posts.index, path = /posts/

    });
}}



# Fetching the query parameters from uri

{{

    route::get('/students', function() {
        
        dump( request->all() ); //will dump out all the query params in form of an array
        
        dump( request->input('id') ) //will dump out only id from query params

        dump( request->input('id', 1 ) ) //will dump out value 1 if id is not passed in query

    })

    // request-> input() will look for all acceptable input sources like form, query, session.
    
    `if we want specifically search for query we can simply use :` request->query()
}}

must visit : `https://laravel.com/docs/10.x/requests#input`;


