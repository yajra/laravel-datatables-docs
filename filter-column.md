---
title: Filter Column
description: Implement custom column filtering for DataTables
---


# Filter Column

In some cases, you need to implement custom search logic for a specific column. The `filterColumn` API allows you to define custom SQL WHERE clauses for column-specific searches.

---

<a name="basic"></a>
## Basic Usage

```php
use Yajra\DataTables\Facades\DataTables;
use Illuminate\Support\Facades\DB;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::query()->select([
        'id',
        DB::raw("CONCAT(users.first_name, ' ', users.last_name) as fullname"),
        'email',
        'created_at',
        'updated_at',
    ]);

    return DataTables::eloquent($model)
        ->filterColumn('fullname', function($query, $keyword) {
            $sql = "CONCAT(users.first_name, ' ', users.last_name) LIKE ?";
            $query->whereRaw($sql, ["%{$keyword}%"]);
        })
        ->toJson();
});
```

---

## With Multiple Columns

Filter multiple columns independently:

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::query();

    return DataTables::eloquent($model)
        ->filterColumn('name', function($query, $keyword) {
            $query->where('name', 'LIKE', "%{$keyword}%");
        })
        ->filterColumn('email', function($query, $keyword) {
            $query->where('email', 'LIKE', "%{$keyword}%");
        })
        ->filterColumn('status', function($query, $keyword) {
            $query->where('status', $keyword);
        })
        ->toJson();
});
```

---

## With OR Conditions

Search across multiple columns with OR:

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::query();

    return DataTables::eloquent($model)
        ->filterColumn('name', function($query, $keyword) {
            $query->where(function($q) use ($keyword) {
                $q->where('first_name', 'LIKE', "%{$keyword}%")
                  ->orWhere('last_name', 'LIKE', "%{$keyword}%")
                  ->orWhereRaw(
                      "CONCAT(first_name, ' ', last_name) LIKE ?",
                      ["%{$keyword}%"]
                  );
            });
        })
        ->toJson();
});
```

---

## With Numeric Comparison

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::query();

    return DataTables::eloquent($model)
        ->filterColumn('price', function($query, $keyword) {
            $query->where('price', '>=', $keyword);
        })
        ->toJson();
});
```

---

## With Date Range

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;
use Carbon\Carbon;

Route::get('user-data', function() {
    $model = User::query();

    return DataTables::eloquent($model)
        ->filterColumn('created_at', function($query, $keyword) {
            $date = Carbon::parse($keyword)->startOfDay();
            $query->whereDate('created_at', '=', $date);
        })
        ->toJson();
});
```

---

## With Column Alias

When using table aliases or computed columns:

```php
use Yajra\DataTables\Facades\DataTables;
use Illuminate\Support\Facades\DB;

Route::get('user-data', function() {
    $model = DB::table('users as u')
        ->select('u.*');

    return DataTables::query($model)
        ->filterColumn('user_name', function($query, $keyword) {
            $query->whereRaw("CONCAT(u.first_name, ' ', u.last_name) LIKE ?", ["%{$keyword}%"]);
        })
        ->toJson();
});
```

---

## Closure Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `$query` | Builder | The query builder instance |
| `$keyword` | string | The search keyword from DataTables |

---

## See Also

- [Manual Search](/docs/{{package}}/{{version}}/manual-search) - Custom search logic
- [Smart Search](/docs/{{package}}/{{version}}/smart-search) - Wildcard search
- [Regex Search](/docs/{{package}}/{{version}}/regex) - Regular expression search
