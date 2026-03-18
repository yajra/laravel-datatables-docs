---
title: Response Resource
description: Use Laravel API Resources with DataTables
---

# Resource Response

DataTables response using Laravel model resource.

---

## Basic Usage

```php
<?php

use Yajra\DataTables\Facades\DataTables;
use App\Models\User;
use App\Http\Resources\UserResource;

Route::get('user-data', function() {
    $users = User::paginate(10);

    $resource = UserResource::collection($users);

    return DataTables::of($resource)->toJson();
});
```

---

## Example Response

```json
{
    "draw": 10,
    "recordsTotal": 10,
    "recordsFiltered": 10,
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

## See Also

- [Fractal Response](/docs/{{package}}/{{version}}/response-fractal) - Using Fractal transformers
- [Response Object](/docs/{{package}}/{{version}}/response-object) - Default response format
