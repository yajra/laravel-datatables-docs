# HTML Builder Macro

The `macro()` method allows you to extend the HTML Builder with custom functionality. This is useful for creating reusable column types or custom builder methods.

## Basic Usage

### Defining a Macro

```php
use Yajra\DataTables\Html\Builder;
use Yajra\DataTables\Html\Column;

Builder::macro('addEditColumn', function () {
    $attributes = [
        'title' => 'Edit',
        'data' => 'edit',
        'name' => '',
        'orderable' => false,
        'searchable' => false,
        'render' => 'function(data, type, row) {
            return "<a href=\"/users/" + row.id + "/edit\" class=\"btn btn-sm btn-primary\">Edit</a>";
        }',
    ];

    $this->collection->push(new Column($attributes));

    return $this;
});
```

### Using a Macro

```php
$html = $builder->addEditColumn()->ajax(route('users.data'));
```

## Complete Examples

### Custom Action Column Macro

```php
use Yajra\DataTables\Html\Builder;
use Yajra\DataTables\Html\Column;

Builder::macro('addCustomActions', function ($view = true, $edit = true, $delete = true) {
    $actions = [];
    
    if ($view) {
        $actions[] = '<a href="/users/{id}" class="btn btn-sm btn-info">View</a>';
    }
    
    if ($edit) {
        $actions[] = '<a href="/users/{id}/edit" class="btn btn-sm btn-primary">Edit</a>';
    }
    
    if ($delete) {
        $actions[] = '<button class="btn btn-sm btn-danger" onclick="deleteUser({id})">Delete</button>';
    }

    $render = 'function(data, type, row) {
        return "' . implode(' ', $actions) . '";
    }';

    $attributes = [
        'title' => 'Actions',
        'data' => 'id',
        'name' => 'actions',
        'orderable' => false,
        'searchable' => false,
        'exportable' => false,
        'printable' => true,
        'render' => $render,
    ];

    $this->collection->push(new Column($attributes));

    return $this;
});
```

### Using the Macro

```php
Route::get('users', function(Builder $builder) {
    if (request()->ajax()) {
        return DataTables::of(User::query())->toJson();
    }

    $html = $builder
        ->addCustomActions(true, true, true)
        ->columns([
            Column::make('id'),
            Column::make('name'),
            Column::make('email'),
        ]);

    return view('users.index', compact('html'));
});
```

### Custom Column Macro

```php
Builder::macro('addStatusColumn', function () {
    $attributes = [
        'title' => 'Status',
        'data' => 'status',
        'name' => 'status',
        'render' => 'function(data, type, row) {
            return data == "active" 
                ? "<span class=\"badge badge-success\">Active</span>" 
                : "<span class=\"badge badge-danger\">Inactive</span>";
        }',
    ];

    $this->collection->push(new Column($attributes));

    return $this;
});
```

### Using the Status Column Macro

```php
$html = $builder
    ->addStatusColumn()
    ->columns([
        Column::make('id'),
        Column::make('name'),
    ]);
```

### Conditional Macro

```php
Builder::macro('addAdminColumns', function () {
    if (Gate::allows('view-admin')) {
        $this->addColumn([
            'data' => 'admin_field',
            'title' => 'Admin Field',
        ]);
    }

    return $this;
});
```

## Macro Best Practices

1. **Return $this**: Always return `$this` to allow method chaining
2. **Use Column class**: Use `Column` class for consistency
3. **Document macros**: Add PHPDoc comments for custom macros
4. **Keep it simple**: Macros should do one thing well
5. **Use Gate checks**: Include authorization checks when appropriate

## Complete Example

```php
use Yajra\DataTables\Html\Builder;
use Yajra\DataTables\Html\Column;
use Illuminate\Support\Facades\Gate;

// Define macros
Builder::macro('addEditColumn', function () {
    $attributes = [
        'title' => 'Edit',
        'data' => 'id',
        'name' => 'edit',
        'orderable' => false,
        'searchable' => false,
        'exportable' => false,
        'printable' => true,
        'render' => 'function(data, type, row) {
            return "<a href=\"/users/" + data + "/edit\" class=\"btn btn-sm btn-primary\">Edit</a>";
        }',
    ];

    $this->collection->push(new Column($attributes));

    return $this;
});

Builder::macro('addDeleteColumn', function () {
    $attributes = [
        'title' => 'Delete',
        'data' => 'id',
        'name' => 'delete',
        'orderable' => false,
        'searchable' => false,
        'exportable' => false,
        'printable' => true,
        'render' => 'function(data, type, row) {
            return "<button class=\"btn btn-sm btn-danger\" onclick=\"deleteUser(" + data + ")\">Delete</button>";
        }',
    ];

    $this->collection->push(new Column($attributes));

    return $this;
});

// Use macros
Route::get('users', function(Builder $builder) {
    if (request()->ajax()) {
        return DataTables::of(User::query())->toJson();
    }

    $html = $builder
        ->addEditColumn()
        ->addDeleteColumn()
        ->columns([
            Column::make('id'),
            Column::make('name'),
            Column::make('email'),
        ]);

    return view('users.index', compact('html'));
});
```

## See Also

- [Html Builder](/docs/{{package}}/{{version}}/html-builder)
- [Html Builder Column](/docs/{{package}}/{{version}}/html-builder-column)
