---
title: Security
description: Security policy for Laravel DataTables
---

# Security

If you discover any security related issues, please email [aqangeles@gmail.com](mailto:aqangeles@gmail.com) instead of using the issue tracker.

---

<a name="xss"></a>
## XSS Protection

Laravel DataTables escapes all column data by default to protect against Cross-Site Scripting (XSS) attacks.

To allow HTML content in specific columns, use the `rawColumns` method:

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    return DataTables::eloquent(User::query())
        ->addColumn('action', function (User $user) {
            return '<button class="btn btn-sm btn-primary">Edit</button>';
        })
        ->rawColumns(['action']) // Allow HTML in this column
        ->toJson();
});
```

> [!WARNING]
> Only use `rawColumns` with trusted data. Never pass user input directly to HTML output without proper sanitization.

---

## CSRF Protection

Laravel DataTables automatically handles CSRF tokens when using the `scripts()` method:

```blade
@push('scripts')
{{ $dataTable->scripts(attributes: ['type' => 'module']) }}
@endpush
```

Ensure your layout includes the CSRF token meta tag:

```blade
<meta name="csrf-token" content="{{ csrf_token() }}">
```

---

## Mass Assignment Protection

When using the Editor plugin, ensure your models have proper `$fillable` attributes:

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class User extends Model
{
    protected $fillable = [
        'name',
        'email',
        // Only include fields that should be mass-assigned
    ];
}
```
