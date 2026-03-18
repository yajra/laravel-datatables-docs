# HTML Builder Index Column

The `addIndex()` method provides a quick way to add an index column to your DataTable. This column displays the row number or index of each record.

## Basic Usage

### Using addIndex()

```php
$html = $builder->addIndex();
```

### With Custom Attributes

```php
$html = $builder->addIndex([
    'title' => '#',
    'width' => '50px',
]);
```

## Default Attributes

The index column has the following default attributes:

```php
[
    'defaultContent' => '',
    'data' => 'DT_RowIndex',
    'name' => 'DT_RowIndex',
    'title' => '',
    'render' => null,
    'orderable' => false,
    'searchable' => false,
    'exportable' => false,
    'printable' => true,
    'footer' => '',
]
```

## Complete Examples

### Basic Index Column

```php
Route::get('users', function(Builder $builder) {
    if (request()->ajax()) {
        return DataTables::of(User::query())->toJson();
    }

    $html = $builder
        ->addIndex()
        ->columns([
            Column::make('id'),
            Column::make('name'),
            Column::make('email'),
        ]);

    return view('users.index', compact('html'));
});
```

### Index Column with Custom Title

```php
$html = $builder->addIndex([
    'title' => '#',
]);
```

### Index Column with Custom Width

```php
$html = $builder->addIndex([
    'width' => '40px',
]);
```

### Index Column with Custom Data

```php
$html = $builder->addIndex([
    'data' => 'DT_RowIndex',
]);
```

## Using Column::index()

You can also use the `Column::index()` method:

```php
$html = $builder->columns([
    Column::index(),
    Column::make('id'),
    Column::make('name'),
]);
```

## Integration with DataTables

The index column works with the [`addIndexColumn()`](/docs/{{package}}/{{version}}/index-column) method of DataTables:

```php
Route::get('users', function(Builder $builder) {
    if (request()->ajax()) {
        return DataTables::of(User::query())
            ->addIndexColumn()
            ->toJson();
    }

    $html = $builder
        ->addIndex()
        ->columns([...]);

    return view('users.index', compact('html'));
});
```

## See Also

- [Html Builder](/docs/{{package}}/{{version}}/html-builder)
- [Index Column](/docs/{{package}}/{{version}}/index-column)
