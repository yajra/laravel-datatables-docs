---
title: Smart Search
description: Configure DataTables search behavior with wildcards and search modes
---

# Smart Search

Smart search controls how DataTables matches search keywords against your data. The package offers multiple search modes to suit different use cases.

---

## Search Modes Overview

| Mode | Behavior | Use Case |
|------|----------|----------|
| **Default (Smart)** | `%keyword%` - wraps with wildcards | General search |
| **Starts With** | `keyword%` - prefix only | Autocomplete, suggestions |
| **Ends With** | `%keyword` - suffix only | File extensions, codes |
| **Exact Match** | `keyword` - no wildcards | IDs, codes, exact terms |

---

## Basic Usage

By default, smart search is **enabled** with automatic wildcards:

```php
Route::get('user-data', function () {
    return DataTables::eloquent(User::query())->toJson();
});
```

Searching for "john" becomes `%john%`.

---

## Disable Smart Search

Use `->smart(false)` for exact matching:

```php
Route::get('user-data', function () {
    return DataTables::eloquent(User::query())
        ->smart(false)
        ->toJson();
});
```

Searching for "john" matches exactly "john", not "Johnny".

---

## Starts With Search

Search for records that **start with** the given keyword:

```php
Route::get('user-data', function () {
    return DataTables::eloquent(User::query())
        ->startsWithSearch()
        ->toJson();
});
```

### Behavior Comparison

When searching "John":

| Search Type | SQL Pattern | Matches |
|-------------|-------------|---------|
| Default (Contains) | `%John%` | "John", "Johnny", "Long John" |
| Starts With | `John%` | "John", "Johnny" only |
| Ends With | `%John` | "Long John" only |
| Exact Match | `John` | "John" only |

---

## How Smart Search Works

Smart search automatically transforms user input:

1. **Wildcard Wrapping** - Terms are wrapped with `%` for partial matching
2. **Multi-word AND Logic** - Spaces split terms, all must match

| User Types | Generated SQL |
|------------|---------------|
| `john` | `WHERE name LIKE '%john%'` |
| `john doe` | `WHERE name LIKE '%john%' AND name LIKE '%doe%'` |

---

## Global Configuration

Configure defaults in `config/datatables.php`:

```php
return [
    // Wrap search terms with % wildcards
    'smart' => true,

    // Case insensitive searching
    'case_insensitive' => true,

    // Use wildcards in search
    'use_wildcards' => false,
];
```

---

## When to Use

- **Use Smart Search (default)**: When you want users to find results by typing any part of a word
- **Use Starts With**: When building autocomplete fields or suggestions
- **Disable Smart Search**: When searching for exact IDs, codes, or precise values

---

## See Also

- [Filter Column](/docs/{{package}}/{{version}}/filter-column) - Custom column filtering
- [Manual Search](/docs/{{package}}/{{version}}/manual-search) - Custom filtering logic
- [Regex Search](/docs/{{package}}/{{version}}/regex) - Regular expression search
