---
title: White List Columns
description: Enable sorting and searching only on specific columns
---

# White Listing Columns

The whitelist feature allows you to explicitly define which columns can be used for sorting and searching. Only columns defined in the whitelist will be searchable and sortable.

---

<a name="basic"></a>
## Basic Usage

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::query();

    return DataTables::eloquent($model)
        ->whitelist(['name', 'email'])
        ->toJson();
});
```

---

## Common Use Cases

### Limit Searchable Columns

```php
return DataTables::eloquent(User::query())
    ->whitelist(['name', 'email', 'status'])
    ->toJson();
```

### Performance Optimization

```php
// Only index columns will be sorted and searched
return DataTables::eloquent(User::query())
    ->whitelist(['id', 'name', 'email', 'created_at'])
    ->toJson();
```

---

## See Also

- [Black Listing Columns](/docs/{{package}}/{{version}}/blacklist) - Disable sorting/searching on specific columns
- [Column Configuration](/docs/{{package}}/{{version}}/html-builder-column) - Column attributes
