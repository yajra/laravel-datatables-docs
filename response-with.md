---
title: Additional Data Response
description: Add extra server-side data to your DataTables response
---

# Additional Data Response

You can add additional server data to your response using the `with` API.

---

## Adding Response using Key and Value

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::query();

    return DataTables::eloquent($model)
        ->with('posts', 100)
        ->with('comments', 20)
        ->toJson();
});
```

---

## Adding Response using Array

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::query();

    return DataTables::eloquent($model)
        ->with([
            'posts' => 100,
            'comments' => 20,
        ])
        ->toJson();
});
```

---

## Adding Response with Closure

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::query();

    return DataTables::eloquent($model)
        ->with('count', function() use ($model) {
            return $model->count();
        })
        ->toJson();
});
```

---

## Adding Response with Query Callback

> [!NOTE]
> This is for Query and Eloquent instance only.

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::query();

    return DataTables::eloquent($model)
        ->withQuery('count', function($filteredQuery) {
            return $filteredQuery->count();
        })
        ->toJson();
});
```

---

## Example Response

```json
{
    "draw": 1,
    "recordsTotal": 10,
    "recordsFiltered": 3,
    "data": [
        {
            "id": 476,
            "name": "Esmeralda Kulas",
            "email": "abbott.cali@heaney.info",
            "created_at": "2016-07-31 23:26:14",
            "updated_at": "2016-07-31 23:26:14",
            "deleted_at": null,
            "superior_id": 0
        },
        {
            "id": 6,
            "name": "Zachery Muller",
            "email": "abdullah.koelpin@yahoo.com",
            "created_at": "2016-07-31 23:25:43",
            "updated_at": "2016-07-31 23:25:43",
            "deleted_at": null,
            "superior_id": 1
        }
    ],
    "posts": 100,
    "comments": 20
}
```

---

## See Also

- [Object Response](/docs/{{package}}/{{version}}/response-object) - Default response format
- [Array Response](/docs/{{package}}/{{version}}/response-array) - Array response format
