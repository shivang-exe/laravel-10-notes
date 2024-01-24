##                                TESTING IN LARAVEL

When our application grows it becomes hard to check the previous features of application are working well. So its better to write the test cases hand to hand while building application.


In laravel testing is divided in two parts:

1. Unit Test:
The code is divided into small isolated parts and are tested. 
Testing the smallest units in our app. 
ex: class, fxns

2. Feature Test:
Focuses on the complete feature rather than individually test.

lets take an example 

Consider a Post creation form :-
Feature testing deals with whether the post operation is actually created in the database if so fetch it and check whether the title and body is correctly inserted or not, while unit testing deals with the input from the user, it must be string, a valid date.


Reliability:

Manual testing: ? ( depends upon how well the tester understands the code )

Unit testing: If all the fxns are working correctly even though there is no guarantee that it will work also  well together as group. 

Feature testing: Has a great reliability.

End-End: Highest reliability



Speed:

Manual testing: snail

Unit testing:  fast

Feature testing: slow

End-End: very slow



`Laravel uses php unit as its main testing library`.

Creating a test file ->
cmd: `php artisan make:test TestName` will create a feature test file
cmd: `php artisan make:test --unit TestName` will create a unit test file

Execute test file ->
cmd: `./vendor/bin/phpunit`                   will test all test cases
cmd: `./vendor/bin/phpunit --filter=TestName` will test only provided test




#                                 UNIT TESTING

suppose we are testing a controller having functions like  ->  create(),       store(),       update()     
then we will create thier respective unit test in this way ->  test_create(),  test_store(),  test_update() 


# steps taken while creating test

postControllerTest.php

{{

    public function test_create()
    {
        // 1. Define the goal
            // use comments

        // 2. Replicate the env/restriction
            $repo = $this->app->make(PostController::class);

        // 3. Source of truth
            payload = [
                'cheem' => 'huehuehue'
            ]

        // 4. Compare
        $result = $repo->create(payload)
        $this->assertSame($payload['cheem'], $result->cheem );
    }
}}





#                                 FEATURE TESTING

Feature testing like testing with our APIs. LETS SAY WE TEST ON POSTS

{{

    // this trait will re-migrates the data everytime the any test will executes
    // slow down tests, therefore always use in-memory sql-lite 
    // this db will always need this below trait because everytime we have to create tables. as after all tests they will flashed away
    use RefreshDatabase;

    public function test_index()
    {
        // Load data in db
        $posts = Post::factory(10)->create();$
        $postIds = $posts->map( fn($post) => $post->id ); // map returns a collection

        // call index endpoint using get method
        $response = $this->json('get', '/posts');

        // assert status
        $response->assertStatus(200);$

        // verify records
        dump($response->json() );
        $data = $response->json('data'); // will return only data
        collect($data)->each( fn($post) => this->assertTrue(in_array( $post->id, $postIds->toArray() )) );
        // $data = $response->json('data.19'); // will return only data having index 19
    }

    public function test_show()
    {

        $post = Post::factory()->create()$;

        $response = $this->json('get', '/posts{{$post->id}}');

        $response->assertStatus(200);$

        $data = $response->json('data');
        $this->assertEquals( $data[id], $post->id , 'Response ID are not same');
    }


}}



// Live reload php - phpunit watcher




# TDD INTRO :                            TEST-DRIVEN-DEVELOPMENT 

 motto - `Write tests before code`.

It will give idea to think, how our app will behave before we dive to code.
It is having a good history and it drastically reduces the bugs in code.