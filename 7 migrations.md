##                                             MIGRATIONS

`Apart from creating database schema it is having a vast usage.`
HOW? - Migrations are like version control for our databse, allowing a team to share and use the same schema.

Why i referred version control here?
Its because, let say for any reason any wrong query is commited. thereby with help of Migrations we can rollback it too.                 


cmd : `php artisan make:migration create_TableName_table`


# path => database/migrations/...

Inside there a new migration has been created that is returning a class extending migration class.
There we got two functions `up` and `down`:

-> up{
    The up() function is used to add new tables, columns or indexes to database;
}


-> down(){
    The down() function reverse the operations performed by `up` function. (Rollback function)
} 


<a Suppose if we have to run a create database query, then in the up() we write the code to create a table and in the down() table we'll write the code to drop the same table > 


Migrate cmd:  `php artisan migrate`

Check status which file is migrated cmd: `php artisan migrate:status` 

EVERYTIME WE MIGRATE THE BATCH NUMBER IS INCREMENTED BY 1. 
( this migrate data is stored inside laravel database automatically, that's how we able to check status and perform rollback operations )





$ ROLLBACK MIGRATIONS $

Till now we had seen how to migrate the files and create schema, let now see how we can rollback the same
NOTE: `rollback cmd rollbacks all the migrations of previous batch`.

rollback cmd: `php artisan migrate:rollback`

If we want to rollback last 5 migrations then we can do so using `--step=5` tag
final cmd: `php artisan migrate:rollback --step=5`

Migrate all commands use reset
final cmd: `php artisan migrate:reset`

Rollback and Migrate Together
final cmd: `php artisan migrate:refresh`

Migration restart along with seeding
final cmd: `php artisan migrate:refresh --seed`






$ CREATING TABLES $
We can create tables using  `create`  function.

cmd: `php artisan make:migrate create_tableName_table`

SYNTAX: 
Schema::create('table_name', function( Blueprint $ref){

    // columns...

});

Example:
{{
    Schema::create('students', function( Blueprint $table){

        $tabe->id();
        $table->string('name');

    });
}}





$ DROPING TABLES $
We can Drop tables using drop() fxn or dropIfExist() fxn.

cmd: `php artisan make:migration dropping_tableName_table`

SYNTAX AND EXAMPLE:
{{

Schema::drop('tableName');
        OR
Schema::dropIfExists('tableName');

}}






$ CHECKING TABLES AND COLUMNS $
We can check existence of a table and a column using hasTable([param]) and hasColumn([params]) respectively;

{{
    if( Schema::hasTable('tableName') && Schema::hasColumn('columnName') )
    {
        // both returns a boolean value
    }  
}}






$ RENAMING A TABLE $
{{
    // create a migration file
    Schema::rename('tableName', 'TableName');
}}






$ CREATING A COLUMN $

{{

// inside a create function
Schema::table('TableName', function( Blueprint $table){

    //creating columns
    $table->id();
    $table->string('ColumnName');

})

//Available Column Types

id()

timestamps()

increments('col')

integer('col')

char( 'col', length)

float('col_name', total_digit, decimal_digit )

double('col_name', total_digit, decimal_digit )

decimal('col_name', total_digit, decimal_digit )

boolean('col_name')

string('col_name')

text('col_name')

date('col_name')




}}



$ ADDING A NEW COLUMN TO ALREADY EXISTING TABLE $

cmd: `php artisan make:migration add_column --table=TableName`

{{
    Schema::table('TableName', function( Blueprint table){ 
        table->integer('new_col');
    })
}}





$ DROPPING A COLUMN TO ALREADY EXISTING TABLE $

cmd: `php artisan make:migration drop_column --table=TableName`

{{
    Schema::table('TableName', function( Blueprint table){ 
        table->dropColumn([column_names]);
    })
}}







$COLUMN MODIFIERS$

Add column constraints. 

->autoIncrement();
->default($value);
->nullable();    


EXAMPLE
{{

    Schema::create('TableName', function( Blueprint $table){                                              $
        $table->integer('id')->default(10);                                                               $
    })
}}