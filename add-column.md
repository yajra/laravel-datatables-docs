---
title: Add Column
description: Add custom computed columns to your DataTables response
---

# Add Column

You can add custom columns to your DataTables response using the `addColumn` API. Added columns are computed columns and are not part of the database, so search and sort are disabled by default.

> [!WARNING]
> Added columns are computed and not part of the database. Search/sort will be disabled. Use `editColumn` if you need search/sort functionality.

---

<a name="blade"></a>
## Method 1: Blade String Syntax

Use Blade-style string interpolation:

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::query();

    return DataTables::eloquent($model)
        ->addColumn('intro', 'Hi {{$name}}!')
        ->toJson();
});
```

---

<a name="closure"></a>
## Method 2: Closure

Use a closure for more complex logic:

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::query();

    return DataTables::eloquent($model)
        ->addColumn('intro', function(User $user) {
            return 'Hi ' . $user->name . '!';
        })
        ->toJson();
});
```

---

<a name="view"></a>
## Method 3: Blade View

Render the column using a Blade view:

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::query();

    return DataTables::eloquent($model)
        ->addColumn('intro', 'users.datatables.intro')
        ->toJson();
});
```

Create your view at `resources/views/users/datatables/intro.blade.php`:

```blade
Hi {{ $name }}!
```

---

## Column Ordering

Position the column at a specific index by passing order as the third argument:

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::query();

    return DataTables::eloquent($model)
        ->addColumn('intro', 'Hi {{$name}}!', 2) // Position at index 2
        ->toJson();
});
```

---

<a name="multiple"></a>
## Multiple Columns

Add multiple columns by chaining:

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
        ->addColumn('status_badge', function (User $user) {
            return $user->is_active ? 'Active' : 'Inactive';
        })
        ->toJson();
});
```

---

## Common Use Cases

### Action Buttons

```php
->addColumn('action', function (User $user) {
    return '<a href="/users/'.$user->id.'/edit">Edit</a>';
})
->rawColumns(['action']) // Allow HTML rendering
```

### Computed Values

```php
->addColumn('total_amount', function (Order $order) {
    return '$' . number_format($order->items->sum('price'), 2);
})
```

### Conditional Formatting

```php
->addColumn('status', function (User $user) {
    if ($user->is_active) {
        return '<span class="badge bg-success">Active</span>';
    }
    return '<span class="badge bg-secondary">Inactive</span>';
})
->rawColumns(['status'])
```

---

## Comparison: addColumn vs editColumn

| Feature | `addColumn` | `editColumn` |
|---------|-------------|--------------|
| Search | ❌ Disabled | ✅ Enabled |
| Sort | ❌ Disabled | ✅ Enabled |
| Database Column | ❌ Not required | ✅ Required |
| Use Case | Computed values | Modifying existing |

> [!TIP]
> Need both search/sort AND multiple columns? Use `editColumn` to modify existing columns and chain multiple calls.

---

## See Also

- [Edit Column](/docs/{{package}}/{{version}}/edit-column) - Modify existing columns
- [Remove Column](/docs/{{package}}/{{version}}/remove-column) - Remove columns
- [Raw Columns](/docs/{{package}}/{{version}}/raw-columns) - Allow HTML in columns
