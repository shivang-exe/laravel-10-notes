#                                  Rate Limiting 

Rate limiting is a concept in laravel that enable us to set certain the no of requests must be fullfill in a given time period, if the requests will excceed the limit user will got a 429 Error, ( Too many requests ).


# Step:1 

head over to-> `App\Http\Providers\RouteServiceProvider.php` 
Inside the boot function create a limiter:

            RateLimiter::for('three-visits', function (Request $request) {
            return Limit::perMinute(3)->by($request->user()?->id ?: $request->ip());
           });

{{
    `we can use various parameters to assign no of requests`

    // perMinute(n)
    return Limit::perMinute(3)->by($request->user()?->id ?: $request->ip());

    //perHour(n)
    return Limit::perHour(3)->by($request->user()?->id ?: $request->ip());

    //perDay(n)
    return Limit::perDay(3)->by($request->user()?->id ?: $request->ip());

}}



{{
    `Assign to the routes`
    Route::middleware('throttle:three-visits')->get('/home', [HomeController::class, 'index']);


    `Assign directly to Home Controller`
    public function __construct()
    {
        $this->middleware('throttle:three-visits')->only(['index']);$
    }
    
}}