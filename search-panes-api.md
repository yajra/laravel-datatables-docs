---
title: SearchPanes Extension
description: Add server-side SearchPanes support to DataTables
---

# SearchPanes Extension

DataTables SearchPanes allows users to filter data by selecting values from a list of options. This documentation covers the server-side SearchPanes API.

---

<a name="basic"></a>
## Basic Usage

Use the `searchPane()` API to add search pane options on the response:

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    return DataTables::eloquent(User::query())
        ->searchPane('status', [
            'Active' => User::where('status', 'active')->count(),
            'Inactive' => User::where('status', 'inactive')->count(),
        ])
        ->toJson();
});
```

---

<a name="with-builder"></a>
## With Custom Builder Callback

For more complex filtering logic, pass a callback that receives the query builder:

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    return DataTables::eloquent(User::query())
        ->searchPane('role', [
            'Admin' => 10,
            'User' => 50,
        ], function ($query, $values) {
            // Custom filter logic when pane value is selected
            $query->whereIn('role', $values);
        })
        ->toJson();
});
```

---

<a name="options-format"></a>
## Options Format

The options can be passed as:

- **Array**: Key-value pairs of label => count
- **Collection with toArray()**: Any `Arrayable` object
- **Callable**: Returns the options dynamically

```php
// Array format
'status' => ['active' => 100, 'inactive' => 25]

// Callable format
'status' => fn() => Status::pluck('name', 'id')->toArray()
```

---

## See Also

- [SearchPanes Configuration](/docs/{{package}}/{{version}}/search-panes-options) - Client-side configuration
- [SearchPanes Hide Columns](/docs/{{package}}/{{version}}/search-panes-hide-columns) - Exclude columns from panes
