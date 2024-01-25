#                                           QUERY BUILDER


Usually we query our database using pure DB class, but laravel provide us a shorthand through which we can query data without writing the raw SQL.

Enable us to query the database without writing raw SQL.

Query Builder internally uses PFO parameter binding to protect our application against SQL injection attack.

# Destructuring Query builder

We start our query using `table()` method given by DBfacade.

Table method returns a query builder instance for a given table allowing us to chain more constraints into the query. like WHERE, GROUP_BY etc

ex: 
{{

    use Illuminate\Support\Facades\DB;

    // get() -> returns all rows from the table
    $student = DB::table('TableName')->get();                                                                    $

}}



$ RETRIEVE A SINGLE ROW FROM A TABLE $
 

{{
    
    // use`first()` method to fetch the very first row from result of a query.
    $student = DB::table('student')->where('city', 'bokaro')->first();                                             $



    // use `value( col_name )` method to fetch the very first row attribute of a query.
    $student = DB::table('student')->where('city', 'bokaro')->value('name');                                        $



    // use 'find( primary_key )' method to find that row having provided primary_key
    $student = DB::table('student')->find(2);                                                                       $

}}





$ RETRIEVE ALL ROWS FROM A TABLE $
{{


    // use `get()` to return all rows of data
    $student = DB::table('student')->get();                                                                         $
    

 
    // use 'pluck( col_name )' to get List of all column values
    $student = DB::table('student')->pluck('city');                                                                 $    
    // ( can create the result an assosiative array by passing two columns in the parameter )




    // Whenever we have a lot of data in our database then instead of acquring all data at once we acquire data in chunks so that our database must not be overloaded.

    // here orderBy is must, without it won't work

    //chunk( noOfRows, fn ): noOfRows are the no of row elements that exists in a chunk, that chunk is recieved in fn 
    $students = DB::table('student')->orderBy('id')->chunk( 5, function( $students ){ .. })                                             $



}}



$ AGGREGATES $ 

{{

// Counts no of rows
$student = DB::table('student')->count();                                                                            $

// returns maximum from all rows
$student = DB::table('student')->max('marks');                                                                       $

//  return min from all rows
$student = DB::table('student')->min('marks');                                                                       $

// determine if record exists
if( DB::table('student')->where('id',2)->exists() ){  ..will execute if true  }                                      $

// determine if record exists
if( DB::table('student')->where('id',2)->doesntexists() ){  ..will execute if true  }                                $


}}




$ SELECT FROM TABLE $

{{

// use select( [cols] ) method to get all values of a specified colm
$student = DB::table('student')->select('name', 'email');                                                              $


// use distinct() method to get all unique values
$student = DB::table('student')->distinct()->select('name', 'email');                                                  $


// use where( 'col_name', relational operator,  value_expected ) method to apply conditions
$student = DB::table('student')->where('id', '=', 3)->select('name', 'email');                                         $


// use orWhere( 'col_name', relational operator,  value_expected )  A | B 
$student = DB::table('student')->where('id', '=', 3)->orWhere('name', '=', 'harsh')->get();                            $


// use whereBetween( 'col_name', [ @range ] )  returns all the rows that having col_name value in between the range
$student = DB::table('student')->whereBetween('marks', [70,100])->select('name', 'email');                             $


// use whereDate( 'col_name', 'yyyy-mm-dd' ) returns all the rows that having equal dates. 
$student = DB::table('student')->whereDate('order_date', '2023-12-21')->select('name', 'email');                       $


// use whereMonth( 'col_name', 'mm' ) returns all the rows that having equal dates. 
$student = DB::table('student')->whereMonth('order_date', '12')->select('name', 'email');                              $


// use whereDay( 'col_name', 'dd' ) returns all the rows that having equal dates. 
$student = DB::table('student')->whereDay('order_date', '12')->select('name', 'email');                                $


// use orderBy( colm_name ) to sort the data ASC or DESC
$student = DB::table('student')->orderBy('marks')->select('name', 'email');                                            $
$student = DB::table('student')->orderBy('marks', 'desc')->select('name', 'email');                                    $



// sort data according to the date -> latest( col ) : will return the latest inserted data first
$student = DB::table('student')->latest()->select('name', 'email');                                $

// sort data according to the date -> oldest( col ) : will return the oldest inserted data first
$student = DB::table('student')->oldest()->select('name', 'email');                                $


  
}}




$ GROUP BY  AND  HAVING  $
//config/database.php => inside mysql invert 'strict' to false;
{{

$students = DB::table('table')->groupBy('marks')->having('marks', '>', 90 )->get();                           $

}}



$ LIMIT AND OFFSET $
{{

// limit can be refered as take(): end after taking 5 rows
$students = DB::table('table')->take(5)->get();                                  $
$students = DB::table('table')->limit(5)->get();                                  $


// offset can be referred as skip(n) : Skip n rows and start from 6th
$students = DB::table('table')->skip(5)->take(6)->get();                           $
$students = DB::table('table')->offset(5)->limit(6)->get();                           $


}}






$ INSERT DATA $

{{

    DB::table('student')->insert([ 
        'name' => 'shivang',
        'age' => 20
     ],
     [  
        'name' => 'harsh',
        'age' => 20
     ]);


   // If any rule is voilated then it will be ignored else inserted
    DB::table('student')->insertOrIgnore([ 
        'name' => 'shivang',
        'age' => 20
     ],
     [  
        'name' => 'harsh',
        'age' => 20
     ]);


    // If any rule is voilated then it will be ignored else inserted
    $var = DB::table('student')->insertGetId([                                                     $
        'name' => 'shivang',
        'age' => 20
     ],
     [  
        'name' => 'harsh',
        'age' => 20
     ]); 
}}





$ UPDATE DATA $

{{

    $affected_rows = DB::table('student')->where('id', 2)->update([                                  $
        'name' => 'Raj',
         'age' => 21
    ]);

    $affected_rows = DB::table('student')->where('id', 2)->updateOrInsert(                               $

        ['name' => 'Raj'],             // [ which one update()  ]
        ['name' => 'Shivang']          // [  what to update ]

    );
}}



$ DELETE DATA $

{{
    // delete() to delete
    DB::table('student')->where('id', 12)->delete();

    // delete a table
    DB::table('student')->truncate();
}}


