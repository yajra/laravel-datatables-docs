---
title: Remove Column
description: Remove columns from your DataTables response
---

# Remove Column

You can remove columns from your DataTables response using the `removeColumn` API. This is useful for excluding sensitive or unnecessary data from the response.

---

## Basic Usage

Remove a single column:

```php
use Yajra\DataTables\Facades\DataTables;

Route::get('user-data', function() {
    $model = App\User::query();

    return DataTables::eloquent($model)
        ->removeColumn('password')
        ->toJson();
});
```

---

## Remove Multiple Columns

Chain multiple removes or pass an array:

```php
return DataTables::eloquent($model)
    ->removeColumn('password')
    ->removeColumn('remember_token')
    ->toJson();
```

Or use multiple arguments:

```php
return DataTables::eloquent($model)
    ->removeColumn('password', 'remember_token', 'api_token')
    ->toJson();
```

---

## Common Use Cases

### Remove Sensitive Data

```php
return DataTables::eloquent($model)
    ->removeColumn('password')
    ->removeColumn('api_token')
    ->removeColumn('secret_question')
    ->removeColumn('secret_answer')
    ->toJson();
```

### Remove Internal Fields

```php
return DataTables::eloquent($model)
    ->removeColumn('deleted_at')
    ->removeColumn('created_by')
    ->removeColumn('updated_by')
    ->toJson();
```

### Combine with AddColumn

```php
return DataTables::eloquent($model)
    ->removeColumn('first_name')
    ->removeColumn('last_name')
    ->addColumn('full_name', function (User $user) {
        return $user->first_name . ' ' . $user->last_name;
    })
    ->toJson();
```

---

## Alternative: Make Hidden

You can also use Laravel's `makeHidden` method on Eloquent models:

```php
return DataTables::eloquent($model)
    ->makeHidden(['password', 'remember_token'])
    ->toJson();
```

> 💡 **Tip**: `makeHidden` is more performant as it happens at the model level before array conversion.

---

## Column Visibility Comparison

| Method | When Data is Removed | Use Case |
|--------|---------------------|----------|
| `removeColumn` | During DataTables processing | Exclude from response only |
| `makeHidden` | During model serialization | Always exclude from JSON |
| `$hidden` property | On model serialization | Persistent model hiding |

---

## See Also

- [Make Hidden](/docs/{{package}}/{{version}}/make-hidden) - Hide Eloquent attributes
- [Add Column](/docs/{{package}}/{{version}}/add-column) - Add computed columns
- [Raw Columns](/docs/{{package}}/{{version}}/raw-columns) - Allow HTML rendering
