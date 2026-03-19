---
title: Manual Order
description: Implement custom ordering logic in DataTables
---

# Manual Order

You may optionally disable the default ordering function of DataTables and write your own using the `order` API.

---

<a name="basic"></a>
## Basic Usage

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::query();

    return DataTables::eloquent($model)
        ->order(function ($query) {
            if (request()->has('name')) {
                $query->orderBy('name', 'asc');
            }

            if (request()->has('email')) {
                $query->orderBy('email', 'desc');
            }
        })
        ->toJson();
});
```

---

<a name="advanced-ordering"></a>
## Advanced Ordering

### Multiple Column Ordering

```php
return DataTables::eloquent(User::query())
    ->order(function ($query) {
        // Primary sort
        $query->orderBy('status', 'asc');
        // Secondary sort
        $query->orderBy('name', 'asc');
    })
    ->toJson();
```

### Conditional Ordering

```php
return DataTables::eloquent(User::query())
    ->order(function ($query) {
        $sortColumn = request('sort', 'created_at');
        $sortDirection = request('direction', 'desc');

        // Only order by allowed columns
        $allowedColumns = ['name', 'email', 'created_at'];
        if (in_array($sortColumn, $allowedColumns)) {
            $query->orderBy($sortColumn, $sortDirection);
        }
    })
    ->toJson();
```

---

## See Also

- [Order Column](/docs/{{package}}/{{version}}/order-column) - Order by specific column
- [Order Columns](/docs/{{package}}/{{version}}/order-columns) - Multiple column ordering
