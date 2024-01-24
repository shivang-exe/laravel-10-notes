#                                                FORMS 

-> CREATING FORM :

    In the blade file simply create an form using plain HTML
    ex: 

    <form method="post" action="/user" >
       @csrf
       <input />
       <input />
       <input />
    </form>

    All the things are fine but what is @csrf here?
    -> Cross-site request forgery is an attack where an unauthorized cmd is injected on behalf of a authenticated user.
    here, @csrf directive saves us from this attack.











-> ACCESSING FORM :

    In this controllers use Request object to access the data.
    ex:
{{
    function acceptData( Request $req )
    {




        // Print all the data we got from Form 
        print_r($req->all());               ||      print_r( $req->input() );


        // Print a single data on the basis name attribute
        print_r( $req->input('name') );     ||      print_r( $req->name );







        // Check if attribute exists?
        if( $req->has('attr') )
        {
            print_r( $req->attr );
        }

        // also if all are exists then
        if( $req->has( ['attr1', 'attr2'] ) )
        {}

        // also if any one of them exists then
        if( $req->hasAny( ['attr1','attr2'] ) )
        {}








        // Check if attribute is missing?
        if( $req->missing('attr') )
        {
            print_r( 'Attr is missing');
        }







        // If we want to store our data in session then we are having [ $req->flash() ]

        STORE ALL DATA         : $req->flash();

        STORE ONLY SINGLE DATA : $req->flashOnly('attr');

        STORE MULTIPLE DATA    : $req->flashOnly( ['attr1','attr2'] );

        STORE EXCEPT           : $req->flashExcept('attr');

        ACCESS ALL DATA        : $req->old();

        ACCESS SPECIFIC DATA   : $req->old('attr');










        // Redirect to another page along with flashing the data to session


        PREREQUISITES: name that route which you want to redirects to.

        return redirect('/oldform')->withInput();

        In the oldform file -> the data can be accessed in such a way 
        `
            {{ namedRoute('attr_name') }}
        `

        // Additional Info: Flash except some attributes 
        return redirect()->route('namedRoute')->withInput(  $request->except('attr_name')  );


    } 


}}

#  --> VALIDATING FORM:

Once the form is submitted we can validate the form data using `validate()`. It accepts an associative array where key is the 'name' attribute and value are the rules. Rules can be multiple, if so then they are initialised in a array or we can use pipe symbol.

{{
    $data = $request->validate([
        'name' =>  'required'|'max:255' ,
        'description' => ['required', 'min:20'],
        'email' => 'required'|'email'
    ]);

    // check out all rules here
    rules_url = 'https://laravel.com/docs/10.x/validation#available-validation-rules';
}}

`How to get error if the validations are not fullfilled`
By using {{ $errors }} we'll get our errors if so.




#  --> SAVING FORM DATA:

Inside the controllers after validation we can perform our save operations.

{{

    use App\Models\Task;

    function saveMyData()
    {
        // 1st perform validations
        $data = $request->validate([
        'name' =>  'required'|'max:255' ,
        'description' => ['required', 'min:20'],
        'email' => 'required'|'email'
        ]);

        // 2nd save data
        $task = new Task;                                                                            $
        $task->name = $data['name'],
        $task->description = $data['description'],
        $task->email = $data['email'],

        $task->save();                                                                                $
        return redirect()->route('tasks.show', [ 'id' => $task->id ] );                               $
    }

}}





# save the form data using model

{{

use App\Http\Requests\TaskRequest;
use App\Models\Task;

function save( TaskRequest $req )
{
    $data = $req->validated();

    // make sure you enabled mass assignment
    Task::create( $data );
}

}}



# update the form data using model

{{

use App\Http\Requests\TaskRequest;
use App\Models\Task;

function update( Task $task, TaskRequest $req )
{
    $data = $req->validated();

    // make sure you enabled mass assignment
    $task->update( $data );
}

}}




# delete the form data using model

{{

// controller file
use App\Models\Task;

function delete( Task $task )
{
    $task->delete();                                                                            $
    return redirect()->route('tasks.index');
}


// Route file
Route::delete('tasks/{task}', [ TaskController::class, 'delete'] );

}}