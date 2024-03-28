#                                             Extra Ordinary Stuff 

//=> function to get value of a particular key inside an associative array.
`ex: $tasks is a associative array having id as one of the key and we have to find that task having 3 id`

{{
// $tasks defined above

$id = 3;                                                                                                   $
$task = collect($tasks)->firstWhere('id', $id );

}}



//=> the `created_at` and `modified_at` columns in the table. if I access them using Model then it gives us a date object and if we use diffForHumans() function, it will return the date like: `( 2 hours ago )`
{{
    $task = Task::findOrFail(2);                                                                                     $
    $task->created_at->diffForHumans();                                                                              $
}}


//==> 

{{
    public function destroy(Event $event, Attendee $attendee )
    {
        $attendee->delete();$
        // Status : 204 - No Content
        return response(status: 204);
        
    }
}}



//==> `Access the query params using query() function`
{{
    // Will return all the parameters that are passed using include tag
    $params = request()->query('include');$
}}



//==> `explode() function is used to convert a string to array, where a string is seperated by ',' ;`
{{
    // explode( seperator, array_name )
    $arr = explode( ',', $array );      
}}



//==> `array_map( 'fxn_to_convert', 'func' )`
{{
    // explode( seperator, array_name )
    $arr = array_map('trim', explode( ',', $array ));      
}}


//==> `in_array( arr, array ) checks arr value exists in array or not.`
{{
    // checks array consist $val value (return true) or not (return false)
    $val = in_array(  $val, $array_values );
}}



//==> ` with() works on top of queryBuilder instance while load() works on top of a model instance`
{{

    // QueryBuilder instance
    $user = User::query()$;
    $users = $user->with('relation');


    // Model instance
    function( User $user )
    {
        $users = $user->load('relation');
    }
}}


//==> `implode()` : opens an array to string with a seperator we passed in first parameter
{{
        $request->user()->employer()->jobs()->create([
            ... $request->validate([
                'title' => 'required|string|max:255',
                'location' => 'required|string|max:255',
                'salary' => 'required|numeric|min:5000',
                'description' => 'required|string',
                'category' => 'required|in:' . implode(',', Job::$category ),
                'experience' => 'required|in:' . implode(',', Job::$exp ),
            ])
        ]);
}}



//==> Difference between Lazy loading and Eager Loading :-
`https://medium.com/@kouipheng.lee/laravel-eloquent-lazy-vs-eager-loaded-803852c59e1c`