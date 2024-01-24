#                                          REQUESTS 

Laravel provides us ability to create user defined requests used for authorisation and validating rules.
cmd: `php artisan make:request RequestName`

Location: App\Http\Requests\RequestName;

Example:1: `Checking our form data using user defined requests`

{{


# Request File
class TaskRequest extends FormRequest
{

    // Logic for authorisation
    public function authorize()
    {
        retrurn true;
    }


    // Assign your rules here
    public function rules()
    {
        return [
            'title' => 'required|max:255',
            'description' => 'required',
            'long_description' => 'required',
        ];
    }
}


# Controller file

use App\Http\Requests\TaskRequest;

function create( TaskRequest $req )                                                               
{

        // Will tell that rules are validated successfully and pass the form data to $data variable
        $data = $req->validated();

        $task = new Task;                                                                            $
        $task->title = $data['title'];
        $task->description = $data['description'];
        $task->long_description = $data['long_description'];

        $task->save();                                                                               $
}

                   |
                   |  (Mass assignment)
                   V  

function create( TaskRequest $req )                                                               
{
        $data = $req->validated();

        // create function accepts an assosiative array where keys are column attributes and values are table values and that's exactly $req->validated() returns us.
        Task::create( $data );                                                                 $
} 

function update( Task $task, TaskRequest $req )                                                
{
        $data = $req->validated();

        // update function accepts an assosiative array where keys are column attributes and values are table values and that's exactly $req->validated() returns us, it is same as create but works upon already existing data.
        $task->update($data);                                                                  $
} 

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
