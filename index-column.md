---
title: Index Column
description: Add row index tracking to DataTables response
---

# Index Column

In some cases, you need to track the index of the records on your response. To achieve this, you can add an index column on your response by using `addIndexColumn` API.

---

<a name="basic"></a>
## Basic Usage

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::query();

    return DataTables::eloquent($model)
        ->addIndexColumn()
        ->toJson();
});
```

---

<a name="how-it-works"></a>
## How It Works

Using `addIndexColumn` will add another column on your response with a column name that is set in the [`index_column`](/docs/{{package}}/{{version}}/general-settings#index-column) configuration.

The default index column name is `DT_RowIndex`.

---

<a name="customizing-row-id"></a>
## Customizing Row ID

If you want to customize the index used by the `DT_RowIndex`, you can use `setRowId('COLUMN')` to change the index number:

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::query();

    return DataTables::eloquent($model)
        ->setRowId('id')
        ->toJson();
});
```

> [!WARNING]
> Be careful with this option. An index should be unique and an integer to be useful for DataTables.

---

<a name="example-response"></a>
## Example Response

```json
{
    "draw": 1,
    "recordsTotal": 10,
    "recordsFiltered": 3,
    "data": [
        {
            "DT_RowIndex": 1,
            "id": 1,
            "name": "John Doe",
            "email": "john@example.com"
        },
        {
            "DT_RowIndex": 2,
            "id": 2,
            "name": "Jane Smith",
            "email": "jane@example.com"
        }
    ]
}
```

---

<a name="see-also"></a>
## See Also

- [Row Options](/docs/{{package}}/{{version}}/row-options) - More row configuration options
- [General Settings](/docs/{{package}}/{{version}}/general-settings) - Configuration options
