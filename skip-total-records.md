---
title: Skip Total Records
description: Skip total records count query for performance
---

# Skip Total Records

> [!WARNING]
> This feature is deprecated in v10. Use `setTotalRecords` instead.

Skip total records aims to improve DataTables response time by skipping the total records count query and setting its value equals to the filtered total records.

---

## Example

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::query();

    return DataTables::eloquent($model)
        ->skipTotalRecords()
        ->toJson();
});
```

---

## See Also

- [Set Total Records](/docs/{{package}}/{{version}}/set-total-records) - Manually set total records
- [Set Filtered Records](/docs/{{package}}/{{version}}/set-filtered-records) - Manually set filtered records
