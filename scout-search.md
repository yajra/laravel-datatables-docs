---
title: Scout Search Integration
description: Use Laravel Scout with DataTables for full-text search
---

# Scout Search Integration

Laravel DataTables supports [Laravel Scout](https://laravel.com/docs/scout) for full-text search capabilities. This integration uses Scout's search engine (Meilisearch or Algolia) for searching while maintaining DataTables' pagination, filtering, and ordering.

---

<a name="requirements"></a>
## Requirements

- Laravel Scout installed and configured
- A Searchable model
- Meilisearch or Algolia as the search driver

---

<a name="basic"></a>
## Basic Usage

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    return DataTables::eloquent(User::query())
        ->enableScoutSearch(User::class)
        ->toJson();
});
```

---

<a name="max-hits"></a>
## Configure Maximum Results

By default, Scout search returns up to 1000 results. Adjust this with the second parameter:

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    return DataTables::eloquent(User::query())
        ->enableScoutSearch(User::class, 500)
        ->toJson();
});
```

---

<a name="filters"></a>
## Dynamic Filters

Use `scoutFilter()` to add dynamic filters to the Scout search:

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    return DataTables::eloquent(User::query())
        ->enableScoutSearch(User::class)
        ->scoutFilter(function ($keyword) {
            // Add Scout filters based on request parameters
            $filters = [];
            
            if (request()->has('role')) {
                $filters[] = "role = '" . request('role') . "'";
            }
            
            if (request()->has('verified')) {
                $filters[] = "verified = " . (request('verified') ? 'true' : 'false');
            }
            
            return implode(' AND ', $filters);
        })
        ->toJson();
});
```

---

<a name="ordering"></a>
## Search Result Ordering

When using Scout search:
- Results are automatically ordered by search relevance
- User ordering is disabled by default to maintain relevance ordering
- The response includes `disableOrdering: true` in the appended data

---

<a name="supported-engines"></a>
## Supported Search Engines

| Engine | Support | Notes |
|--------|---------|-------|
| Meilisearch | ✅ Full | Recommended |
| Algolia | ✅ Full | Requires API configuration |

---

<a name="fallback"></a>
## Automatic Fallback

If Scout search fails (e.g., network error, service unavailable), DataTables automatically falls back to the standard database search.

---

## See Also

- [Laravel Scout Documentation](https://laravel.com/docs/scout)
- [Manual Search](/docs/{{package}}/{{version}}/manual-search) - Custom search logic
- [Smart Search](/docs/{{package}}/{{version}}/smart-search) - Wildcard search
