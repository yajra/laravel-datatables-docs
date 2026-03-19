---
title: Edit Column
description: Edit or modify existing columns in your DataTables response
---


# Edit Column

You can modify existing columns in your DataTables response using the `editColumn` API. Unlike `addColumn`, edited columns remain part of the database and support search/sort functionality.

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
        ->editColumn('name', 'Modified: {{$name}}')
        ->toJson();
});
```

---

<a name="closure"></a>
## Method 2: Closure

Use a closure for dynamic values:

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::query();

    return DataTables::eloquent($model)
        ->editColumn('name', function(User $user) {
            return strtoupper($user->name);
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
        ->editColumn('name', 'users.datatables.name')
        ->toJson();
});
```

Create your view at `resources/views/users/datatables/name.blade.php`:

```blade
<strong>{{ $name }}</strong>
```

---

## Common Use Cases

### Format Dates

```php
use Illuminate\Support\Str;

Route::get('user-data', function() {
    return DataTables::eloquent(User::query())
        ->editColumn('created_at', function (User $user) {
            return $user->created_at->format('M d, Y');
        })
        ->toJson();
});
```

### Money Formatting

For money values, use `Column::make()` with `exportFormat()` for exports:

```php
// Use Column::make() for decimal/float columns
Column::make('price', 'orders.price')
```

### Truncate Text

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;
use Illuminate\Support\Str;

Route::get('product-data', function() {
    return DataTables::eloquent(Product::query())
        ->editColumn('description', function (Product $product) {
            return Str::limit($product->description, 50);
        })
        ->toJson();
});
```

### Status Badges

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    return DataTables::eloquent(User::query())
        ->editColumn('status', function (User $user) {
            $color = $user->is_active ? 'success' : 'secondary';
            return '<span class="badge bg-'.$color.'">'.$user->status.'</span>';
        })
        ->rawColumns(['status']) // Allow HTML rendering
        ->toJson();
});
```

---

## Multiple Column Edits

Chain multiple edits:

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    return DataTables::eloquent(User::query())
        ->editColumn('name', function (User $user) {
            return $user->full_name;
        })
        ->editColumn('email', function (User $user) {
            return '<a href="mailto:'.$user->email.'">'.$user->email.'</a>';
        })
        ->editColumn('created_at', function (User $user) {
            return $user->created_at->diffForHumans();
        })
        ->rawColumns(['email']) // Allow HTML in email column
        ->toJson();
});
```

---

## Comparison: editColumn vs addColumn

| Feature | `editColumn` | `addColumn` |
|---------|--------------|-------------|
| Search | ✅ Enabled | ❌ Disabled |
| Sort | ✅ Enabled | ❌ Disabled |
| Database Column | ✅ Required | ❌ Not required |
| Use Case | Modify existing | Add new computed |

---

## See Also

- [Add Column](/docs/{{package}}/{{version}}/add-column) - Add new computed columns
- [Remove Column](/docs/{{package}}/{{version}}/remove-column) - Remove columns
- [Raw Columns](/docs/{{package}}/{{version}}/raw-columns) - Allow HTML in columns
