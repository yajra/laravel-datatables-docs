# HTML Builder Column

The `Column` class provides a fluent interface for defining table columns with various attributes and options.

## Creating Columns

### Using Column::make()

```php
use Yajra\DataTables\Html\Column;

Column::make('id')
    ->title('ID')
    ->data('id')
    ->name('id');
```

### Using Column::computed()

```php
Column::computed('action', 'Action')
    ->orderable(false)
    ->searchable(false)
    ->render('function(data, type, row) { ... }');
```

### Using Column::checkbox()

```php
Column::checkbox()
    ->title('<input type="checkbox" id="dataTablesCheckbox"/>');
```

### Using Column::formatted()

```php
Column::formatted('name')
    ->render('full.name_formatted');
```

## Column Attributes

| Attribute | Type | Default | Description |
|-----------|------|---------|-------------|
| `name` | string | `data` value | Column name for search/ordering |
| `data` | string | - | Key from JSON response |
| `title` | string | `data` in title case | Column header text |
| `searchable` | bool | `true` | Enable/disable search |
| `orderable` | bool | `true` | Enable/disable ordering |
| `render` | string | `null` | JavaScript render function |
| `footer` | string | `''` | Footer content |
| `exportable` | bool | `true` | Include in exports |
| `printable` | bool | `true` | Include in print view |
| `className` | string | `''` | CSS class name |
| `width` | string | `''` | Column width |
| `visible` | bool | `true` | Show/hide column |

## Fluent Methods

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

## Complete Example

```php
Column::make('user.name')
    ->title('User Name')
    ->data('user.name')
    ->name('users.name')
    ->searchable(true)
    ->orderable(true)
    ->className('text-center')
    ->width('150px')
    ->exportable(true)
    ->printable(true)
    ->footer('Total Users')
    ->render('function(data, type, row) {
        return "<a href=\"/users/" + row.id + "\">" + data + "</a>";
    }');
```

## See Also

- [Html Builder](/docs/{{package}}/{{version}}/html-builder)
