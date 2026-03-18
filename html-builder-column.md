# HTML Builder Column

The `Column` class provides a fluent interface for defining table columns with various attributes and options. It extends the `Fluent` class and provides many helper methods for common column configurations.

## Creating Columns

### Using Column::make()

The most common way to create a column:

```php
use Yajra\DataTables\Html\Column;

Column::make('id')
    ->title('ID')
    ->data('id')
    ->name('id');
```

### Using Column::computed()

For computed columns that don't exist in the database:

```php
Column::computed('action', 'Action')
    ->orderable(false)
    ->searchable(false)
    ->render('function(data, type, row) { ... }');
```

### Using Column::checkbox()

For checkbox columns (commonly used for bulk operations):

```php
Column::checkbox()
    ->title('<input type="checkbox" id="dataTablesCheckbox"/>');
```

### Using Column::formatted()

For formatted columns that use DataTables' render system:

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
| `defaultContent` | string | `''` | Default value when data is null |
| `titleAttr` | string | `''` | Title attribute for header |
| `orderData` | array|int | `null` | Order data source |
| `orderDataType` | string | `null` | Order data type |
| `orderSequence` | array | `null` | Order sequence |
| `cellType` | string | `'th'` | Cell type (`th` or `td`) |
| `type` | string | `null` | Column type |
| `contentPadding` | string | `null` | Content padding |
| `createdCell` | string | `null` | Callback for cell creation |
| `exportFormat` | string\|callable | `null` | Export format |
| `exportRender` | callable | `null` | Export render callback |

## Fluent Methods

### title()

Set the column title:

```php
Column::make('name')->title('User Name');
```

### searchable()

Enable or disable search:

```php
Column::make('name')->searchable(false);
```

### orderable()

Enable or disable ordering:

```php
Column::make('name')->orderable(false);
```

### className()

Add CSS class to the column:

```php
Column::make('status')->className('text-center');
```

### content()

Set default content for null values:

```php
Column::make('description')->content('N/A');
```

### responsivePriority()

Set responsive priority (lower = higher priority):

```php
Column::make('id')->responsivePriority(1);
```

### hidden()

Hide the column:

```php
Column::make('internal_data')->hidden();
```

### visible()

Show or hide the column:

```php
Column::make('private')->visible(false);
```

### addClass()

Add CSS class (appends to existing):

```php
Column::make('name')->addClass('font-weight-bold');
```

### printable()

Enable or disable print visibility:

```php
Column::make('action')->printable(false);
```

### exportable()

Enable or disable export visibility:

```php
Column::make('internal')->exportable(false);
```

### width()

Set column width:

```php
Column::make('id')->width('50px');
```

### data()

Set the data source:

```php
Column::make('name')->data('user.name');
```

### name()

Set the column name:

```php
Column::make('name')->name('users.name');
```

### editField()

Set the edit field (for Editor integration):

```php
Column::make('name')->editField('name');
```

### orderData()

Set order data source:

```php
Column::make('name')->orderData([0, 1]);
```

### orderDataType()

Set order data type:

```php
Column::make('name')->orderDataType('dom-text');
```

### orderSequence()

Set order sequence:

```php
Column::make('name')->orderSequence(['asc', 'desc']);
```

### cellType()

Set cell type:

```php
Column::make('id')->cellType('td');
```

### type()

Set column type:

```php
Column::make('date')->type('date');
```

### contentPadding()

Set content padding:

```php
Column::make('name')->contentPadding('10px');
```

### createdCell()

Set callback for cell creation:

```php
Column::make('status')->createdCell('function(td, cellData, rowData, row, col) { ... }');
```

### renderJs()

Use a built-in DataTables renderer:

```php
Column::make('name')->renderJs('text');
Column::make('price')->renderJs('number', 2, ',', '.');
```

### render()

Set custom render function:

```php
Column::make('status')->render('function(data, type, row) {
    return data == "active" ? "<span class=\"badge badge-success\">Active</span>" : "<span class=\"badge badge-danger\">Inactive</span>";
}');
```

### renderRaw()

Set raw render value:

```php
Column::make('name')->renderRaw('function(data, type, row) { return data; }');
```

### exportFormat()

Set export format:

```php
Column::make('date')->exportFormat('m/d/Y');
```

### exportRender()

Set export render callback:

```php
Column::make('amount')->exportRender(function($data, $row) {
    return '$' . number_format($data, 2);
});
```

### footer()

Set footer content:

```php
Column::make('amount')->footer('Total: $0.00');
```

### titleAttr()

Set title attribute for header:

```php
Column::make('name')->titleAttr('Click to sort by name');
```

### defaultContent()

Set default content:

```php
Column::make('description')->defaultContent('No description');
```

## Column Helper Methods

### titleFormat()

Format a string to title case:

```php
Column::titleFormat('user_name'); // Returns "User Name"
```

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
    }')
    ->createdCell('function(td, cellData, rowData, row, col) {
        $(td).addClass("user-name-cell");
    }');
```

## See Also

- [Html Builder](/docs/{{package}}/{{version}}/html-builder)
- [Add Column](/docs/{{package}}/{{version}}/add-column)
- [Add Columns](/docs/{{package}}/{{version}}/add-columns)
