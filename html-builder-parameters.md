# Html Builder Parameters

Parameters are basically the options you pass when declaring your `DataTable` js script.

See the [`DataTables.net`](https://DataTables.net) official documentation for the list of all possible [`options`](https://DataTables.net/reference/option/).


## Example
```php
$builder->parameters([
	'paging' => true,
	'searching' => true,
	'info' => false,
	'searchDelay' => 350,
	'language' => [
		'url' => url('js/DataTables/language.json')
	],
]);
```
