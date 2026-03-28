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

Column::make('name')
    ->title('Name')
    ->data('name')
    ->name('name')
```

### ID Columns

For ID columns, use `Column::make('id')`:

```php
Column::make('id')
    ->title('ID')
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
| `renderJs()` | Use built-in JS renderer |
| `renderRaw()` | Set raw render value |
| `exportRender()` | Set export render callback |
| `footer()` | Set footer content |
| `exportable()` | Enable/disable export |
| `printable()` | Enable/disable print |
| `className()` | Add CSS class |
| `addClass()` | Append CSS class |
| `width()` | Set column width |
| `visible()` | Show/hide column |
| `hidden()` | Hide column |
| `orderData()` | Set order data |
| `orderDataType()` | Set order data type |
| `orderSequence()` | Set order sequence |
| `cellType()` | Set cell type |
| `type()` | Set column type |
| `createdCell()` | Set cell callback |
| `contentPadding()` | Set content padding |
| `responsivePriority()` | Set responsive priority |
| `titleAttr()` | Set title attribute |
| `exportFormat()` | Set export format |
| `defaultContent()` | Set default content |
| `editField()` | Set edit field |

---

<a name="visibility"></a>
## Visibility

```php
// Hide column
Column::make('secret')->hidden();

// Show/hide column
Column::make('visible_col')->visible(true);
Column::make('hidden_col')->visible(false);

// Set responsive priority (higher = more important)
Column::make('name')->responsivePriority(1);
```

---

<a name="rendering"></a>
## Rendering

### Built-in Renderers

```php
// Use DataTables built-in renderers
Column::make('price')->renderJs('number');

// With parameters
Column::make('date')->renderJs('moment', 'YYYY-MM-DD');
Column::make('datetime')->renderJs('datetime', 'yyyy-MM-dd HH:mm:ss');
```

### Custom Render Functions

```php
Column::make('name')->render('function(data, type, row) {
    return "<strong>" + data + "</strong>";
}');

// Raw renderer (for arrays/objects)
Column::make('details')->renderRaw('data.details');
```

### Export-Only Rendering

```php
Column::make('actions')->exportRender(function($row) {
    return strip_tags($row->actions);
});
```

### Content Padding

```php
Column::make('description')->contentPadding('5px');
```

---

<a name="class"></a>
## Styling

```php
// Set class name
Column::make('name')->className('text-center');

// Add additional class
Column::make('name')->addClass('text-bold');

// Set column width
Column::make('name')->width('150px');

// Set column style
Column::make('name')->style('background-color: #f0f0f0');
```

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
