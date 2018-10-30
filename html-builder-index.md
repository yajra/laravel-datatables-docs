# Html Builder Index Column

You can use `addIndex` api for a quick adding of column with a pre-defined sets attibutes.
Add index column is mostly used for creating a column that contains a the internal index of the current record being displayed.

The default attributes of index column are:
```php
[
	'defaultContent' => '',
	'data'           => 'DT_RowIndex',
	'name'           => 'DT_RowIndex',
	'title'          => '',
	'render'         => null,
	'orderable'      => false,
	'searchable'     => false,
	'exportable'     => false,
	'printable'      => true,
	'footer'         => '',
];
```

The `addIndex` api should be used along with [`addIndexColumn`](/docs/{{package}}/{{version}}/index-column) of `DataTables`.
