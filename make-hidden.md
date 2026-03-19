---
title: Make Columns Hidden
description: Hide specific columns from the DataTables response
---

# Hide Attributes from JSON

When you are working with Eloquent Object, you can apply the `makeHidden()` ([Laravel documentation](https://laravel.com/docs/master/eloquent-serialization#hiding-attributes-from-json)) function before converting your object toArray().

This can prevent overriding attributes to be computed and increase the performance of converting the object into an array.

---

<a name="basic"></a>
## Basic Usage

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::with('posts.comment')->query();

    return DataTables::eloquent($model)
        ->makeHidden('posts')
        ->toJson();
});
```

---

<a name="common-use-cases"></a>
## Common Use Cases

### Hide Sensitive Data

```php
return DataTables::eloquent(User::query())
    ->makeHidden(['password', 'api_token', 'remember_token'])
    ->toJson();
```

### Hide Large Relationships

```php
return DataTables::eloquent(Post::query())
    ->makeHidden(['comments', 'metadata'])
    ->toJson();
```

### Combine with Make Visible

```php
return DataTables::eloquent(User::query())
    ->makeHidden('posts')  // Hide from serialization
    ->makeVisible('secret_field')  // Force visible
    ->toJson();
```

---

<a name="performance-benefits"></a>
## Performance Benefits

| Method | Performance | Use Case |
|--------|-------------|----------|
| `makeHidden` | ✅ Faster | Prevent attribute computation |
| `makeHidden` | ✅ Better | Large relationships |

> [!TIP]
> Using `makeHidden` at the model level is more performant than filtering columns after serialization.

---

## See Also

- [Remove Column](/docs/{{package}}/{{version}}/remove-column) - Remove columns from response
- [Raw Columns](/docs/{{package}}/{{version}}/raw-columns) - Allow HTML rendering
