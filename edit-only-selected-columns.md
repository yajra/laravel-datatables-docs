---
title: Edit Only Selected Columns
description: Edit only columns that are selected in the DataTables request
---

# Edit Only Selected Columns

The `editOnlySelectedColumns()` API ensures that `editColumn()` callbacks are only executed for columns that are explicitly selected in the DataTables request.

---

<a name="basic"></a>
## Basic Usage

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    return DataTables::eloquent(User::query())
        ->editOnlySelectedColumns()
        ->editColumn('name', fn(User $user) => strtoupper($user->name))
        ->editColumn('email', fn(User $user) => strtolower($user->email))
        ->toJson();
});
```

---

<a name="use-case"></a>
## Use Case

This is useful when you want to apply expensive transformations only to columns that are actually being displayed:

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\Order;

Route::get('order-data', function() {
    return DataTables::eloquent(Order::query())
        ->editOnlySelectedColumns()
        ->editColumn('details', function (Order $order) {
            // This is only called if 'details' column is selected
            return $order->loadExpensiveDetails();
        })
        ->editColumn('total', function (Order $order) {
            // This is only called if 'total' column is selected
            return number_format($order->total, 2);
        })
        ->toJson();
});
```

When a user hides the `details` or `total` column in their DataTables configuration, the corresponding callbacks are not executed, improving performance.

---

<a name="without-selection"></a>
## Behavior Without Selection

When no columns are explicitly selected in the request (default DataTables behavior), all `editColumn()` callbacks will be executed as normal.

---

## See Also

- [Edit Column](/docs/{{package}}/{{version}}/edit-column) - Modifying existing columns
- [Add Column](/docs/{{package}}/{{version}}/add-column) - Adding custom columns
