# HTML Builder

The HTML Builder is a powerful fluent interface for generating DataTables HTML markup and JavaScript configuration. It provides a clean, expressive way to define your table structure, columns, and options without writing raw HTML or complex JavaScript.

## Installation

The HTML Builder is part of the `yajra/laravel-datatables-html` package. Install it via Composer:

```bash
composer require yajra/laravel-datatables-html:"^13.0"
```

After installation, publish the configuration and assets:

```bash
php artisan vendor:publish --tag=datatables-html
```

## Basic Usage

### Via Dependency Injection

You can inject the `Builder` class directly into your controller methods:

```php
use Yajra\DataTables\Html\Builder;
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('users', function(Builder $builder) {
    if (request()->ajax()) {
        return DataTables::of(User::query())->toJson();
    }

    $html = $builder->columns([
        Column::make('id'),
        Column::make('name'),
        Column::make('email'),
        Column::make('created_at'),
    ]);

    return view('users.index', compact('html'));
});
```

### Via IoC Container

```php
Route::get('users', function() {
    $builder = app('datatables.html');
});
```

### From DataTables Instance

```php
use Yajra\DataTables\DataTables;

Route::get('users', function(DataTables $dataTable) {
    $builder = $dataTable->getHtmlBuilder();
});
```

## Complete Example

```php filename=routes/web.php
use Yajra\DataTables\Facades\DataTables;
use Yajra\DataTables\Html\Builder;
use Yajra\DataTables\Html\Column;
use App\Models\User;

Route::get('users', function(Builder $builder) {
    if (request()->ajax()) {
        return DataTables::of(User::query())->toJson();
    }

    $html = $builder->columns([
        Column::make('id'),
        Column::make('name'),
        Column::make('email'),
        Column::make('created_at'),
        Column::make('updated_at'),
    ]);

    return view('users.index', compact('html'));
});
```

```blade filename=resources/views/users/index.blade.php
@extends('app')

@section('contents')
    {{ $html->table() }}
@endsection

@push('scripts')
    {{ $html->scripts() }}
@endpush
```

## Configuration

Default table attributes can be configured by publishing the config file. Edit `config/datatables-html.php` to customize defaults:

```php
return [
    'table' => [
        'class' => 'table table-bordered',
        'id' => 'dataTable',
    ]
];
```

## Column Types

### Standard Columns

Use `Column::make()` to create standard columns:

```php
Column::make('id')
    ->title('ID')
    ->data('id')
    ->name('id')
```

### Computed Columns

For columns that don't exist in the database:

```php
Column::computed('action', 'Action')
    ->orderable(false)
    ->searchable(false)
    ->render('function(data, type, row) { ... }')
```

### Checkbox Columns

Quickly add a checkbox column for bulk operations:

```php
Column::checkbox()
```

Or use the helper method:

```php
$builder->addCheckbox()
```

### Index Columns

Add a row index column:

```php
Column::index()
```

Or use the helper method:

```php
$builder->addIndex()
```

### Action Columns

Add an action column for edit/delete buttons:

```php
Column::action()
```

Or use the helper method:

```php
$builder->addAction()
```

## Column Attributes

Each column accepts the following attributes:

| Attribute | Type | Description |
|-----------|------|-------------|
| `name` | string | Column name from data source (used for search/ordering) |
| `data` | string | Key from JSON response for rendering |
| `title` | string | Column header text |
| `searchable` | bool | Enable/disable search (default: true) |
| `orderable` | bool | Enable/disable ordering (default: true) |
| `render` | string | JavaScript function for custom rendering |
| `footer` | string | Footer content |
| `exportable` | bool | Include in exports (default: true) |
| `printable` | bool | Include in print view (default: true) |
| `className` | string | CSS class for the column |
| `width` | string | Column width |
| `visible` | bool | Show/hide column (default: true) |

## Table Configuration

### Basic Table

```php
$html = $builder->columns([...]);
echo $html->table();
```

### Table with Custom Attributes

```php
echo $html->table(['class' => 'table table-striped', 'id' => 'users-table']);
```

### Table with Footer

```php
echo $html->table(['class' => 'table'], true);
```

### Table with Search Headers

```php
echo $html->table([], false, true);
```

## AJAX Configuration

### Basic AJAX

```php
$html = $builder->ajax(route('users.data'));
```

### AJAX with POST Method

```php
$html = $builder->postAjax(route('users.data'));
```

### AJAX with Custom Data

```php
$html = $builder->ajax([
    'url' => route('users.data'),
    'type' => 'GET',
    'data' => 'function(d) { d.key = "value"; }'
]);
```

### Minified AJAX

Shortens long URLs by removing unnecessary query parameters:

```php
$html = $builder->minifiedAjax(route('users.data'));
```

## Parameters

Configure DataTables options using the `parameters()` method:

```php
$html = $builder->parameters([
    'paging' => true,
    'searching' => true,
    'info' => false,
    'searchDelay' => 350,
    'language' => [
        'url' => url('js/dataTables/language.json')
    ],
]);
```

## Event Callbacks

Use JavaScript callbacks for custom functionality:

```php
$html = $builder->parameters([
    'drawCallback' => 'function() { alert("Table Drawn"); }',
    'createdRow' => 'function(row, data) { ... }',
    'footerCallback' => 'function(tfoot, data) { ... }',
]);
```

Or use the fluent methods:

```php
$html = $builder
    ->drawCallback('function() { ... }')
    ->createdRow('function(row, data) { ... }')
    ->footerCallback('function(tfoot, data) { ... }');
```

## Plugins

### Select Plugin

Enable row selection:

```php
$html = $builder
    ->selectStyleSingle()
    ->selectInfo(true)
    ->selectItemsRow();
```

### Buttons Plugin

Add export buttons:

```php
use Yajra\DataTables\Html\Button;

$html = $builder->buttons(
    Button::make('excel')->text('Export Excel'),
    Button::make('pdf')->text('Export PDF'),
    Button::make('print')->text('Print')
);
```

### SearchPanes Plugin

Enable search panes:

```php
$html = $builder->searchPanes(true);
```

### Responsive Plugin

Enable responsive behavior:

```php
$html = $builder->responsive(true);
```

## Fluent API

The Builder provides many fluent methods for configuration:

```php
$html = $builder
    ->setTableId('users-table')
    ->addTableClass('table table-bordered')
    ->columns([...])
    ->ajax(route('users.data'))
    ->parameters([...]);
```

## Custom Templates

Create custom JavaScript templates:

```php
$html = $builder->setTemplate('custom-template');
```

Or use built-in templates:

```php
// As options (returns JavaScript object)
$html = $builder->asOptions();

// As function (returns function declaration)
$html = $builder->asFunction();
```

## Scripts Generation

Generate DataTables JavaScript:

```php
// With default type
echo $html->scripts();

// With module type (for Vite)
echo $html->scripts([], ['type' => 'module']);

// Raw scripts
echo $html->generateScripts();
```

## Advanced Usage

### Conditional Columns

```php
if (Gate::allows('edit-user')) {
    $builder->addColumn(['data' => 'edit', 'title' => 'Edit']);
}
```

### Dynamic Columns

```php
$columns = [];
foreach ($fields as $field) {
    $columns[] = Column::make($field);
}
$html = $builder->columns($columns);
```

### Custom Column Rendering

```php
Column::make('status')
    ->render('function(data, type, row) {
        return data == "active" 
            ? "<span class=\"badge badge-success\">Active</span>" 
            : "<span class=\"badge badge-danger\">Inactive</span>";
    }')
```

## Best Practices

1. **Use Column Builder**: Prefer `Column::make()` over raw arrays for better readability
2. **Configure Defaults**: Set default table attributes in config
3. **Use AJAX**: Always use AJAX for server-side processing with large datasets
4. **Optimize Columns**: Set `exportable` and `printable` to false for action columns
5. **Custom Rendering**: Use `render()` for complex column display logic
6. **Security**: Always authorize columns using `Gate` checks

## See Also

- [Columns Documentation](/docs/{{package}}/{{version}}/add-columns)
- [Buttons Plugin](/docs/{{package}}/{{version}}/buttons-installation)
- [Editor Integration](/docs/{{package}}/{{version}}/editor-installation)
