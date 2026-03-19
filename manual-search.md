---
title: Manual Search
description: Implement custom filtering logic in DataTables
---

# Manual Search

Use the `filter()` API when you need complete control over how queries are filtered, bypassing the package's automatic search handling.

---

## Manual Search Only

Disable global search and implement your own filtering:

```php
Route::get('user-data', function () {
    return DataTables::eloquent(User::query())
        ->filter(function ($query) {
            $query->when(request('name'), fn($q) => 
                $q->where('name', 'like', "%" . request('name') . "%")
            );
            $query->when(request('email'), fn($q) => 
                $q->where('email', 'like', "%" . request('email') . "%")
            );
        })
        ->toJson();
});
```

---

## Manual Search with Global Search

Enable global search alongside your custom filters by passing `true` as the second argument:

```php
Route::get('user-data', function () {
    return DataTables::eloquent(User::query())
        ->filter(function ($query) {
            $query->when(request('name'), fn($q) => 
                $q->where('name', 'like', "%" . request('name') . "%")
            );
        }, true) // Keep global search enabled
        ->toJson();
});
```

---

## Advanced Filtering Examples

### Filter by Multiple Criteria

```php
Route::get('user-data', function () {
    return DataTables::eloquent(User::query())
        ->filter(function ($query) {
            $query->when(request('status'), fn($q, $v) => 
                $q->where('status', $v)
            );
            $query->when(request('from_date'), fn($q, $v) => 
                $q->whereDate('created_at', '>=', $v)
            );
            $query->when(request('to_date'), fn($q, $v) => 
                $q->whereDate('created_at', '<=', $v)
            );
            $query->when(request('has_posts'), fn($q) => 
                $q->has('posts')
            );
        })
        ->toJson();
});
```

### OR Conditions

Search across multiple columns with OR logic:

```php
Route::get('user-data', function () {
    return DataTables::eloquent(User::query())
        ->filter(function ($query) {
            $query->when(request('search'), function ($q, $search) {
                $q->where(fn($q) => $q
                    ->where('name', 'like', "%{$search}%")
                    ->orWhere('email', 'like', "%{$search}%")
                );
            });
        })
        ->toJson();
});
```

---

## Quick Reference

| Need | Solution |
|------|----------|
| Complete query control | `->filter()` without second argument |
| Custom filters + global search | `->filter(..., true)` |
| Specific column logic | [Filter Column](/docs/{{package}}/{{version}}/filter-column) |
| Wildcard matching | [Smart Search](/docs/{{package}}/{{version}}/smart-search) |

---

## See Also

- [Filter Column](/docs/{{package}}/{{version}}/filter-column) - Custom column filtering
- [Smart Search](/docs/{{package}}/{{version}}/smart-search) - Wildcard search
- [Regex Search](/docs/{{package}}/{{version}}/regex) - Regular expression search
