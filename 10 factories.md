#                                      Factories

Factory is directly related to our table and is used to add dummy data easily.

cmd: `php artisan make:factory FactoryName`
cmd: `php artisan make:factory FactoryName --model=ModelName` (Creating Factory for a specific Model)


In the database/factories : We will get our factory file, now inside a definition function we need to return our table data according to table schema.

{{

    public function definition()
    {
        return [
            'name' => fake()->name(),
            'email' => fake()->unique()->safeEmail(),
            'email_verified_at' => now(),
            'password' => fake()->password(),
            'remember_token' => Str::random(10),
        ];
    }

}}

Now to execute this factory we need a seederFile.

{{

    use App\Model\Student;
    
    public function run()
    {
      //Model::factory()->count(no_of_entries)->create()
        Student::factory()->count(10)->create();
    }
}}