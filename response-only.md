---
title: Only Response
description: Return only selected columns in DataTables response
---

# Only Response

Get only selected columns in response.

---

## Basic Usage

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::query();

    return DataTables::eloquent($model)
        ->only(['id', 'name'])
        ->toJson();
});
```

---

## Example Response

```json
{
    "draw": 2,
    "recordsTotal": 10,
    "recordsFiltered": 2,
    "data": [
        {
            "id": 476,
            "name": "Esmeralda Kulas"
        },
        {
            "id": 6,
            "name": "Zachery Muller"
        }
    ]
}
```

---

## See Also

- [Object Response](/docs/{{package}}/{{version}}/response-object) - Default response format
- [Remove Column](/docs/{{package}}/{{version}}/remove-column) - Remove specific columns
