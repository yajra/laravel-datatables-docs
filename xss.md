---
title: XSS Filtering
description: Protect against XSS attacks with column escaping
---

# XSS Filtering

Since v7.0, all DataTable responses are encoded to prevent XSS attacks. In case you need to display HTML on your columns, you can use the `rawColumns` API.

> [!NOTE]
> The `action` column is allowed as raw by default.

---

<a name="raw"></a>
## Raw Columns

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\Role;

Route::get('role-data', function() {
    return DataTables::eloquent(Role::select())
        ->rawColumns(['name', 'action'])
        ->toJson();
});
```

---

## Escape Selected Fields

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\Role;

Route::get('role-data', function() {
    return DataTables::eloquent(Role::select())
        ->escapeColumns(['name'])
        ->toJson();
});
```

---

## Escape All Columns

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\Role;

Route::get('role-data', function() {
    return DataTables::eloquent(Role::select())
        ->escapeColumns()
        ->toJson();
});
```

---

## Remove Escaping of All Columns

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\Role;

Route::get('role-data', function() {
    return DataTables::eloquent(Role::select())
        ->escapeColumns([])
        ->toJson();
});
```

> [!WARNING]
> Disabling escaping for all columns is dangerous and may expose your application to XSS attacks. Only use this option when you fully control the content.

---

## Escape by Output Index

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\Role;

Route::get('role-data', function() {
    return DataTables::eloquent(Role::select())
        ->escapeColumns([0])
        ->make();
});
```

---

## Security Best Practices

| Practice | Description |
|----------|-------------|
| Use `rawColumns` sparingly | Only allow HTML in columns that need it |
| Sanitize user input | Always sanitize data before rendering as HTML |
| Use `e()` helper | Laravel's `e()` function for explicit escaping |
| Review exported data | Check exports for potential XSS vectors |

---

## See Also

- [Raw Columns](/docs/{{package}}/{{version}}/raw-columns) - Allow HTML in columns
- [Security](/docs/{{package}}/{{version}}/security) - Security best practices
