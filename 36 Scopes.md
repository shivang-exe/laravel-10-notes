##                            SCOPES IN LARAVEL

`Suppose we need to push our data in such a way that the lastest one will be get by the very first then we can do this using scopes`

Create a directory `scopes` under app dir and create a php file

{{

// ReverseScope.php

namespace App\Scopes;

use Illuminate\Database\Eloquent\Model;
use Illuminate\Database\Eloquent\Scope;
use Illuminate\Database\Eloquent\Builder;

class ReverseScope implements Scope
{

    public function apply( Builder $builder, Model $model )
    {
        $builder->orderBy('id', 'desc');$
    }

}

}}

{{

    // Using scope in a model
    protected static function boot()
    {
        parent::boot();

        static::addGlobalScope(new ReverseScope() );
    }

}}