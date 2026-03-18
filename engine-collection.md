---
title: Collection Data Source
description: Use Laravel Collections as data source for DataTables
---

# Collection Data Source

You can use Laravel's Collection as data source for your DataTables. The `Yajra\DataTables\CollectionDataTable` class handles the conversion of your Collection into a DataTables-compatible response.

> [!NOTE]
> Collections are best suited for smaller datasets. For large datasets, consider using Eloquent or Query Builder engines.

---

## Usage Methods

### Method 1: Factory Pattern

```php
use Yajra\DataTables\Facades\DataTables;

Route::get('user-data', function() {
    $collection = collect([
        ['id' => 1, 'name' => 'John'],
        ['id' => 2, 'name' => 'Jane'],
        ['id' => 3, 'name' => 'James'],
    ]);

    return DataTables::of($collection)->toJson();
});
```

### Method 2: Facade with Collection

```php
use Yajra\DataTables\Facades\DataTables;

Route::get('user-data', function() {
    $collection = collect([
        ['id' => 1, 'name' => 'John'],
        ['id' => 2, 'name' => 'Jane'],
        ['id' => 3, 'name' => 'James'],
    ]);

    return DataTables::collection($collection)->toJson();
});
```

### Method 3: Dependency Injection

```php
use Yajra\DataTables\DataTables;

Route::get('user-data', function(DataTables $dataTables) {
    $collection = collect([
        ['id' => 1, 'name' => 'John'],
        ['id' => 2, 'name' => 'Jane'],
        ['id' => 3, 'name' => 'James'],
    ]);

    return $dataTables->collection($collection)->toJson();
});
```

### Method 4: IoC Container

```php
use Yajra\DataTables\Facades\DataTables;

Route::get('user-data', function() {
    $collection = collect([
        ['id' => 1, 'name' => 'John'],
        ['id' => 2, 'name' => 'Jane'],
        ['id' => 3, 'name' => 'James'],
    ]);

    return app('datatables')->collection($collection)->toJson();
});
```

### Method 5: Direct Instance

```php
use Yajra\DataTables\CollectionDataTable;
use App\Models\User;

Route::get('user-data', function() {
    $collection = User::all();

    return (new CollectionDataTable($collection))->toJson();
});
```

---

## With Eloquent Models

Convert Eloquent models to collection:

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $collection = User::all();

    return DataTables::collection($collection)->toJson();
});
```

---

## With Filtering

Apply collection methods before passing to DataTables:

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $collection = User::all()
        ->filter(function ($user) {
            return $user->is_active;
        });

    return DataTables::collection($collection)->toJson();
});
```

---

## Adding Custom Columns

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $collection = User::all();

    return DataTables::collection($collection)
        ->addColumn('full_name', function ($user) {
            return $user->first_name . ' ' . $user->last_name;
        })
        ->toJson();
});
```

---

## Response Structure

```json
{
    "draw": 1,
    "recordsTotal": 100,
    "recordsFiltered": 50,
    "data": [
        {
            "id": 1,
            "name": "John Doe",
            "email": "john@example.com"
        }
    ]
}
```

---

## When to Use Collections

| Use Case | Recommendation |
|----------|----------------|
| Small datasets (< 1000 rows) | ✅ Perfect for collections |
| Large datasets | ❌ Use Eloquent or Query Builder |
| API data transformation | ✅ Great with Fractal |
| Complex calculations | ✅ Use Collection methods |

---

## See Also

- [Eloquent Data Source](/docs/{{package}}/{{version}}/engine-eloquent) - Use Eloquent models
- [Query Builder Data Source](/docs/{{package}}/{{version}}/engine-query) - Use Query Builder
- [Add Column](/docs/{{package}}/{{version}}/add-column) - Add custom columns
