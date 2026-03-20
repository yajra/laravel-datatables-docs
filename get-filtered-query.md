---
title: Get Filtered Query
description: Access the filtered, ordered, and paginated query from DataTables
---

# Get Filtered Query

The `getFilteredQuery()` API returns the query after DataTables processing (filtering, ordering, pagination) has been applied. This is useful when you need to access the processed query for debugging or additional operations.

---

<a name="basic"></a>
## Basic Usage

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::query();

    $dataTable = DataTables::eloquent($model);
    
    // Access the filtered query
    $filteredQuery = $dataTable->getFilteredQuery();
    
    // Log the SQL for debugging
    logger()->info($filteredQuery->toSql());
    
    return $dataTable->toJson();
});
```

---

<a name="use-cases"></a>
## Use Cases

### Debugging

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $dataTable = DataTables::eloquent(User::query());
    
    // Check if any filters were applied
    $query = $dataTable->getFilteredQuery();
    
    if ($query->getBindings()) {
        logger()->info('Query with filters', [
            'sql' => $query->toSql(),
            'bindings' => $query->getBindings(),
        ]);
    }
    
    return $dataTable->toJson();
});
```

### Counting After Filtering

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $dataTable = DataTables::eloquent(User::query());
    
    // Get count of filtered results
    $filteredCount = $dataTable->getFilteredQuery()->count();
    
    return $dataTable
        ->with('filtered_count', $filteredCount)
        ->toJson();
});
```

---

<a name="note"></a>
## Note

> `getFilteredQuery()` calls `prepareQuery()` internally, which means filtering, ordering, and pagination are applied to the query. The query is cloned before processing, so the original query is not modified.

---

## See Also

- [Additional Data Response](/docs/{{package}}/{{version}}/response-with) - Adding extra data to response
- [Query Builder Data Source](/docs/{{package}}/{{version}}/engine-query) - Using Query Builder
