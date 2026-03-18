---
title: Black List Columns
description: Disable sorting and searching on specific columns
---

# Black Listing Columns

The blacklist feature allows you to explicitly disable sorting and searching on specific columns. Columns defined in the blacklist will be excluded from sorting and searching operations.

---

<a name="basic"></a>
## Basic Usage

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::query();

    return DataTables::eloquent($model)
        ->blacklist(['password', 'name'])
        ->toJson();
});
```

---

## Common Use Cases

### Exclude Sensitive Data

```php
return DataTables::eloquent(User::query())
    ->blacklist(['password', 'api_token', 'remember_token'])
    ->toJson();
```

### Exclude Action Columns

```php
return DataTables::eloquent(User::query())
    ->blacklist(['action', 'edit', 'delete'])
    ->toJson();
```

---

## See Also

- [White Listing Columns](/docs/{{package}}/{{version}}/whitelist) - Enable sorting/searching only on specific columns
- [Column Configuration](/docs/{{package}}/{{version}}/html-builder-column) - Column attributes
