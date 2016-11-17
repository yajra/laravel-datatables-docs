# Html Builder Action Column

You can use `addAction` api for a quick adding of column with a pre-defined sets attibutes.
Add action column is mostly used for creating a column that contains the `editing` options for your row.

The default attributes of action column are:
```php
[
	'defaultContent' => '',
	'data'           => 'action',
	'name'           => 'action',
	'title'          => 'Action',
	'render'         => null,
	'orderable'      => false,
	'searchable'     => false,
	'exportable'     => false,
	'printable'      => true,
	'footer'         => '',
];
```