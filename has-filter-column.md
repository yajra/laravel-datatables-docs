---
title: Has Filter Column
description: Check if a column has a custom filter defined
---

# Has Filter Column

The `hasFilterColumn()` API checks if a custom filter has been defined for a specific column using `filterColumn()`.

---

<a name="basic"></a>
## Basic Usage

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;
use Illuminate\Database\Eloquent\Builder;

Route::get('user-data', function() {
    $dataTable = DataTables::eloquent(User::query())
        ->filterColumn('name', fn(Builder $query, $keyword) =>
            $query->whereRaw(
                "CONCAT(first_name, ' ', last_name) LIKE ?",
                ["%{$keyword}%"]
            )
        );

    // Check if 'name' has a custom filter
    if ($dataTable->hasFilterColumn('name')) {
        // Custom filter is defined
    }

    return $dataTable->toJson();
});
```

---

<a name="use-case"></a>
## Use Case: Conditional Logic

This is useful when building conditional logic based on whether custom filters exist:

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;
use Illuminate\Database\Eloquent\Builder;

Route::get('user-data', function() {
    $dataTable = DataTables::eloquent(User::query());

    // Define custom filter only if not already defined
    if (!$dataTable->hasFilterColumn('status')) {
        $dataTable->filterColumn('status', function(Builder $query, $keyword) {
            $query->where('status', $keyword);
        });
    }

    return $dataTable->toJson();
});
```

---

<a name="check-all"></a>
## Checking Multiple Columns

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $columns = ['name', 'email', 'status'];
    $dataTable = DataTables::eloquent(User::query());

    foreach ($columns as $column) {
        if ($dataTable->hasFilterColumn($column)) {
            // Log which columns have custom filters
            logger()->info("Custom filter defined for: {$column}");
        }
    }

    return $dataTable->toJson();
});
```

---

## See Also

- [Filter Column](/docs/{{package}}/{{version}}/filter-column) - Define custom column filters
- [Manual Search](/docs/{{package}}/{{version}}/manual-search) - Custom search logic
