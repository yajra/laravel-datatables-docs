---
title: HTML Builder Column
description: Configure columns in the DataTables HTML Builder
---


# HTML Builder Column

The `Column` class provides a fluent interface for defining table columns with various attributes and options.

---

<a name="creating"></a>
## Creating Columns

### Using Column::make()

```php
use Yajra\DataTables\Html\Column;

Column::make('id')
    ->title('ID')
    ->data('id')
    ->name('id')
```

### Using Column::computed()

Computed columns are generated on the client-side with data from the server:

```php
use Yajra\DataTables\Html\Column;

Column::computed('action', 'Action')
    ->orderable(false)
    ->searchable(false)
```

### Using Column::checkbox()

```php
use Yajra\DataTables\Html\Column;

Column::checkbox()
    ->title('<input type="checkbox" id="dataTablesCheckbox"/>');
```

### Using Column::formatted()

```php
use Yajra\DataTables\Html\Column;

Column::formatted('name')
    ->render('full.name_formatted');
```

---

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

---

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

---

## Complete Example

```php
use Yajra\DataTables\Html\Column;

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

---

## See Also

- [HTML Builder](/docs/{{package}}/{{version}}/html-builder) - Main HTML Builder documentation
- [HTML Builder Parameters](/docs/{{package}}/{{version}}/html-builder-parameters) - All available parameters
- [HTML Builder Callbacks](/docs/{{package}}/{{version}}/html-builder-callbacks) - JavaScript callbacks
