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

<a name="with-count"></a>
## With Count (and Sum)

Use `withCount()` and `withSum()` to efficiently load relation counts and sums, avoiding N+1 queries:

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::withCount('posts'); // Results in posts_count attribute

    return DataTables::eloquent($model)
        ->addColumn('full_name', function (User $user) {
            return $user->first_name . ' ' . $user->last_name;
        })
        ->toJson();
});
```

When defining the column for count/sum values, use `orderable(true)` and `searchable(false)`:

```php
// In your Html builder:
Column::make('posts_count', 'posts_count')
    ->orderable(true)
    ->searchable(false),

// For sums, use Column::make with the sum attribute name:
Column::make('posts_sum_amount', 'posts_sum_amount')
    ->orderable(true)
    ->searchable(false),
```

> [!WARNING]
> Never use `$user->posts()->count()` or `$user->posts()->sum('amount')` in addColumn — this causes N+1 queries. Always use `withCount()`/`withSum()` in the query and access the preloaded attribute.

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

Add computed columns with closures. Use a variable name matching the model (e.g., `$user` for `User`):

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::withCount('posts'); // Use withCount to avoid N+1

    return DataTables::eloquent($model)
        ->addColumn('full_name', function (User $user) {
            return $user->first_name . ' ' . $user->last_name;
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
