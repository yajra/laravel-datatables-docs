---
title: Collection Data Source
description: Use Laravel Collections as data source for DataTables
---

# Collection Data Source

Use Laravel's Collection as the data source for your DataTables. The `Yajra\DataTables\CollectionDataTable` class handles the conversion of your Collection into a DataTables-compatible response.

> [!NOTE]
> Collections are best suited for smaller datasets. For large datasets, consider using Eloquent or Query Builder engines.

---

<a name="quick-start"></a>
## Quick Start

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

---

<a name="eloquent"></a>
## With Model Collections

Load models and pass as collection:

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $collection = User::all();

    return DataTables::collection($collection)->toJson();
});
```

> [!NOTE]
> This approach loads all models into memory first. For large datasets, consider using the [Eloquent Engine](/docs/{{package}}/{{version}}/engine-eloquent) instead.

---

<a name="custom-columns"></a>
## Adding Custom Columns

Add computed columns with closures:

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

<a name="filtering"></a>
## Filtering

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

> [!TIP]
> When using collection filtering with DataTables, the entire dataset is filtered in PHP memory. For large datasets, consider filtering at the database level using Eloquent or Query Builder engines.

---

<a name="when-to-use"></a>
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
