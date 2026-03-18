---
title: Regex Searching
description: Enable regular expression search in DataTables
---

# Regex Searching

DataTables has the ability to perform searches using regular expressions for more advanced pattern matching.

> [!NOTE]
> Regex search only works and tested on the following Laravel DB drivers: MySQL, SQLite, and Oracle.

---

<a name="regex"></a>
## Enable Regex Search

To enable regex, you need to explicitly set it in your JavaScript:

```javascript
$(document).ready(function() {
    $('#example').DataTable({
        processing: true,
        serverSide: true,
        ajax: '',
        columns: [
            {data: 'id', name: 'id'},
            {data: 'name', name: 'name'},
            {data: 'email', name: 'email'},
            {data: 'created_at', name: 'created_at'},
            {data: 'updated_at', name: 'updated_at'}
        ],
        search: {
            "regex": true
        }
    });
});
```

---

## Common Patterns

### Search for Emails

```
.*@example\.com$
```

### Search for Numbers in Range

```
^([1-9]|[1-7][0-9]|80)$
```

### Search Starting with Pattern

```
^John
```

### Search Ending with Pattern

```
\.pdf$
```

---

## Case Sensitivity

Regex patterns are case-sensitive by default. Use the `i` flag for case-insensitive matching:

```javascript
search: {
    "regex": true,
    "caseInsensitive": true
}
```

---

## Backend Configuration

You may also need to configure your DataTables service to handle regex searches:

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    return DataTables::eloquent(User::query())
        ->toJson();
});
```

---

## See Also

- [Smart Search](/docs/{{package}}/{{version}}/smart-search) - Wildcard search
- [Manual Search](/docs/{{package}}/{{version}}/manual-search) - Custom filtering logic
- [Filter Column](/docs/{{package}}/{{version}}/filter-column) - Custom column filtering
