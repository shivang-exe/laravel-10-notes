###                                         Caching


`Some of the data retrieval or processing tasks performed by your application could be CPU intensive or take several seconds to complete. When this is the case, it is common to cache the retrieved data for a time so it can be retrieved quickly on subsequent requests for the same data. The cached data is usually stored in a very fast data store such as Memcached or Redis.`

`Thankfully, Laravel provides an expressive, unified API for various cache backends, allowing you to take advantage of their blazing fast data retrieval and speed up your web application.`

SYNTAX:
{{
    use Illuminate\Support\Facades\Cache;
    $books = Cache::remember('cache_key', 3600, $book_data );

                        OR

    $books = cache()->remember('cache_key', 3600, $book_data );
    
    // 1st Param: It is a unique key which is directly mapped with the provided data. It is responsible for fetching out the data. It is automatically deleted after a certain time period provided in second parameter

    // 2nd param: It is the time given to tell till when we have to store this cached result

    // 3rd param: The data provided will be assosiated with the key. 
}}



# put( 'key', data, seconds | expiration time )

The put method is used to cache the data on the basis of its specific key.
Put method can also re-assign cache files.
{{
    // With seconds
    Cache::put( 'my_key', $data, 3600 );

    // With expiration time
    Cache::put( 'my_key', $data, now()->addMinutes(10) );
}}


# global cache helper: cache()
cache() is global method that can give us instance of cache Facade or directly cache files;
{{

    // directly cache files using seconds
    cache(['my_key' => $data], 3600 );

    // directly cache files using expiration time
    cache(['my_key' => $data], now()->addMinutes(30) );

    // returs cache facade instance
    cache()->put( 'my_key', $data, 3400 );

}}


#  remember( 'key', seconds, closure )  
Closure is same as callback in javascript.
Retrieve the data if key exists else save it with specified key and time.
{{

    cache()->remember( 'key', 3600, fn() => DB::table('users')->get() );

}}



# add( 'key', value, time  )
It only stores the file if it is already not existing in cache store, return true if added successfully else false.
can not re-assign.


# forever( 'key', value, time )
It is used to cache the file permanently, have to manually delete if we need to



# get('key', default_value )
It is used to access any cached value using its particular key. If the key do not exists then it returns default_value we passed in 2nd param and if we do not pass it then null will be returned.
{{
    $data = cache()->get('key', $default );
}} 




# has('key')
Check if a file exists in cache store or not.



# forget('key')
We can explicitly remove cache items by using forget() method by passing that key in 1st param


# pull('key')
Retrieve and delete together;

# flush()
clear all cache




##  EVENTS TRIGGER AUTOMATICALLY ON DB OPERATIONS

We are storing the results on the cache and on the time we are returning the same but what if we update or delete any particular model? still we will se those cached results.

In this case laravel will emit events that we can handle in model.

{{

class Book extends Model
{
    use HasFactory;

    // Can resolve the models inside a protected + static function called booted

    protected static function booted()
    {

        // Update Event
        static::updated( fn( Book $book) => cache()->forget( "key:". $book->id ) );

        //Delete Event
        static::daleted( fn( Book $book) => cache()->forget( "key:". $book->id ) );

        // view all events: https://laravel.com/docs/10.x/eloquent#events
    }
}


`however this will not be triggered everytime we do updates on database. What does that mean? `
` Let say we are updating the data manually to the database directly like using DB FACADE or Query Builder or Raw SQL basically all those that not include Eloquent way to update models, in this case the events will not be triggered`


}}

