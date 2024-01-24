#                              Blade Templating Engine

Like EJS in Express.Js here we have Blade in Laravel, it simply allows us to write plain `php` code inside html.
extension :- name.blade.php

{{ }} -> data representation
Ex: {{ $name }}

` NOTE: WE CAN CODE ANY php STATEMENTS INSIDE BLADE ECHO {{ }} `





-------------------> `Conditional Directives`

If Directive syntax:

{{
@if( condition )
    .................
@endif
}}



If..Else.. Directive syntax:

{{
@if( condition )
    ............
@else
    ............
@endif
}}



If..elseif.. Directive syntax:

{{

@if( condition )
    ............
@elseif( condition ) 
    ............
@else
    ...........
@endif

}}



unless Directive -> will not execute if condition is true 

{{

@unless( cond )
    .......
@endunless

}}


isset Directive -> Will only execute if the value inside the parameter is not undefined or null

{{

@isset( $value )
    .....
@endisset

}}

empty directive  -> Will only execute if the parameter inside it is either undefined or null

{{

@empty( $value )
    .......
@endempty

}}



---------------------------------------> Environmental Directives

// Will exxcute if Environment is local in development phase
{{

@env('local')
    .......
@endenv

}}


// Will exxcute in production phase

{{

@production
    ......
@endproduction

}}


-------------------------------> Authentication Directives

Auth Directive

{{

@auth
    .................
@endauth

}}


Guest Directive

{{

@guest
    ............
@endguest

}}


-------------------------------> Switch Directives
{{

@switch( expression )

    @case( expression a )
        ........
        @break
    
    @case( expression b )
        .......
        @break

    @default
        .......

@endswitch

}}




-------------------------------> Loop Directives
{{

@for( init; cond; iter )
        .........
@endfor




@forEach( arr as value )
        ........
@endForEach

}}


// Is used when there may be a possibility of an empty array, if so then execute that statement that are written under @empty
{{

@forelse ( arr as value )
        ........
@empty
        ........
@endforelse



@while( condn )
        ......
@endwhile

}}

------------------------------------------> Break and continue 

`@break`

// break and continue also has power now to check the condition. i.e. no need to add an extra if statement

`@continue`



syntax 1: break( condn )
syntax 2: continue( condn )





------------------------------------------> class directive

{{

@class(['class_one', 'class_two class_three'=> boolean_exp] );
// class_one will always be applied while class_two and class_three will be applied if (boolean_exp) is true.

}}




------------------------------------------> loop varible

has all the power of the current iteration inside a loop.
just look at them






-------------------------------------------> Include directive ( works like partials in Express )

we can put a view inside an already existing view by using include directive and passing the name of the view which we wants to include.


syntax: @include(' view_name ');

To pass the data while including the sub_view 

syntax: @include( 'view_name', ['key' => 'value' ] );

TIP: if the view file is importing data from controller and it has included a sub_view then the sub_view will also containing that data which is imported from controllers.



--------> @includeIf directive      => if we're having a doubt that passed view is actually view or not ? if it is, then it will render it else did nothing.

syntax:     @includeIf( 'view_name' );


--------> @includeWhen and @includeUnless  => if we want to include a view on the basis of any condition then we will use these two.

syntax:    @includeWhen( `BOOLEAN_EXPRESSION`, 'view_name' );     -> include if condn is true
syntax:    @includeUnless( `BOOLEAN_EXPRESSION`, 'view_name' );   -> include if condn is false








---------------------------------------------------------> Each Directive
Each is used when we wants to loop any array and at the same time ,include a file


ex: 

            @foreach( arr_name as item )
            @include('blade_name');
            @endForEach

    `REPLACE ABOVE WITH THE BELOW ONE`

            @each( 'blade_name', arr_name, 'item' );








--------------------------------------------------------------> Once directive   => used when we need to execute the set of statements only Once






------------------------------------------------------------> Raw php in blade file

syntax-> @php      ...........          @endphp

ex: 

@php    
        .....
@endphp

