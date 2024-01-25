#                           AUTHORISATION - GATES & POLICIES

Laravel provides simple mechanism for authorisation using gates and policies.

dox : `https://laravel.com/docs/10.x/authorization#main-content`

# Uncovering- GATES

$ STEP: 1 $
First of all go to `App\Providers\AuthServiceProvider.php` and add a line ->

{{
use Illuminate\Support\Facades\Gate; 
}}

$ STEP: 2 $
Now inside the boot function, you have to define your gate ->

{{

    public function boot()
    {
        $this->registerPolicies();$

        // Gate::define('ability_name', closure )
        Gate::define( 'isAdmin', function($user){

            // $user is pointing to the user which is logged in currently.

            if( $user->email === 'admin@123' ) {
                // For true the user is authorised
                return true;
            }
            else {
                // For false the user is not authorised
                return false;
            }
        });
    }
}}


$ STEP: 3(a) $
Apply the gate in the routes for validations.

{{

    Route::get('/posts', [PostController::class, 'index'] )->middleware(['can:ability_name']);
    ex:- Route::get('/posts', [PostController::class, 'index'] )->middleware(['can:isAdmin']);
}}




$ STEP: 3(b) $
Apply the gate in the blade files.

{{
    @can('isAdmin')
     
     ... 
    (if isAdmin ability is fullfilled by user then the code btw this directive will be displayed else not)

    @endcan
}}




$ ADD-ON $
instead of code your buisness logic inside boot func, create a seperate file under:

-> File location :- `App\Gates`
-> Your file name and class name must be same.

{{

// ex: adminGate.php

namespace App\Gates;

class AdminGate
{

    public function check_admin( $user )
    {
            if( $user->email === 'admin@123' ) {
                return true;
            }
            else {
                return false;
            }
    }

}

// utilise in AuthServiceProvider.php

use App\Gates\AdminGate;

Gate::define('isAdmin', [AdminGate::class, 'check_admin'] );


}}






To authorize an action using gates, you should use the allows or denies methods provided by the Gate facade. `Note that you are not required to pass the currently authenticated user to these methods. Laravel will automatically take care of passing the user into the gate closure.` 

{{

    public function update(Request $request, Post $post): RedirectResponse
    {
        if (! Gate::allows('update-post', $post)) {
            abort(403);
        }

        OR

        $this->authorize('update-post', $post);
 
        // Update the post...
 
        return redirect('/posts');
    }


}}


dox-point: `https://laravel.com/docs/10.x/authorization#authorizing-actions-via-gates`








# Uncovering-> Policies

policies are classes that organize authorisation logic around a particular model or resource.

cmd- `php artisan make:policy modelPolicy --model=ModelName(opt.)`
ex:  `php artisan make:policy PostPolicy --model=Post`


once it has been created it must be registered in:
App\Providers\AuthServiceProvider.php contains a policies property that maps your models with policies.
( Only if you do not follow the laravel standard convention )

{{

        protected $policies = [
        ModelName::class => PolicyName::class {} 
        ];

}}

After running cmd. You will get an Policy file where we have to code the functions same as we do in gates.


Route calling:
{{                                                                        fxn name
                                                                              |
                                                                              V
Route::get('/posts',[Controller::class, 'index'])->middleware(['auth', 'can:isAdmin', Model]);

}}

Blade Rendering

{{
@can( 'methodName', Model instance)

@cannot
}}


If i am using standard mechanism like resource controller, then laravel will create each authorisation action for us and each action is already mapped.

link - `https://laravel.com/docs/10.x/authorization#authorizing-resource-controllers`

The following controller methods will be mapped to their corresponding policy method. When requests are routed to the given controller method, the corresponding policy method will automatically be invoked before the controller method is executed:


Controller Method	   Policy Method

index	          -       viewAny
show	          -       view
create	          -       create
store	          -       create
edit	          -       update
update	          -       update
destroy	          -       delete


To achieve that i have to add a method in Controller constructor
{{


class PostController extends Controller
{

    public function __construct()
    {
        $this->authorizeResource(Post::class, 'post');$
    }
}


}}





SIMPLY USE POLICIES INSIDE CONTROLLERS

{{

    link: `https://laravel.com/docs/10.x/authorization#via-the-user-model`
}}




