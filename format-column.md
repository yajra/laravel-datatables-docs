---
title: Column Formatting
description: Format columns using formatter classes in DataTables
---

# Column Formatting

DataTables supports formatting columns using formatter classes through the `formatColumn()` API. This allows you to define reusable formatters for consistent column presentation.

---

<a name="formatter-interface"></a>
## Creating a Formatter

Create a class that implements the `Yajra\DataTables\Contracts\Formatter` interface:

```php
<?php

namespace App\DataTables\Formatters;

use Yajra\DataTables\Contracts\Formatter;

class StatusFormatter implements Formatter
{
    public function format(mixed $value, $row): string
    {
        return match ($value) {
            'active' => '<span class="badge bg-success">Active</span>',
            'inactive' => '<span class="badge bg-secondary">Inactive</span>',
            'pending' => '<span class="badge bg-warning">Pending</span>',
            default => '<span class="badge bg-dark">' . e($value) . '</span>',
        };
    }
}
```

---

<a name="basic-usage"></a>
## Basic Usage

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;
use App\DataTables\Formatters\StatusFormatter;

Route::get('user-data', function() {
    return DataTables::eloquent(User::query())
        ->formatColumn('status', new StatusFormatter())
        ->toJson();
});
```

---

<a name="multiple-columns"></a>
## Format Multiple Columns

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;
use App\DataTables\Formatters\DateFormatter;

Route::get('user-data', function() {
    return DataTables::eloquent(User::query())
        ->formatColumn(['created_at', 'updated_at'], new DateFormatter('M d, Y'))
        ->toJson();
});
```

---

<a name="closure"></a>
## Using Closures

For simple formatting, use closures instead of a formatter class:

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    return DataTables::eloquent(User::query())
        ->formatColumn('status', function ($value, $row) {
            return ucfirst($value);
        })
        ->toJson();
});
```

---

<a name="accessor-format"></a>
## Accessor Format

If no formatter or closure is provided, the column value is automatically extracted:

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    return DataTables::eloquent(User::query())
        // Extracts and displays the 'name' attribute
        ->formatColumn('name', 'name')
        ->toJson();
});
```

---

## See Also

- [Edit Column](/docs/{{package}}/{{version}}/edit-column) - Modify existing columns
- [Raw Columns](/docs/{{package}}/{{version}}/raw-columns) - Allow HTML rendering
