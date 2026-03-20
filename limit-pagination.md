---
title: Limit Pagination
description: Custom pagination using LIMIT without OFFSET in DataTables
---

# Limit Pagination

The `limit()` API allows you to implement custom pagination using only `LIMIT` without `OFFSET`. This is useful when you want to implement cursor-based pagination or infinite scroll patterns.

---

<a name="basic"></a>
## Basic Usage

```php
use Yajra\DataTables\Facades\DataTables;
use Illuminate\Support\Facades\DB;

Route::get('user-data', function() {
    $query = DB::table('users')->select('*');

    return DataTables::query($query)
        ->limit(function (Builder $builder) {
            // Custom LIMIT logic
            $builder->limit(100);
        })
        ->toJson();
});
```

---

<a name="cursor-pagination"></a>
## Cursor-Based Pagination

Implement cursor-based pagination by tracking the last seen ID:

```php
use Yajra\DataTables\Facades\DataTables;
use Illuminate\Support\Facades\DB;

Route::get('user-data', function() {
    $lastId = request()->input('cursor');

    $query = DB::table('users')
        ->select('*')
        ->orderBy('id');

    if ($lastId) {
        $query->where('id', '>', $lastId);
    }

    return DataTables::query($query)
        ->limit(function (Builder $builder) {
            $builder->limit(50);
        })
        ->toJson();
});
```

---

<a name="time-based"></a>
## Time-Based Pagination

Paginate through records based on timestamps:

```php
use Yajra\DataTables\Facades\DataTables;
use Illuminate\Support\Facades\DB;
use Carbon\Carbon;

Route::get('activity-data', function() {
    $before = request()->input('before');

    $query = DB::table('activities')
        ->select('*')
        ->orderBy('created_at', 'desc');

    if ($before) {
        $query->where('created_at', '<', Carbon::parse($before));
    }

    return DataTables::query($query)
        ->limit(function (Builder $builder) {
            $builder->limit(25);
        })
        ->toJson();
});
```

---

## Comparison

| Feature | Standard Pagination | LIMIT Only |
|---------|-------------------|------------|
| OFFSET support | ✅ Yes | ❌ No |
| Cursor-based | ❌ No | ✅ Yes |
| Infinite scroll | Limited | ✅ Perfect |
| Performance at scale | Degrades | ✅ Constant |

---

## See Also

- [Skip Paging](/docs/{{package}}/{{version}}/skip-paging) - Skip pagination entirely
- [General Settings](/docs/{{package}}/{{version}}/general-settings) - Configuration options
