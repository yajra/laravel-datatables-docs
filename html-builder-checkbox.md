# Html Builder Checkbox Column

You can use `addCheckbox` api for a quick adding of column with a pre-defined sets attibutes.
Add checkbox column is mostly used for creating a column that contains a checkbox `input`.

The default attributes of checkbox column are:
```php
[
	'defaultContent' => '<input type="checkbox" ' . $this->html->attributes($attributes) . '/>',
	'title'          => $this->form->checkbox('', '', false, ['id' => 'DataTablesCheckbox']),
	'data'           => 'checkbox',
	'name'           => 'checkbox',
	'orderable'      => false,
	'searchable'     => false,
	'exportable'     => false,
	'printable'      => true,
	'width'          => '10px',
];
```
