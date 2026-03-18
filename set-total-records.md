---
title: Set Total Records
description: Manually set total records count
---

# Set Total Records

In some cases, we need to manually set the total records of our DataTables and skip its internal counting functionality. To achieve this, use the `setTotalRecords($count)` API.

---

<a name="basic"></a>
## Basic Usage

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::query();

    return DataTables::eloquent($model)
        ->setTotalRecords(100)
        ->toJson();
});
```

---

## Common Use Cases

### Pre-calculated Count

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::query();
    $totalCount = User::count();

    return DataTables::eloquent($model)
        ->setTotalRecords($totalCount)
        ->toJson();
});
```

### Cache Count

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;
use Illuminate\Support\Facades\Cache;

Route::get('user-data', function() {
    $model = User::query();
    $totalCount = Cache::remember('users_count', 60, fn () => User::count());

    return DataTables::eloquent($model)
        ->setTotalRecords($totalCount)
        ->toJson();
});
```

---

## See Also

- [Set Filtered Records](/docs/{{package}}/{{version}}/set-filtered-records) - Manually set filtered records
- [Skip Total Records](/docs/{{package}}/{{version}}/skip-total-records) - Skip total records (deprecated)
