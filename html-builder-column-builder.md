# HTML Builder - Column Builder

The Column Builder is a fluent interface for building column definitions. It provides a clean, expressive way to define columns without using raw arrays.

## Basic Usage

### Creating a Column

```php
use Yajra\DataTables\Html\Column;

$column = Column::make('id')
    ->title('ID')
    ->data('id')
    ->name('id');
```

### Adding to Builder

```php
$html = $builder->columns([
    Column::make('id'),
    Column::make('name'),
    Column::make('email'),
]);
```

## Column Creation Methods

### Column::make()

Create a standard column:

```php
Column::make('id')
    ->title('ID')
    ->data('id')
    ->name('id');
```

### Column::computed()

Create a computed column:

```php
Column::computed('action', 'Action')
    ->orderable(false)
    ->searchable(false)
    ->render('function(data, type, row) { ... }');
```

### Column::checkbox()

Create a checkbox column:

```php
Column::checkbox()
    ->title('<input type="checkbox" id="dataTablesCheckbox"/>');
```

### Column::formatted()

Create a formatted column:

```php
Column::formatted('name')
    ->render('full.name_formatted');
```

## Fluent Methods

All fluent methods from [`Column`](/docs/{{package}}/{{version}}/html-builder-column) are available:

| Method | Description |
|--------|-------------|
| `title()` | Set column title |
| `data()` | Set data source |
| `name()` | Set column name |
| `searchable()` | Enable/disable search |
| `orderable()` | Enable/disable ordering |
| `render()` | Set render function |
| `footer()` | Set footer content |
| `exportable()` | Enable/disable export |
| `printable()` | Enable/disable print |
| `className()` | Add CSS class |
| `width()` | Set column width |
| `visible()` | Show/hide column |
| `orderData()` | Set order data |
| `orderDataType()` | Set order data type |
| `orderSequence()` | Set order sequence |
| `cellType()` | Set cell type |
| `type()` | Set column type |
| `createdCell()` | Set cell callback |
| `exportFormat()` | Set export format |
| `exportRender()` | Set export render |

## Complete Examples

### Basic Column

```php
Column::make('id')
    ->title('ID')
    ->data('id')
    ->name('id')
    ->searchable(true)
    ->orderable(true)
    ->className('text-center')
    ->width('50px');
```

### Computed Column

```php
Column::computed('action', 'Action')
    ->orderable(false)
    ->searchable(false)
    ->exportable(false)
    ->printable(true)
    ->render('function(data, type, row) {
        return "<a href=\"/users/" + row.id + "/edit\" class=\"btn btn-sm btn-primary\">Edit</a>";
    }');
```

### Checkbox Column

```php
Column::checkbox()
    ->title('<input type="checkbox" id="selectAll"/>')
    ->className('select-checkbox')
    ->orderable(false)
    ->searchable(false)
    ->exportable(false);
```

### Full Example

```php
use Yajra\DataTables\Html\Builder;
use Yajra\DataTables\Html\Column;

Route::get('users', function(Builder $builder) {
    if (request()->ajax()) {
        return DataTables::of(User::query())->toJson();
    }

    $html = $builder->columns([
        Column::make('id')
            ->title('ID')
            ->data('id')
            ->name('id')
            ->width('50px'),
            
        Column::make('name')
            ->title('Name')
            ->data('name')
            ->name('users.name')
            ->searchable(true)
            ->orderable(true),
            
        Column::make('email')
            ->title('Email')
            ->data('email')
            ->name('users.email')
            ->searchable(true)
            ->orderable(true),
            
        Column::computed('action', 'Action')
            ->orderable(false)
            ->searchable(false)
            ->exportable(false)
            ->printable(true)
            ->render('function(data, type, row) {
                return "<a href=\"/users/" + row.id + "/edit\" class=\"btn btn-sm btn-primary\">Edit</a>";
            }'),
    ]);

    return view('users.index', compact('html'));
});
```

## Comparison: Array vs Column Builder

### Before (Array)

```php
$columns = [
    ['data' => 'id', 'title' => 'ID'],
    ['data' => 'name', 'title' => 'Name'],
];
```

### After (Column Builder)

```php
$columns = [
    Column::make('id')->title('ID'),
    Column::make('name')->title('Name'),
];
```

## See Also

- [Html Builder](/docs/{{package}}/{{version}}/html-builder)
- [Html Builder Column](/docs/{{package}}/{{version}}/html-builder-column)
