###                                  MOST AWAITED: M O D E L S

# ORM - OBJECT RELATION MAPPING

Whatever is table in database is model here. using that model we can leverage different sorts of db operations without quering with database.

like if we want all data from the table, we simply code: `$data = Model::all();`

Models are directly related with the tables. They are having one-one relationship that means for each table its model is also defined.

// Creating an model 
cmd: `php artisan make:model ModelName`

cmd: `php artisan make:model ModelName -m` ( Create model along with its migration file )
cmd: `php artisan make:model ModelName -f` ( Create model along with its factory file )
cmd: `php artisan make:model ModelName -s` ( Create model along with its seeder file )
cmd: `php artisan make:model ModelName -c` ( Create model along with its controller file )

cmd: `php artisan make:model ModelName -mf` ( Create model along with its migration & controller file )
...

cmd: `php artisan make:model ModelName -mfsc` ( gives all ) 



MODEL STRUCTURE:
{{
  
namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;                 

class Student extends Model
{
    use HasFactory;

    // Change your database
    protected $connection = 'mysql';

    // want to change the table name
    protected $table = 'students';

    // Change your primary_key ( by default it is id )
    protected $primary_key = 'roll_no';

    // Change your key type ( by default it is id )
    protected $keyType = 'string';

    // set incrementing false
    public $incrementing = false;

    // set timestamps false
    public $timestamps = false;
}

}}





# Access data from Models

// Inside the Controller import the model namespace;
{{

    use App\Models\ModelName;

    // The static function all() returns all the data inside the model 
    $data = ModelName::all();                                                                                $

    // The static function find( _id_ ) takes primary key and return its data
    $data = ModelName::find($id);                                                                              

    // The static function findOrFail( _id_ ) takes primary key, if (exists) return data else return error4O4
    $data = ModelName::findOrFail($id);          
    
}}

`REMARK:  php artisan tinker will provide you a shell to execute these queries for better development exp`




# Route Model Binding

Laravel gives us superpower to bind the route parameters directly with models. What does that mean?

Most of the time we pass id in the route which is a primary key of any model instance and then using that id we find its correct instance using `find()` | `findOrFail()`. 

Route Model binding provides us opportunity to do it in a single move.

{{

    Route::get('/tasks/{id}/edit' , function($id){
        $task = findOrFail($id);
    });

    
    Route::get('/tasks/{task}/edit', function( Task $task){
        // now don't need to use any fxn, we got our instance in $task variable automatically
    })


    // If we pass an id that does'nt exist then as usual we will be redirected to ERROR 4O4 page.
    // in the route still we have to pass the id.
}}



# IMPORTANT: To enable mass assignment feature we need to explicitly enable it in the respective model.

{{

class Task extends Model
{
    use HasFactory;

    // Here we change a property fillable and assign all the required column names which we want to fill using create and update;
    protected $fillable = [ 'title', 'description', 'long_description' ];


    // likewise we have $guarded that is opposite of fillable, instead of these allow all to fill
    protected $guarded = [ 'password', 'id'];  // ( Not recommended ) 
}

}}



#    add a local query using scopeAttribute

{{

    use Illuminate\Support\Eloquent\Builder;
    class Student extends Model 
    {
        use HasFactory;

        // here scope prefix is mandatory
        public function scopeName( Builder $query, string $title )
        {
            return $query->where('name', 'LIKE', '%{$title}%' );
        }
    }

    Student::name('shivang')->get();
    // Above query is same as = Student::where('name', 'LIKE', '%{$title}%' )->get();      
}}