# Collection Data Source

You may use Laravel's Collection as data source for your dataTables.
You can look at `Yajra\DataTables\Enginges\CollectionEngine` class which handles the conversion of your Collection into a readbale DataTable API response.

<a name="factory"></a>
## Collection via Factory

```php
use DataTables;

Route::get('user-data', function() {
	$collection = collect([
		['id' => 1, 'name' => 'John'],
		['id' => 2, 'name' => 'Jane'],
		['id' => 3, 'name' => 'James'],
	]);

	return DataTables::of($collection)->make(true);
});
```

<a name="facade"></a>
## Collection via Facade

```php
use DataTables;

Route::get('user-data', function() {
	$collection = collect([
		['id' => 1, 'name' => 'John'],
		['id' => 2, 'name' => 'Jane'],
		['id' => 3, 'name' => 'James'],
	]);

	return DataTables::queryBuilder($collection)->make(true);
});
```

<a name="dependency-injection"></a>
## Collection via Dependency Injection

```php
use Yajra\DataTables\DataTables;

Route::get('user-data', function(DataTables $datatables) {
	$collection = collect([
		['id' => 1, 'name' => 'John'],
		['id' => 2, 'name' => 'Jane'],
		['id' => 3, 'name' => 'James'],
	]);

	return $datatables->queryBuilder($collection)->make(true);
});
```
<a name="ioc"></a>
## Collection via IoC

```php

Route::get('user-data', function() {
	$collection = collect([
		['id' => 1, 'name' => 'John'],
		['id' => 2, 'name' => 'Jane'],
		['id' => 3, 'name' => 'James'],
	]);

	return app('datatables')->queryBuilder($collection)->make(true);
});
```
