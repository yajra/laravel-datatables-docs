---
title: Order Column
description: Enable custom ordering on specific columns
---

# Order Column

In some cases, you may want to use a custom order SQL for a specific column. To achieve this, use the `orderColumn` API.

> [!TIP]
> Order column has a special variable `$1` which is being replaced as the order direction value of the request.

---

<a name="basic"></a>
## Basic Usage

In this example, we will order the column name with nulls as last result:

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::query();

    return DataTables::eloquent($model)
        ->orderColumn('name', '-name $1')
        ->toJson();
});
```

---

<a name="closure"></a>
## Using Closure

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::query();

    return DataTables::eloquent($model)
        ->orderColumn('name', function ($query, $order) {
            $query->orderBy('status', $order);
        })
        ->toJson();
});
```

---

## Disable Ordering

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::query();

    return DataTables::eloquent($model)
        ->orderColumn('name', false)
        ->toJson();
});
```

---

## Common Patterns

### Nulls Last

```php
return DataTables::eloquent($model)
    ->orderColumn('name', 'name $1 NULLS LAST')
    ->toJson();
```

### Custom Direction Mapping

```php
return DataTables::eloquent($model)
    ->orderColumn('status', function ($query, $order) {
        // Map ascending/descending to custom status order
        $direction = $order === 'asc' ? 'desc' : 'asc';
        $query->orderBy('priority', $direction);
    })
    ->toJson();
```

---

## See Also

- [Order Columns](/docs/{{package}}/{{version}}/order-columns) - Configure multiple column ordering
- [Order By Nulls Last](/docs/{{package}}/{{version}}/order-by-nulls-last) - Null values ordering
