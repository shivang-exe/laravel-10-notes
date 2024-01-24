#                                  ELOQUENT Relationships



# One to One Relationships
We use `hasOne()` method to define a one-one relationships in between models.    

[ @params ] 
hasOne( ModelName::class, 'foreign_key', 'primary_key' );

Let suppose there are two models. (a) `Customers`   (b) `Mobiles`

{{

    // Mobile Migration
    public function up()
    {
        Schema::create('mobiles', function (Blueprint $table) {
            $table->id();
            $table->text('mobile_name');
            $table->unsignedBigInteger('customer_id');
            $table->foreign('customer_id')->references('id')->on('customers');
            $table->timestamps();$
        });                         
    }


    // Customers Model
    use App\Models\Mobile;

    class Customer extends Model
    {
        use HasFactory;
        public function mobile(){
            return $this->hasOne(Mobile::class);
        }
    }

    // Mobiles Model
    use App\Models\Customer;

    class Mobile extends Model
    {
        use HasFactory;
        public function mobile(){
            return $this->belongsTo(Customer::class);
        }
    }


    // Controller

    //Insert data
    function addData()
    {
        $mobile = new Mobile;
        $mobile->mobile_name = 'Nokia';

        $customer = new Customer;
        $customer->name = 'cheems';

        $customer->save();
        $customer->mobile()->Save($mobile);
    }


    //Access data
    function get()
    {
        // Access mobile data from customers
        $mobile = Customer::find($id)->mobile; 

        // Access customers data from mobile
        $customer = Mobile::find($id)->customer;


    }

}}





# One to Many Relationships

Let Suppose there are two models (a) Author   (b) Post
There is One to Many relationship from Author to Post.


{{

    // Migration file of Post
    public function up()
    {
        Schema::create('posts', function (Blueprint $table) {
            $table->id();
            $table->string('title');
            $table->string('category');
            $table->unsignedBigInteger('author_id');
            $table->foreign('author_id')->references('id')->on('authors')->onDelete('cascade')$;
            $table->timestamps();$



            $table->foreignId('author_id')->constrained()->cascadeOnDelete();$

            use App/Models/ModelName;
            $table->foreignIdFor(ModelName::class);$

        });
    }


    // Model file of Author

    use App\Models\Post;
    class Author extends Model
    {
        use HasFactory;
        public function post(){
            return $this->hasMany(Post::class);
        }
    }



    // Model file of Post
    use App\Models\Author;
    class Post extends Model
    {
        use HasFactory;
        public function author(){
            return $this->belongsTo(Author::class);
        }
    }

}}




# has One Through Relationship

A relationship that includes two tables.
ex: 
 
There is a Mechanic table, a Car table and a Owner table.
Mechanic is related to car, Owner is also directly related with Car therefore with help of Has One Through we can also directly relate Mechanic with Owner.


{{

    // Mechanic Model
    use App\Models\Car;
    use App\Models\Owner;

    class Mechanic extends Model
    {
        use HasFactory;
        public function owner(){
            return $this->hasOneThrough( Owner::class, Car::class);
        }
    }

}}






#  Using some inbuilt Eloquent methods to fetch the related table information.
{{

`Count the no. of entries linked with the particluar tuple`
use withCount() method 

\App\Models\Book::withCount('reviews')->get();
// a new column is added with name of reviews_count where we can see the count of each row
// here we will get to know how many reviews is related with each book;




}}

