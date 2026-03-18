---
title: Array Response
description: Return DataTables response as a plain array instead of objects
---

# Array Response

The default response of the package is an array of objects. If you prefer to return an array response, use `make(false)`.

---

<a name="basic"></a>
## Basic Usage

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::query();

    return DataTables::eloquent($model)
        ->addColumn('intro', 'Hi {{$name}}!')
        ->make(false);
});
```

---

## Example Response

```json
{
    "draw": 1,
    "recordsTotal": 10,
    "recordsFiltered": 3,
    "data": [
        [414, "Abel Cole-name", "jklein@block.com", "2016-07-31 23:26:10", "2016-07-31 23:26:10"],
        [267, "Addie Satterfield-name", "cassin.eva@pouros.info", "2016-07-31 23:26:00", "2016-07-31 23:26:00"],
        [120, "Adeline Mayert-name", "rice.elian@abshire.com", "2016-07-31 23:25:50", "2016-07-31 23:25:50"]
    ]
}
```

---

## When to Use Array Response

| Use Case | Recommendation |
|----------|----------------|
| Legacy JavaScript integration | ✅ Use array response |
| Simple frontend parsing | ✅ Use array response |
| Complex nested data | ❌ Use object response |
| API responses | ❌ Use object response |

---

## See Also

- [Object Response](/docs/{{package}}/{{version}}/response-object) - Default object response
- [Additional Data Response](/docs/{{package}}/{{version}}/response-with) - Add extra data to response
