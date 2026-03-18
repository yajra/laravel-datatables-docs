---
title: Eloquent Data Source
description: Use Laravel Eloquent models as data source for DataTables
---

# Eloquent Data Source

You can use Laravel's Eloquent Model as data source for your DataTables. The `Yajra\DataTables\EloquentDataTable` class handles the conversion of your Eloquent Model into a DataTables-compatible response.

---

## Usage Methods

### Method 1: Factory Pattern

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::query();

    return DataTables::of($model)->toJson();
});
```

### Method 2: Facade with Eloquent

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::query();

    return DataTables::eloquent($model)->toJson();
});
```

### Method 3: Dependency Injection

```php
use Yajra\DataTables\DataTables;
use App\Models\User;

Route::get('user-data', function(DataTables $dataTables) {
    $model = User::query();

    return $dataTables->eloquent($model)->toJson();
});
```

### Method 4: IoC Container

```php
use App\Models\User;

Route::get('user-data', function() {
    $model = User::query();

    return app('datatables')->eloquent($model)->toJson();
});
```

### Method 5: Direct Instance

```php
use Yajra\DataTables\EloquentDataTable;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::query();

    return (new EloquentDataTable($model))->toJson();
});
```

---

## With Relationships

You can eager load relationships:

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::with(['posts', 'roles']);

    return DataTables::eloquent($model)->toJson();
});
```

> [!TIP]
> See [Relationships](/docs/{{package}}/{{version}}/relationships) for detailed information on searching and sorting eager loaded relationships.

---

## With Query Scopes

Apply model scopes for filtered results:

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::active()->verified();

    return DataTables::eloquent($model)->toJson();
});
```

---

## Adding Custom Columns

Add computed columns with closures:

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::query();

    return DataTables::eloquent($model)
        ->addColumn('full_name', function (User $user) {
            return $user->first_name . ' ' . $user->last_name;
        })
        ->addColumn('posts_count', function (User $user) {
            return $user->posts->count();
        })
        ->toJson();
});
```

> [!NOTE]
> Added columns are computed and not part of the database, so search and sort are disabled by default. Use `editColumn` if you need search/sort functionality.

---

## With Soft Deletes

Include soft-deleted records:

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::withTrashed();

    return DataTables::eloquent($model)->toJson();
});
```

---

## Response Structure

The Eloquent engine returns:

```json
{
    "draw": 1,
    "recordsTotal": 100,
    "recordsFiltered": 50,
    "data": [
        {
            "id": 1,
            "name": "John Doe",
            "email": "john@example.com",
            "created_at": "2024-01-01T00:00:00.000000Z"
        }
    ]
}
```

---

## See Also

- [Query Builder Data Source](/docs/{{package}}/{{version}}/engine-query) - Use Query Builder
- [Collection Data Source](/docs/{{package}}/{{version}}/engine-collection) - Use Laravel Collections
- [Add Column](/docs/{{package}}/{{version}}/add-column) - Add custom columns
- [Relationships](/docs/{{package}}/{{version}}/relationships) - Eager loading relationships
