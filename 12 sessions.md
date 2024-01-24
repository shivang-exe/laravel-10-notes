#                                                 SESSIONS 

Session is a powerful way to have a healthy convo between website and user. Session stores data of the user temporarily. It starts when user visits the website and ends when browser is closed.


`FLASH messages in sessions`
Before we dive into the example, let’s take a moment to grasp the concept of flash messages in Laravel 10. Flash messages are short-lived messages that are stored in the session and are typically used to display success or error messages after specific actions are performed. They provide a smooth way to communicate with users, making their experience on your web application more interactive and delightful.

{{

use Illuminate\Http\Request;

use Illuminate\Support\Facades\Validator;

public function exampleFlashMessage(Request $request)

{

    // Validate the incoming request data
    $validator = Validator::make($request->all(), [

        'name' => 'required|string|max:255',
        'email' => 'required|email|max:255',
        'message' => 'required|string|max:1000',

    ]);

    // Check if the validation fails

    if ($validator->fails()) {

        // If validation fails, flash the error messages to the session
        $request->session()->flash('error', 'Oops! There are some errors in your form submission.');          $

        // Redirect back to the previous page with the old input
        return redirect()->back()->withInput();
                 OR
        return redirect()->back()->with('error', 'Oops! There are some errors in your form submission.');

    }

    // If validation passes, continue with your desired action
    // For example, saving the form data to the database
    // Perform some action here...

    // After successful action, flash a success message
    $request->session()->flash('success', 'Form submitted successfully!');                                   $

    // Redirect back to the previous page or any other route
    return redirect()->back();

}



// BLADE FILE

@if(session('success'))

    <div class="alert alert-success">

        {{ session('success') }}

    </div>

@endif

@if(session('error') )

<div class="alert alert-danger">

        {{ session('error') }}

    </div>

@endif



// Extra
route('abc')->with('key', 'value'); // creates a session flash 

}}



# If we want to flash all the form data then it is documented in 11 form.md

When we are redirected to that page, we see a flash message of either `success` or `error` but if we refresh that page it will fly away (short-lived).










#                         ERRORS

Handling errors using sessions is also easy in laravel

<form>
@csrf

<input id='title' name='title' />

@error('title')
   <p> Wrong input </p>
@enderror;

</form>