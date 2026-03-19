---
title: Response using Transformer
description: Transform DataTables responses using Fractal transformers
---

# Response using Transformer

> [!NOTE]
> You'll need to install [laravel-datatables-fractal package](https://github.com/yajra/laravel-datatables-fractal) if you haven't already.

When using transformers, all response manipulations must be done via transformer. Thus `addColumn`, `editColumn`, `removeColumn`, `setRowAttr`, `setClassAttr`, etc. should be avoided when using Fractal.

---

<a name="closure"></a>
## Using Closure

You can pass a closure which will receive an item of the result collection and should return an array:

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function () {
    $model = User::query();

    return DataTables::eloquent($model)
        ->setTransformer(function (User $item) {
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

Thus you can make use of Laravel API Resource:

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;
use App\Http\Resources\UserResource;

Route::get('user-data', function () {
    $model = User::query();

    return DataTables::eloquent($model)
        ->setTransformer(function (User $item) {
            return UserResource::make($item)->resolve();
        })
        ->toJson();
});
```

---

## Using Transformer Class

You can use a Transformer class:

```php
use Yajra\DataTables\Facades\DataTables;
use App\Transformers\UserTransformer;

Route::get('user-data', function () {
    $model = App\Models\User::query();

    return DataTables::eloquent($model)
        ->setTransformer(new UserTransformer)
        ->toJson();
});
```

### Creating a Transformer

Create a transformer class:

```php
<?php
// app/Transformers/UserTransformer.php

namespace App\Transformers;

use App\Models\User;
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

### Using Artisan Command

You can use `make:transformer` command to generate boilerplate:

```bash
php artisan make:transformer User
```

```php
<?php
// app/Transformers/UserTransformer.php

namespace App\Transformers;

use League\Fractal\TransformerAbstract;
use App\Models\User;

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

Or even with included class:

```bash
php artisan make:transformer User Post
```

```php
<?php
// app/Transformers/UserTransformer.php

namespace App\Transformers;

use League\Fractal\TransformerAbstract;
use App\Models\User;
use App\Models\Post;

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

---

## See Also

- [Fractal Plugin Installation](/docs/{{package}}/{{version}}/fractal-installation) - Get started with the Fractal plugin
- [Fractal Transformer Serializer](/docs/{{package}}/{{version}}/response-fractal-serializer) - Configure custom serializers for Fractal transformers
