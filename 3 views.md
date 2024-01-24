## =================== Views in Laravel ======>

Views directory is available in the rescource directory. 
Basically It contains your UI.

Create any file in this directory and it must have the same extension ==> 'name.blade.php'




---------------------->`We must store our views file in a structured way`.


like:-          rescources/views/`admin`                                          rescources/views/`users`
                                  |                                                               |  
                                  |                                                               |
                             `adminDashboard.blade.php `                                 `users.blade.php`
                                  
        its route code must be like:

        SYNTAX ->     Route:: method( 'URI', function() {    return view( foldername.filename )  }  );

{{
    //  EXAMPLE 1 ->   
       Route:: get( '/admin', function() {    return view( 'admin.adminDashboard' )  } );
     
    //  EXAMPLE 2 ->
        Route:: get( '/user', function()  {    return view( 'users.users' )  } ); 
}}    



----------------------> `Access the data in view file` 
          
            <h1> hello {{ $name }} </h1>


