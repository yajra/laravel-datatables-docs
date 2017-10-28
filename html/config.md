# Html Builder Config

Default table attributes are now configurable. 
To begin, you need to publish the config by running `php artisan vendor:publish --tag=datatables-html`

Published config is located at `config/datatables-html.php`. 
You can then update the default table attributes that you prefer for every table rendered using the builder class.

```php
return [
	'table' => [
		'class' => 'table',
		'id' => 'dataTableId'
	]
];
```