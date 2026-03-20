---
title: Ignore Model Getters
description: Prevent getters and mutators from being applied in DataTables response
---

# Ignore Model Getters

The `ignoreGetters()` API prevents Eloquent model getters and mutators from being applied when converting models to array for the DataTables response.

---

<a name="basic"></a>
## Basic Usage

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    return DataTables::eloquent(User::query())
        ->ignoreGetters()
        ->toJson();
});
```

---

<a name="use-case"></a>
## Use Case

When you need to access the raw attribute values without any transformation:

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    // Model has an accessor that formats the name
    // class User extends Model {
    //     public function getNameAttribute($value) {
    //         return strtoupper($value);
    //     }
    // }

    return DataTables::eloquent(User::query())
        ->ignoreGetters()
        ->toJson();
});
```

In this example, `name` will return the raw database value instead of the formatted uppercase value from the accessor.

---

<a name="with-relations"></a>
## With Relations

When eager loading relations, `ignoreGetters` also applies to related models:

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    return DataTables::eloquent(
        User::with(['posts', 'roles'])
    )
        ->ignoreGetters()
        ->toJson();
});
```

---

## See Also

- [Add Column](/docs/{{package}}/{{version}}/add-column) - Adding custom columns
- [Edit Column](/docs/{{package}}/{{version}}/edit-column) - Modifying existing columns
