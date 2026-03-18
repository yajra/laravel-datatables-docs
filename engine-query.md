---
title: Query Builder Data Source
description: Use Laravel Query Builder as data source for DataTables
---

# Query Builder Data Source

You can use Laravel's Query Builder as data source for your DataTables. The `Yajra\DataTables\QueryDataTable` class handles the conversion of your Query Builder into a DataTables-compatible response.

---

## Usage Methods

### Method 1: Factory Pattern

```php
use Yajra\DataTables\Facades\DataTables;
use Illuminate\Support\Facades\DB;

Route::get('user-data', function() {
    $query = DB::table('users');

    return DataTables::of($query)->toJson();
});
```

### Method 2: Facade with Query

```php
use Yajra\DataTables\Facades\DataTables;
use Illuminate\Support\Facades\DB;

Route::get('user-data', function() {
    $query = DB::table('users');

    return DataTables::query($query)->toJson();
});
```

### Method 3: Dependency Injection

```php
use Yajra\DataTables\DataTables;
use Illuminate\Support\Facades\DB;

Route::get('user-data', function(DataTables $dataTables) {
    $query = DB::table('users');

    return $dataTables->query($query)->toJson();
});
```

### Method 4: IoC Container

```php
use Yajra\DataTables\Facades\DataTables;
use Illuminate\Support\Facades\DB;

Route::get('user-data', function() {
    $query = DB::table('users');

    return app('datatables')->query($query)->toJson();
});
```

### Method 5: Direct Instance

```php
use Yajra\DataTables\QueryDataTable;
use Illuminate\Support\Facades\DB;

Route::get('user-data', function() {
    $query = DB::table('users');

    return (new QueryDataTable($query))->toJson();
});
```

---

## With Join Statements

```php
use Yajra\DataTables\Facades\DataTables;
use Illuminate\Support\Facades\DB;

Route::get('user-data', function() {
    $query = DB::table('users')
        ->join('posts', 'users.id', '=', 'posts.user_id')
        ->select('users.*', DB::raw('COUNT(posts.id) as post_count'))
        ->groupBy('users.id');

    return DataTables::query($query)->toJson();
});
```

---

## With Raw Expressions

```php
use Yajra\DataTables\Facades\DataTables;
use Illuminate\Support\Facades\DB;

Route::get('user-data', function() {
    $query = DB::table('users')
        ->select([
            'id',
            DB::raw("CONCAT(first_name, ' ', last_name) as full_name"),
            'email',
            DB::raw("DATE_FORMAT(created_at, '%Y-%m-%d') as created_date"),
        ]);

    return DataTables::query($query)->toJson();
});
```

---

## With Subqueries

```php
use Yajra\DataTables\Facades\DataTables;
use Illuminate\Support\Facades\DB;

Route::get('user-data', function() {
    $query = DB::table('users')
        ->select('users.*')
        ->selectSub(function ($query) {
            $query->from('posts')
                  ->selectRaw('COUNT(*)')
                  ->whereColumn('user_id', 'users.id');
        }, 'post_count');

    return DataTables::query($query)->toJson();
});
```

---

## Filtering Results

```php
use Yajra\DataTables\Facades\DataTables;
use Illuminate\Support\Facades\DB;

Route::get('user-data', function() {
    $query = DB::table('users')
        ->where('active', true)
        ->where('email_verified_at', '!=', null);

    return DataTables::query($query)->toJson();
});
```

---

## Response Structure

```json
{
    "draw": 1,
    "recordsTotal": 100,
    "recordsFiltered": 50,
    "data": [
        {
            "id": 1,
            "name": "John Doe",
            "email": "john@example.com",
            "created_at": "2024-01-01 00:00:00"
        }
    ]
}
```

---

## When to Use Query Builder

| Use Case | Recommendation |
|----------|----------------|
| Complex JOINs | ✅ Perfect for Query Builder |
| Raw SQL | ✅ Great for custom queries |
| Large datasets | ✅ Efficient server-side processing |
| Simple CRUD tables | Consider Eloquent |

---

## See Also

- [Eloquent Data Source](/docs/{{package}}/{{version}}/engine-eloquent) - Use Eloquent models
- [Collection Data Source](/docs/{{package}}/{{version}}/engine-collection) - Use Collections
- [Filter Column](/docs/{{package}}/{{version}}/filter-column) - Custom column filtering
- [Manual Search](/docs/{{package}}/{{version}}/manual-search) - Custom search logic
