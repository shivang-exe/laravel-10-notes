#                                                 Components.

Thinking like React Components. as the definition states that they are something which can be reusable once created.

here there are two approaches available for writing components:

1-> Class Based Components
2-> Anonymous Components



-------------------------------------------------------> Create command for components
#CLASS BASED Components

-> create components using : `php artisan make:component component_name`

-> `php artisan make:component dir_name/component_name`    ( if we want to add this file inside a directory, then use this cmd)


NOTE- ` AFTER CREATION WE GET TWO FILES (A) Blade file ( rescources/view/ )  (B) PhP file (App/view/components/)`




--------------------------------------------------------> Rendering Components

In the blade file just use    ->       <x- component_name >
if it was in a directory then ->       <x- directory_name.component_name >







----------------------------------------------------------> Passing Data to Components

if the value is hard-coded then we can pass it simply by assigning to a html attribute in <x- ... /> tag

syntax and ex:-        <x-card title="Cheem Card" />

if the value is an PhP expression then we had to make slight changes in the html attribute

syntax and ex:-        <x-card :description=$des />


----------> Definig the data in the component in its class constructor present in App/View/Component/component_name

define the attributes first and add them to the constructor.


----------> Display the component data in the blade file
{{ $title }}






---------------------------------------------------> Component Methods

-> Creating functions in component file and use it in component.

->    create a function simply -> $public function func_name() { }

->    now use it in card view file by {{ func_name() }}

`ALSO WE CAN PASS PARAMETERS`





-------------------------------> What if we had declared an attribute in the tag and did'nt code its instance in its constructor class.

then look the beauty of laravel, it provides you {{ $attributes }} through which you get all the attributes that you has'nt added in the constructor class

ex:- 

<x-card title="huehue" :description=$descr class="red-class" />

and suppose constructor class looks like:


                                    class card extends Component
                                    {

                                        public $title;
                                        public $description;

                                        public function __construct( $title, $description )
                                        {
                                            $this->title = $title;
                                            $this->description = $description;
                                        }

                                    }



Now in the card component we can access all the uninitialised vars by using: {{ $attributes }}

-> This can also be used for debugging purpose








-----------------------------> Additional info [ Merge the data at the component side ]

if we want to merge some data into that attribute which we are getting from {{ $attributes }} ( if not exist then it will create new ), then

<h1> {{ $attributes->merge([ 'class' => 'merged-class' ]) }} </h1>





















================================================== [  ANONYMOUS COMPONENTS  ] ==================================================

I am literally shocked when i learn Anonymous component.

To create Anonymous component just create single file only in `rescources/views/components/component_name` and simply write your content over there, rest all the process is same but this time we don't having need of any constructor initialisation. 


#### After the deep research i find the difference between the two is: for complex processing we usually prefer class components and vice versa.



















