---
title: Smart Search
description: Configure DataTables smart search with automatic wildcards
---

# Smart Search

You can optionally enable/disable DataTables smart search feature by using the `smart` API. When enabled, wildcards are automatically added to search terms.

Default value is `true`.

---

## Disable Smart Search

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::query();

    return DataTables::eloquent($model)
        ->smart(false)
        ->toJson();
});
```

---

## Enable Smart Search (Default)

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::query();

    return DataTables::eloquent($model)
        ->smart(true) // Default behavior
        ->toJson();
});
```

---

## How Smart Search Works

| User Search | Smart Search Enabled | Smart Search Disabled |
|-------------|---------------------|---------------------|
| `john` | `%john%` | `john` |
| `john doe` | `%john%` and `%doe%` | `john doe` |

---

## Configuration

You can also configure smart search globally in `config/datatables.php`:

```php
<?php

return [
    /*
     * Smart search adds wildcards automatically
     */
    'smart' => true,

    /*
     * Case insensitive searching
     */
    'case_insensitive' => true,

    /*
     * Wild card character for partial matching
     */
    'use_wildcards' => false,
];
```

---

## See Also

- [Manual Search](/docs/{{package}}/{{version}}/manual-search) - Custom filtering logic
- [Filter Column](/docs/{{package}}/{{version}}/filter-column) - Custom column filtering
- [Regex Search](/docs/{{package}}/{{version}}/regex) - Regular expression search
