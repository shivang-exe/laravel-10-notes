#                                   Grind With Database

`We can see all the required connection configurations are already written in config/database.php .`
# NOTE- Can change the credentials from the .env files

0) We can see in the default area-> we're providing mysql by default if another DB is not selected `lino:18`


# Running SQL QUERIES:
Once we had configured database connection we can run sql queries using DB facade.
namespace location: `use Illuminate\Support\Facades\DB;`



 
$SELECT$

Syntax: select( 'sql_query', [params] );
Examples:

{{

    // one
   $arr = DB::select( 'SELECT * FROM articles' );                                                      $

    // Two
   $arr = DB::select( 'SELECT * FROM articles WHERE id=?', [2] );                                      $

    // Three
   $arr = DB::select( 'SELECT * FROM articles WHERE id=:id', [':id'=>2 ] );                            $

}}

`Return : ` select function always return an array of results;




$INSERT$

Syntax: insert( 'sql_query', [params] );
Examples:

{{                                                  

    // Two
DB::insert( 'INSERT INTO articles(title, content) VALUES (?, ? )', ['Cheem', 'huehue'] );

    // One
DB::insert( 'INSERT INTO articles(title, content) VALUES (:title)', ['title'=>'huehue'] );

}}




$UPDATE$

Syntax: upadte( 'sql_query', [params] );
Examples:

{{

    // one
   $arr = DB::update( 'UPDATE articles SET title=:title WHERE id=:id',['title'=>'hue', ':id'=>1] );  $

    // Two
   $arr = DB::update( 'UPDATE articles SET title=? WHERE id=?',['hue', 1] );                         $

}}

`Return : ` The no. of rows is affected is returned by the update method.





$DELETE$


Examples:

{{

    // one
   $arr = DB::delete( 'DELETE FROM articles' );                                                      $

    // Two
   $arr = DB::delete( 'DELETE FROM articles WHERE id=?', [2] );                                      $

    // Three
   $arr = DB::delete( 'DELETE FROM articles WHERE id=:id', [':id'=>2 ] );                            $

}}

`Return : ` The no. of rows is affected is returned by the delete method.



$ TRANSACTIONS $

`If we want to execute multiple queries at the same time, then we can use transactions`
Working: Execute the statements if there's any error occured in between then it will rolled back to starting else if all statements executed succesfully then it will be commited to the database.

# MAINAINS ATOMICITY
Syntax: DB::transactions( function(){

    ... queries

});

Example:
{{

    DB::transaction(function(){

        DB::insert( 'INSERT INTO articles(title, content) VALUES (?, ? )', ['Cheem', 'huehue'] );

        DB::insert( 'INSERT INTO articles(title, content) VALUES (:title)', ['title'=>'huehue'] );
        
    })
}}


