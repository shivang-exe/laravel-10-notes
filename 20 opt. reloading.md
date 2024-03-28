#                             OPTIONAL RELOADING

With the help of laravel we can enable optional reloading in our REST API using query param

# Step 1: 
Fetch out all the relationships using query param and convert it to array

{{

    protected function shouldIncludeRelation( string $relation )
    {
        $include = request()->query('include');

        if( !$include ){
            return false;
        }

        $relations = array_map('trim', explode(',', $include) );

        // returns boolean
        return in_array($relation, $relations );
    }

}}


# Step 2:
Add the logic in your controller 

{{

    public function index()
    {

        $query = Event::query();

        $relations = ['user', 'attendee' ];

        foreach( $relations as $relation )
        {
            $query->when(
                $this->shouldIncludeRelation($relation),
                fn( $q ) => $q->with($relation)
            );
        }

        return EventResource::collection( $query->get() );
    }

}}

# Step 3
Check the same in Thunderclient by passing query param ex: `{{URL}}/events/?include=user,attendee`


