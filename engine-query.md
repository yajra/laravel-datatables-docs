# Query Builder Data Source

You may use Laravel's Query Builder as data source for your dataTables.
You can look at `Yajra\DataTables\QueryDataTable` class which handles the conversion of your Query Builder into a readable DataTable API response.

<a name="factory"></a>
## Query Builder via Factory

```php
use DB;
use DataTables;

Route::get('user-data', function() {
	$query = DB::table('users');

	return DataTables::of($query)->toJson();
});
```

<a name="facade"></a>
## Query Builder via Facade

```php
use DB;
use DataTables;

Route::get('user-data', function() {
	$query = DB::table('users');

	return DataTables::query($query)->toJson();
});
```

<a name="dependency-injection"></a>
## Query Builder via Dependency Injection

```php
use DB;
use Yajra\DataTables\DataTables;

Route::get('user-data', function(DataTables $dataTables) {
	$query = DB::table('users');

	return $dataTables->query($query)->toJson();
});
```
<a name="ioc"></a>
## Query Builder via IoC

```php
use DB;

Route::get('user-data', function() {
	$query = DB::table('users');

	return app('datatables')->query($query)->toJson();
});
```

<a name="instance"></a>
## QueryDataTable new Instance

```php
use Yajra\DataTables\QueryDataTable;

Route::get('user-data', function() {
    $query = DB::table('users');

    return (new QueryDataTable($query))->toJson();
});
