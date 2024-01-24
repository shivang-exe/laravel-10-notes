#                              TRAITS

Traits in laravel is a cool feature that enables a class to import the external file functionalities without inheriting it.

-> We must put this trait in `App\Http\Traits`
-> Always assign a namespace to trait for easy import;

ex: 

App\Http\Trait:
creating a file canLoadRnsps.php
{{

    namespace App\Http\Traits;

    trait CanLoadRelationships;

    function test( $q )
    {
        ...
    } 
}}

utilising trait in another file
{{

    use App\Http\Traits\CanLoadRelationships;

    class Abc
    {
        use CanLoadRelationships;

        $a  = this->test();$
    }
}}