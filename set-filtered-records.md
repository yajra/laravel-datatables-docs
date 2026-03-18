---
title: Set Filtered Records
description: Manually set filtered records count
---

# Set Filtered Records

In some cases, we need to manually set the filtered records count of our DataTables and skip its internal counting functionality. To achieve this, use the `setFilteredRecords($count)` API.

---

## Basic Usage

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::query();

    return DataTables::eloquent($model)
        ->setFilteredRecords(100)
        ->toJson();
});
```

---

## Common Use Cases

### Combined with Set Total Records

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::query();
    $totalCount = User::count();
    $filteredCount = User::where('status', 'active')->count();

    return DataTables::eloquent($model)
        ->setTotalRecords($totalCount)
        ->setFilteredRecords($filteredCount)
        ->toJson();
});
```

### Performance Optimization

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;
use Illuminate\Support\Facades\Cache;

Route::get('user-data', function() {
    $model = User::query();
    $filteredCount = Cache::remember('active_users_count', 60, fn () => User::where('status', 'active')->count());

    return DataTables::eloquent($model)
        ->setFilteredRecords($filteredCount)
        ->toJson();
});
```

---

## See Also

- [Set Total Records](/docs/{{package}}/{{version}}/set-total-records) - Manually set total records
- [Skip Total Records](/docs/{{package}}/{{version}}/skip-total-records) - Skip total records (deprecated)
