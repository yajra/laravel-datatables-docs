# Collection Data Source

You may use Laravel's Collection as data source for your dataTables.
You can look at `Yajra\DataTables\CollectionDataTable` class which handles the conversion of your Collection into a readable DataTable API response.

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

	return DataTables::of($collection)->toJson();
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

	return DataTables::collection($collection)->toJson();
});
```

<a name="dependency-injection"></a>
## Collection via Dependency Injection

```php
use Yajra\DataTables\DataTables;

Route::get('user-data', function(DataTables $dataTables) {
	$collection = collect([
		['id' => 1, 'name' => 'John'],
		['id' => 2, 'name' => 'Jane'],
		['id' => 3, 'name' => 'James'],
	]);

	return $dataTables->collection($collection)->toJson();
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

	return app('datatables')->collection($collection)->toJson();
});
```

<a name="instance"></a>
## CollectionDataTable new Instance

```php
use Yajra\DataTables\CollectionDataTable;

Route::get('user-data', function() {
    $collection = App\User::all();

    return (new CollectionDataTable($collection))->toJson();
});
