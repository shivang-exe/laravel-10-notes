#                                      API RESOURCE

We will see here API resource, It is a phenomenal scheme where we can apply filters while we return JSON data as API. suppose we have a model containing 3 attributes username, email and password. Now we want to create an API that returns the same set of user data but ensure the password must not be delievered, in such conditions we use API resource.

another use case of API resource is, for premium members you want to provide complete details and rest of all are same. We also can do so using API resource

dox- `https://laravel.com/docs/10.x/eloquent-resources#main-content`



# Step 1
cmd- `php artisan make:resource UserResource`


# Step 2
App\Http\Resource\ResourceName

I will get a resource file that looks like ->
{{
 
namespace App\Http\Resources;
 
use Illuminate\Http\Request;
use Illuminate\Http\Resources\Json\JsonResource;
 
class UserResource extends JsonResource
{

    public function toArray(Request $request): array
    {
        return [

            // here i have to write all of the attributes which i want to return with response
            'id' => $this->id,
            'name' => $this->name,
            'email' => $this->email,
        ];
    }
}

}}



# Step 3
Instead of returning the Model object return the model resource, like:
{{

    function index()
    {
        $user = User::find(1);
        return new UserResource($user);
    }

}}






# What if our we want to return collections ?

simple then, use Resource collection (huehuehue)


# Step 2.1 
cmd `php artisan make:resource UserResourceCollection`


# Step 2.2

App/Http/Resource/ResourceCollectionName

{{

 
namespace App\Http\Resources;
 
use Illuminate\Http\Request;
use Illuminate\Http\Resources\Json\ResourceCollection;
 
class UserResourceCollection extends ResourceCollection
{
    public function toArray(Request $request): array
    {
        // By default it takes the content from UserResource and return whatever is written
        return parent::toArray($request);
    }
}

}}


# Step 2.3

Instead of returning an array, return ResourceCollection
{{

   function index()
   {
      $users = User::all();
      return new UserResourceCollection($users);
   }

}}