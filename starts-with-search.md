---
title: Starts With Search
description: Search column values that start with a specific keyword
---

# Starts With Search

Starts with search feature allows the package to search our tables with records that start with the given keyword. To enable the feature, chain `->startsWithSearch()` on your instance.

---

<a name="basic"></a>
## Basic Usage

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    return DataTables::eloquent(User::query())
        ->startsWithSearch()
        ->toJson();
});
```

---

## Search Behavior Comparison

| Search Type | Query Behavior |
|-------------|----------------|
| Default | `%keyword%` (contains) |
| Starts With | `keyword%` (prefix only) |
| Ends With | `%keyword` (suffix only) |
| Exact | `keyword` (exact match) |

---

## Example

When searching for "John":
- **Default search**: Returns "John", "Johnny", "Long John"
- **Starts with search**: Returns "John", "Johnny", only

---

## See Also

- [Smart Search](/docs/{{package}}/{{version}}/smart-search) - Wildcard search
- [Filter Column](/docs/{{package}}/{{version}}/filter-column) - Custom column filtering
