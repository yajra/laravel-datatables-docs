---
title: Order Columns
description: Configure column ordering options for multiple columns
---

# Order Columns

In some cases, you may want to use a custom order SQL for a set of columns. To achieve this, use the `orderColumns` API.

---

<a name="variables"></a>
## Special Variables and Placeholders

| Variable | Description |
|----------|-------------|
| `$1` | Replaced with the order direction of the column |
| `:column` | Replaced with the column name set in the first parameter |

---

## Basic Usage

In this example, we will order the column name with nulls as last result:

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::query();

    return DataTables::eloquent($model)
        ->orderColumns(['name', 'email'], '-:column $1')
        ->toJson();
});
```

---

## Common Patterns

### Multiple Columns with Nulls Last

```php
return DataTables::eloquent($model)
    ->orderColumns(['name', 'email', 'created_at'], ':column $1 NULLS LAST')
    ->toJson();
```

### Custom Format

```php
return DataTables::eloquent($model)
    ->orderColumns(['name', 'status'], 'COALESCE(:column, 999) $1')
    ->toJson();
```

---

## See Also

- [Order Column](/docs/{{package}}/{{version}}/order-column) - Single column ordering
- [Order By Nulls Last](/docs/{{package}}/{{version}}/order-by-nulls-last) - Null values ordering
