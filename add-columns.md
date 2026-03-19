---
title: Add Columns
description: Add multiple columns from model attributes to your DataTables response
---

# Add Columns

> [!NOTE]
> This is an **Eloquent-only** helper method for quickly adding multiple columns from model attributes to your DataTables response. Added columns are visible by default and inherit basic DataTables functionality (sorting, searching if enabled).

---

<a name="quick-example"></a>
## Quick Example

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::query();

    return DataTables::eloquent($model)
        ->addColumns(['first_name', 'last_name', 'email'])
        ->toJson();
});
```

---

<a name="usage"></a>
## Usage

The `addColumns()` method accepts an array where:

- **Integer keys**: Column name equals attribute name
- **String keys**: Column name is the key, attribute name is the value

### Example with Mixed Keys

```php
->addColumns([
    'first_name',              // Column: first_name, Attribute: first_name
    'last_name',               // Column: last_name, Attribute: last_name  
    'full_name' => 'name',     // Column: full_name, Attribute: name
])
```

---

<a name="order"></a>
## With Ordering

The second argument sets the **starting order index** for the added columns. This is useful when combining with existing columns or when using with other column methods.

```php
// Adds columns starting at order index 2
// first_name=2, last_name=3, email=4
->addColumns(['first_name', 'last_name', 'email'], 2)
```

> [!TIP]
> If you have existing columns and want to append new columns at the end, you can first check the current column count:
> ```php
> // After your existing columns, get the count and append
> $dataTable->addColumns(['first_name', 'last_name'], $dataTable->columnCount());
> ```

### Interaction with Other Column Methods

`addColumns()` can be combined with other column methods. Columns added via `addColumns()` will be visible by default and support basic sorting if the database column exists.

```php
return DataTables::eloquent($model)
    ->addColumns([
        'first_name',
        'last_name',
        'full_name' => 'name',    // Alias: column name differs from attribute
    ])
    ->addColumn('action', '...')  // Works alongside addColumn()
    ->toJson();
```

<a name="error"></a>
### Error Handling

If a specified attribute doesn't exist on the model, DataTables will include an empty/null value for that column rather than throwing an error.

---

## See Also

- [Add Column](/docs/{{package}}/{{version}}/add-column) - Add a single column with closure or view support
- [Edit Column](/docs/{{package}}/{{version}}/edit-column) - Modify existing columns
