---
title: Eloquent Data Source
description: Use Laravel Eloquent models as data source for DataTables
---

# Eloquent Data Source

Use Laravel's Eloquent Model as the data source for your DataTables. The `Yajra\DataTables\EloquentDataTable` class handles the conversion of your Eloquent Model into a DataTables-compatible response.

---

<a name="quick-start"></a>
## Quick Start

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::query();

    return DataTables::eloquent($model)->toJson();
});
```

---

<a name="relationships"></a>
## With Relationships

Eager load relationships when building your query:

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::with(['posts', 'roles']);

    return DataTables::eloquent($model)->toJson();
});
```

> [!TIP]
> See [Relationships](/docs/{{package}}/{{version}}/relationships) for detailed information on searching and sorting eager loaded relationships.

---

<a name="scopes"></a>
## With Query Scopes

Apply model scopes for filtered results:

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::active()->verified();

    return DataTables::eloquent($model)->toJson();
});
```

---

<a name="custom-columns"></a>
## Adding Custom Columns

Add computed columns with closures:

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::query();

    return DataTables::eloquent($model)
        ->addColumn('full_name', function (User $user) {
            return $user->first_name . ' ' . $user->last_name;
        })
        ->addColumn('posts_count', function (User $user) {
            return $user->posts->count();
        })
        ->toJson();
});
```

> [!NOTE]
> Added columns are computed and not part of the database, so search and sort are disabled by default. Use `editColumn` if you need search/sort functionality.

---

<a name="filtering"></a>
## Filtering

Filter results using Eloquent where clauses:

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::where('active', true)
        ->whereNotNull('email_verified_at');

    return DataTables::eloquent($model)->toJson();
});
```

---

<a name="when-to-use"></a>
## When to Use Eloquent

| Use Case | Recommendation |
|----------|----------------|
| Standard CRUD tables | ✅ Perfect for Eloquent |
| Relationships | ✅ Automatic eager loading |
| Model scopes | ✅ Clean, reusable filtering |
| Complex JOINs | Consider Query Builder |

---

## See Also

- [Query Builder Data Source](/docs/{{package}}/{{version}}/engine-query) - Use Query Builder
- [Collection Data Source](/docs/{{package}}/{{version}}/engine-collection) - Use Laravel Collections
- [Add Column](/docs/{{package}}/{{version}}/add-column) - Add custom columns
- [Relationships](/docs/{{package}}/{{version}}/relationships) - Eager loading relationships
