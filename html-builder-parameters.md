# Html Builder Parameters

Parameters are basically the options you pass when declaring your `DataTable` js script.

See the [`datatables.net`](https://datatables.net) official documentation for the list of all possible [`options`](https://datatables.net/reference/option/).


## Example
```php
$builder->parameters([
	'paging' => true,
	'searching' => true,
	'info' => false,
	'searchDelay' => 350,
	'language' => [
		'url' => url('js/dataTables/language.json')
	],
]);
```