---
title: Query Builder Data Source
description: Use Laravel Query Builder as data source for DataTables
---

# Query Builder Data Source

Use Laravel's Query Builder as the data source for your DataTables. The `Yajra\DataTables\QueryDataTable` class handles the conversion of your Query Builder into a DataTables-compatible response.

---

<a name="quick-start"></a>
## Quick Start

```php
use Yajra\DataTables\Facades\DataTables;
use Illuminate\Support\Facades\DB;

Route::get('user-data', function() {
    $query = DB::table('users');

    return DataTables::query($query)->toJson();
});
```

---

<a name="custom-columns"></a>
## Adding Custom Columns

Add computed columns with closures:

```php
use Yajra\DataTables\Facades\DataTables;
use Illuminate\Support\Facades\DB;

Route::get('user-data', function() {
    $query = DB::table('users');

    return DataTables::query($query)
        ->addColumn('full_name', function (object $row) {
            return $row->first_name . ' ' . $row->last_name;
        })
        ->addColumn('action', function (object $row) {
            return '<button>Edit</button>';
        })
        ->toJson();
});
```

> [!NOTE]
> Added columns are computed and not part of the database, so search and sort are disabled by default. Use `editColumn` if you need search/sort functionality.

---

<a name="filtering"></a>
## Filtering

Filter results in your query before passing to DataTables:

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

<a name="joins"></a>
## With Join Statements

Join related tables when building your query:

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

<a name="when-to-use"></a>
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
