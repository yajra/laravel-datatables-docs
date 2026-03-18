# HTML Builder Action Column

The `addAction()` method provides a quick way to add an action column to your DataTable. This is commonly used for edit, delete, and other row-specific actions.

## Basic Usage

### Using addAction()

```php
$html = $builder->addAction();
```

### With Custom Attributes

```php
$html = $builder->addAction([
    'title' => 'Actions',
    'width' => '100px',
]);
```

### With Prepend

```php
// Prepend to beginning
$html = $builder->addAction([], true);
```

## Default Attributes

The action column has the following default attributes:

```php
[
    'defaultContent' => '',
    'data' => 'action',
    'name' => 'action',
    'title' => 'Action',
    'render' => null,
    'orderable' => false,
    'searchable' => false,
    'exportable' => false,
    'printable' => true,
    'footer' => '',
]
```

## Complete Examples

### Basic Action Column

```php
Route::get('users', function(Builder $builder) {
    if (request()->ajax()) {
        return DataTables::of(User::query())->toJson();
    }

    $html = $builder
        ->columns([
            Column::make('id'),
            Column::make('name'),
            Column::make('email'),
        ])
        ->addAction();

    return view('users.index', compact('html'));
});
```

### Action Column with Custom Title

```php
$html = $builder->addAction([
    'title' => 'Manage',
    'width' => '120px',
]);
```

### Action Column with Custom Render

```php
$html = $builder->addAction([
    'render' => 'function(data, type, row) {
        return "<a href=\"/users/" + row.id + "/edit\" class=\"btn btn-sm btn-primary\">Edit</a> " +
               "<button class=\"btn btn-sm btn-danger\" onclick=\"deleteUser(" + row.id + ")\">Delete</button>";
    }',
]);
```

### Action Column with Custom Data

```php
$html = $builder->addAction([
    'data' => 'id',
    'render' => 'function(data, type, row) {
        return "<a href=\"/users/" + data + "\" class=\"btn btn-sm btn-info\">View</a>";
    }',
]);
```

### Action Column with Custom Footer

```php
$html = $builder->addAction([
    'footer' => 'Actions',
]);
```

## Using Column::action()

You can also use the `Column::action()` method:

```php
$html = $builder->columns([
    Column::make('id'),
    Column::make('name'),
    Column::action(),
]);
```

## Complete Example with Multiple Actions

```php
Route::get('users', function(Builder $builder) {
    if (request()->ajax()) {
        return DataTables::of(User::query())->toJson();
    }

    $html = $builder
        ->columns([
            Column::make('id'),
            Column::make('name'),
            Column::make('email'),
        ])
        ->addAction([
            'render' => 'function(data, type, row) {
                var actions = "";
                
                if (canViewUser) {
                    actions += "<a href=\"/users/" + row.id + "\" class=\"btn btn-sm btn-info mr-1\">View</a>";
                }
                
                if (canEditUser) {
                    actions += "<a href=\"/users/" + row.id + "/edit\" class=\"btn btn-sm btn-primary mr-1\">Edit</a>";
                }
                
                if (canDeleteUser) {
                    actions += "<button class=\"btn btn-sm btn-danger\" onclick=\"deleteUser(" + row.id + ")\">Delete</button>";
                }
                
                return actions;
            }',
            'orderable' => false,
            'searchable' => false,
            'exportable' => false,
            'printable' => true,
        ]);

    return view('users.index', compact('html'));
});
```

## See Also

- [Html Builder](/docs/{{package}}/{{version}}/html-builder)
- [Html Builder Column](/docs/{{package}}/{{version}}/html-builder-column)
