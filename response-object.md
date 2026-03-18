---
title: Object Response
description: Default DataTables response format as JSON objects
---

# Object Response

The default response of Laravel DataTables is a JSON object. Use `toJson()` to convert the response to JSON format.

---

## Basic Usage

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::query();

    return DataTables::eloquent($model)
        ->addColumn('intro', 'Hi {{$name}}!')
        ->toJson();
});
```

---

## Response Structure

```json
{
    "draw": 2,
    "recordsTotal": 10,
    "recordsFiltered": 3,
    "data": [
        {
            "id": 476,
            "name": "Esmeralda Kulas",
            "email": "abbott.cali@heaney.info",
            "created_at": "2016-07-31 23:26:14",
            "updated_at": "2016-07-31 23:26:14",
            "deleted_at": null,
            "superior_id": 0
        },
        {
            "id": 6,
            "name": "Zachery Muller",
            "email": "abdullah.koelpin@yahoo.com",
            "created_at": "2016-07-31 23:25:43",
            "updated_at": "2016-07-31 23:25:43",
            "deleted_at": null,
            "superior_id": 1
        }
    ]
}
```

---

## Response Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `draw` | int | Draw counter for DataTables |
| `recordsTotal` | int | Total records before filtering |
| `recordsFiltered` | int | Total records after filtering |
| `data` | array | Array of row objects |
| `error` | string | Error message if any (optional) |

---

## See Also

- [Array Response](/docs/{{package}}/{{version}}/response-array) - Return as array instead of objects
- [Additional Data Response](/docs/{{package}}/{{version}}/response-with) - Add extra data to response
