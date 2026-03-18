# Response using Transformer

You'll need to install [laravel-datatables-fractal package](https://github.com/yajra/laravel-datatables-fractal) if you haven't already.

When using transformers, all response manipulations must be done via transformer.
Thus `addColumn`, `editColumn`, `removeColumn`, `setRowAttr`, `setClassAttr`, etc. should be avoided when using Fractal.

<a name="closure"></a>
## Closure

You can pass a closure which will receive an item of the result collection and should return an array.

```php
use DataTables;
use App\User;

Route::get('user-data', function () {
    $model = User::query();

    return DataTables::eloquent($model)
        ->setTransformer(function ($item) {
            return [
                'id'         => (int) $item->id,
                'name'       => $item->name,
                'email'      => $item->email,
                'created_at' => (string) $item->created_at,
                'updated_at' => (string) $item->updated_at,
            ];
        })
        ->toJson();
});
```

Thus you can make use of Laravel API Resource.

```php
use DataTables;
use App\User;
use App\Http\Resources\UserResource;

Route::get('user-data', function () {
    $model = User::query();

    return DataTables::eloquent($model)
        ->setTransformer(function ($item) {
            return UserResource::make($item)->resolve();
        })
        ->toJson();
});
```

<a name="transformer"></a>
## Transformer

You can use a Transformer class.

```php
use DataTables;
use App\Transformers\UserTransformer;

Route::get('user-data', function () {
    $model = App\User::query();

    return DataTables::eloquent($model)
        ->setTransformer(new UserTransformer)
        ->toJson();
});
```

<a name="manual-way"></a>
### Manual Way

Create a transformer class, preferably named as below.

`app/Transformers/UserTransformer.php`

```php
namespace App\Transformers;

use App\User;
use League\Fractal\TransformerAbstract;

class UserTransformer extends TransformerAbstract
{
    protected $availableIncludes = ['posts'];

    public function transform(User $user): array
    {
        return [
            'id'         => (int) $user->id,
            'name'       => $user->name,
            'email'      => $user->email,
            'created_at' => (string) $user->created_at,
            'updated_at' => (string) $user->updated_at,
        ];
    }

    public function includePosts(User $user)
    {
        $posts = $user->posts;

        return $this->collection($posts, new PostTransformer);
    }
}
```

<a name="artisan-command-way"></a>
### Artisan Command Way

You can use `make:transformer` command to generate boilerplate.

```bash
php artisan make:transformer User
```

```php
namespace App\Transformers;

use League\Fractal\TransformerAbstract;
use App\User;

class UserTransformer extends TransformerAbstract
{
    public function transform(User $user): array
    {
        return [
            'id' => (int) $user->id,
        ];
    }
}
```

Or even with included class.

```bash
php artisan make:transformer User Post
```

```php
namespace App\Transformers;

use League\Fractal\TransformerAbstract;
use App\User;
use App\Post;

class UserTransformer extends TransformerAbstract
{
    protected $availableIncludes = ['post'];

    public function transform(User $user): array
    {
        return [
            'id' => (int) $user->id,
        ];
    }

    public function includePost(User $user)
    {
        return $this->collection($user->post, new PostTransformer);
    }
}
```

Then you can make final changes.

```php
namespace App\Transformers;

use League\Fractal\TransformerAbstract;
use App\User;
use App\Post;

class UserTransformer extends TransformerAbstract
{
    protected $availableIncludes = ['posts'];

    public function transform(User $user): array
    {
        return [
            'id'         => (int) $user->id,
            'name'       => $user->name,
            'email'      => $user->email,
            'created_at' => (string) $user->created_at,
            'updated_at' => (string) $user->updated_at,
        ];
    }

    public function includePost(User $user)
    {
        return $this->collection($user->posts, new PostTransformer);
    }
}
```

## See Also

- [Fractal Plugin Installation](/docs/fractal-installation) - Get started with the Fractal plugin
- [Fractal Transformer Serializer](/docs/response-fractal-serializer) - Configure custom serializers for Fractal transformers
