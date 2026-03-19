---
title: Regex Search
description: Enable regular expression pattern matching in DataTables search
---

# Regex Search

Enable regular expression (regex) search for advanced pattern matching beyond simple wildcards.

> [!NOTE]
> Regex search is supported on Laravel DB drivers: **MySQL**, **SQLite**, and **Oracle**.

---

## Frontend Configuration

Enable regex in your DataTables JavaScript initialization:

```javascript
$('#users-table').DataTable({
    processing: true,
    serverSide: true,
    ajax: '/user-data',
    columns: [
        { data: 'id', name: 'id' },
        { data: 'name', name: 'name' },
        { data: 'email', name: 'email' },
        { data: 'created_at', name: 'created_at' }
    ],
    search: {
        regex: true
    }
});
```

---

## Case Sensitivity

By default, regex patterns are case-sensitive. Enable case-insensitive matching:

```javascript
search: {
    regex: true,
    caseInsensitive: true
}
```

---

## Backend Configuration

No special backend configuration is required. The package automatically handles regex patterns:

```php
// Works automatically - no changes needed
Route::get('user-data', fn () => DataTables::eloquent(User::query())->toJson());
```

---

## Common Patterns

### Email Addresses

Match emails from a specific domain:

```regex
.*@example\.com$
```

**Example search:** `.*@example\.com$` → matches "john@example.com", "jane@example.com"

### Numbers in Range (1-80)

Match numbers from 1 to 80:

```regex
^([1-9]|[1-7][0-9]|80)$
```

| Matches | Does NOT Match |
|---------|----------------|
| 1, 5, 42, 80 | 0, 81, 100, abc |

### Starts With

Match values beginning with specific text:

```regex
^John
```

**Example search:** `^John` → matches "John", "Johnny", "John Doe"

### Ends With

Match values ending with specific text:

```regex
\.pdf$
```

**Example search:** `\.pdf$` → matches "document.pdf", "file.pdf"

---

## Quick Reference

| Need | Solution |
|------|----------|
| Complex pattern matching | Regex with `search: { regex: true }` |
| Simple wildcard matching | [Smart Search](/docs/{{package}}/{{version}}/smart-search) |
| Column-specific custom logic | [Filter Column](/docs/{{package}}/{{version}}/filter-column) |
| Complete query control | [Manual Search](/docs/{{package}}/{{version}}/manual-search) |

> [!NOTE]
> Supported drivers: **MySQL**, **SQLite**, **Oracle**. Not available for PostgreSQL.

---

## See Also

- [Smart Search](/docs/{{package}}/{{version}}/smart-search) - Wildcard search
- [Manual Search](/docs/{{package}}/{{version}}/manual-search) - Custom filtering logic
- [Filter Column](/docs/{{package}}/{{version}}/filter-column) - Custom column filtering
