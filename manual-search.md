---
title: Manual Search
description: Implement custom filtering logic in DataTables
---

# Manual Search

You can optionally write your own manual filtering functions and disable global search of the package. To achieve this, use the `filter` API.

---

<a name="manual"></a>
## Manual Searching without Global Search

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::query();

    return DataTables::eloquent($model)
        ->filter(function ($query) {
            if (request()->has('name')) {
                $query->where('name', 'like', "%" . request('name') . "%");
            }

            if (request()->has('email')) {
                $query->where('email', 'like', "%" . request('email') . "%");
            }
        })
        ->toJson();
});
```

---

## Manual Searching with Global Search

> [!TIP]
> To enable global search with filter API, set the 2nd argument to `true`.

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::query();

    return DataTables::eloquent($model)
        ->filter(function ($query) {
            if (request()->has('name')) {
                $query->where('name', 'like', "%" . request('name') . "%");
            }

            if (request()->has('email')) {
                $query->where('email', 'like', "%" . request('email') . "%");
            }
        }, true) // Enable global search
        ->toJson();
});
```

---

## Advanced Filtering

### Filter by Status

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    return DataTables::eloquent(User::query())
        ->filter(function ($query) {
            // Filter by status
            if (request('status')) {
                $query->where('status', request('status'));
            }

            // Filter by date range
            if (request('from_date')) {
                $query->whereDate('created_at', '>=', request('from_date'));
            }
            if (request('to_date')) {
                $query->whereDate('created_at', '<=', request('to_date'));
            }

            // Filter by relationship count
            if (request('has_posts')) {
                $query->has('posts');
            }
        })
        ->toJson();
});
```

### Filter with OR Conditions

```php
return DataTables::eloquent(User::query())
    ->filter(function ($query) {
        if ($search = request('search')) {
            $query->where(function ($q) use ($search) {
                $q->where('name', 'like', "%{$search}%")
                  ->orWhere('email', 'like', "%{$search}%");
            });
        }
    })
    ->toJson();
```

---

## See Also

- [Filter Column](/docs/{{package}}/{{version}}/filter-column) - Custom column filtering
- [Smart Search](/docs/{{package}}/{{version}}/smart-search) - Wildcard search
- [Regex Search](/docs/{{package}}/{{version}}/regex) - Regular expression search
