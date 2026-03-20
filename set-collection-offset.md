---
title: Set Collection Offset
description: Set the starting offset for pre-sliced collections in DataTables
---

# Set Collection Offset

The `setOffset()` API defines the offset of the first item in the collection relative to the full dataset. This is useful when working with pre-sliced collections.

---

<a name="basic"></a>
## Basic Usage

```php
use Yajra\DataTables\Facades\DataTables;

Route::get('user-data', function() {
    $collection = collect([
        ['id' => 11, 'name' => 'John'],
        ['id' => 12, 'name' => 'Jane'],
        ['id' => 13, 'name' => 'James'],
    ]);

    return DataTables::collection($collection)
        ->setOffset(10)  // First item is at position 10 in the full dataset
        ->toJson();
});
```

---

<a name="use-case"></a>
## Use Case: Pre-Sliced Data

When you have a collection that's already been sliced from a larger dataset:

```php
use Yajra\DataTables\Facades\DataTables;

Route::get('page-data', function() {
    // Get page 2 of a pre-sliced collection (10 items per page)
    $page = request()->input('page', 1);
    $perPage = 10;
    
    // Get the full dataset and slice for current page
    $fullCollection = cache()->remember('all-users', 3600, function () {
        return User::all()->toArray();
    });
    
    $sliced = collect($fullCollection)
        ->slice(($page - 1) * $perPage, $perPage);

    return DataTables::collection($sliced)
        ->setOffset(($page - 1) * $perPage)
        ->toJson();
});
```

---

<a name="index-column"></a>
## With Index Column

The offset affects the row index when using `addIndexColumn()`:

```php
use Yajra\DataTables\Facades\DataTables;

Route::get('user-data', function() {
    $collection = collect([
        ['id' => 11, 'name' => 'John'],
        ['id' => 12, 'name' => 'Jane'],
    ]);

    return DataTables::collection($collection)
        ->setOffset(10)
        ->addIndexColumn()
        ->toJson();
});

// Result: DT_RowIndex will be 11, 12 instead of 1, 2
```

---

## See Also

- [Collection Data Source](/docs/{{package}}/{{version}}/engine-collection) - Using collections with DataTables
- [Index Column](/docs/{{package}}/{{version}}/index-column) - Adding row index
