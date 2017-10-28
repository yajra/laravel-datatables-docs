# Html Builder Column

Builder Column represents the column to be rendered by your dataTables.
You can use `addColumn` api to add a single column and `columns` api to add multiple columns.

<a name="attributes"></a>
## Column Attributes

A DataTable `Column` accepts the following attributes:

```php
$column = [
	'name' => 'id',
	'data' => 'id',
	'title' => 'Id',
	'searchable' => true,
	'orderable' => true,
	'render' => 'function(){}',
	'footer' => 'Id',
	'exportable' => true,
	'printable' => true,
];
```

You also need to look at [`datatables.net`](https://datatables.net/reference/option/columns) official columns documentation for further reference.

### Name (Optional)
Name attribute represents the `column` name from your data source.
DataTables will use this attribute when performing search and ordering functions.

> {tip} If not set, `name` attribute will automatically be set to same value as `data` attribute.

### Data
Data attribute will be used when rendering the response to your table. This is the `key` from the `json` response `data` array.

### Title (Optional)
Title attribute is used as your `table` column heading `<th>{{$title}}</th>`.

> {tip} If not set, `data` attribute value will be use as `title` with title case format.

### Searchable (Optional)
Searchable attribute will toggle the `searching` ability for the defined column. Default value is `true`.

### Orderable (Optional)
Orderable attribute will toggle the `ordering` ability for the defined column. Default value is `true`.

### Render (Optional)
Render attribute is a `js` script string that you can use to modify the way the column is being rendered via `javascript`.

### Footer (Optional)
Footer attribute will be as your `tables` column's `footer` content `<tfoot></tfoot>`.

> {tip} To display the footer using html builder, pass `true` as 2nd argument on `$builder->table([], true)` api.

### Exportable (Optional)
Exportable attribute will flag the column to be included when `exporting` the table. Default value is `true`.

> Exportable attribute should be used with the [Buttons Plugin](/docs/{{package}}/{{version}}/buttons-installation)

### Printable (Optional)
Printable attribute will flag the column to be included when `printing` the table. Default value is `true`.

> Exportable attribute should be used with the [Buttons Plugin](/docs/{{package}}/{{version}}/buttons-installation)
