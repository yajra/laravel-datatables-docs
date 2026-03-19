---
title: Filter Column
description: Implement custom column filtering for DataTables
---

# Filter Column

Use the `filterColumn()` API when you need custom search logic for a specific column while letting the package handle other columns automatically.

---

<a name="basic-usage"></a>
## Basic Usage

Filter a computed column that combines multiple fields:

```php
Route::get('user-data', function () {
    $model = User::query()->select([
        'id',
        DB::raw("CONCAT(first_name, ' ', last_name) as fullname"),
        'email',
        'created_at',
    ]);

    return DataTables::eloquent($model)
        ->filterColumn('fullname', fn(Builder $query, $keyword) =>
            $query->whereRaw(
                "CONCAT(first_name, ' ', last_name) LIKE ?",
                ["%{$keyword}%"]
            )
        )
        ->toJson();
});
```

### Closure Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `$query` | Builder | The query builder instance |
| `$keyword` | string | The search keyword from DataTables |

---

## Multiple Custom Columns

Define custom filters for different columns:

```php
Route::get('user-data', function () {
    return DataTables::eloquent(User::query())
        ->filterColumn('name', fn(Builder $query, $keyword) =>
            $query->where('name', 'like', "%{$keyword}%")
        )
        ->filterColumn('email', fn(Builder $query, $keyword) =>
            $query->where('email', 'like', "%{$keyword}%")
        )
        ->filterColumn('status', fn(Builder $query, $keyword) =>
            $query->where('status', $keyword)
        )
        ->toJson();
});
```

---

## OR Conditions Across Columns

Search across multiple columns with OR logic:

```php
Route::get('user-data', function () {
    return DataTables::eloquent(User::query())
        ->filterColumn('name', function (Builder $query, $keyword) {
            $query->where(fn(Builder $q) => $q
                ->where('first_name', 'like', "%{$keyword}%")
                ->orWhere('last_name', 'like', "%{$keyword}%")
                ->orWhereRaw(
                    "CONCAT(first_name, ' ', last_name) LIKE ?",
                    ["%{$keyword}%"]
                )
            );
        })
        ->toJson();
});
```

---

## Numeric Comparison

Filter using comparison operators:

```php
Route::get('user-data', function () {
    return DataTables::eloquent(User::query())
        ->filterColumn('price', fn(Builder $query, $keyword) =>
            $query->where('price', '>=', $keyword)
        )
        ->toJson();
});
```

---

## Date Filtering

Parse and filter date values:

```php
Route::get('user-data', function () {
    return DataTables::eloquent(User::query())
        ->filterColumn('created_at', fn(Builder $query, $keyword) =>
            $query->whereDate('created_at', Carbon::parse($keyword)->startOfDay())
        )
        ->toJson();
});
```

---

## With Table Aliases

When using table aliases in your query:

```php
Route::get('user-data', function () {
    $model = DB::table('users as u')->select('u.*');

    return DataTables::query($model)
        ->filterColumn('user_name', fn(Builder $query, $keyword) =>
            $query->whereRaw(
                "CONCAT(u.first_name, ' ', u.last_name) LIKE ?",
                ["%{$keyword}%"]
            )
        )
        ->toJson();
});
```

> [!WARNING]
> Always include `select('table.*')` when using table aliases to avoid id column conflicts.

---

## Quick Reference

| Need | Solution |
|------|----------|
| Custom logic for specific columns | `->filterColumn('name', fn(Builder $q, $k) => ...)` |
| Complete query control | [Manual Search](/docs/{{package}}/{{version}}/manual-search) |
| Simple wildcard matching | [Smart Search](/docs/{{package}}/{{version}}/smart-search) |
| Pattern-based matching | [Regex Search](/docs/{{package}}/{{version}}/regex) |

---

## See Also

- [Manual Search](/docs/{{package}}/{{version}}/manual-search) - Custom search logic
- [Smart Search](/docs/{{package}}/{{version}}/smart-search) - Wildcard search
- [Regex Search](/docs/{{package}}/{{version}}/regex) - Regular expression search
