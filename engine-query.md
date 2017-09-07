# Query Builder Data Source

You may use Laravel's Query Builder as data source for your dataTables.
You can look at `Yajra\DataTables\Enginges\QueryBuilderEngine` class which handles the conversion of your Query Builder into a readbale DataTable API response.

<a name="factory"></a>
## Query Builder via Factory

```php
use DB;
use DataTables;

Route::get('user-data', function() {
	$query = DB::table('users');

	return DataTables::of($query)->make(true);
});
```

<a name="facade"></a>
## Query Builder via Facade

```php
use DB;
use DataTables;

Route::get('user-data', function() {
	$query = DB::table('users');

	return DataTables::queryBuilder($query)->make(true);
});
```

<a name="dependency-injection"></a>
## Query Builder via Dependency Injection

```php
use DB;
use Yajra\DataTables\DataTables;

Route::get('user-data', function(DataTables $datatables) {
	$query = DB::table('users');

	return $datatables->queryBuilder($query)->make(true);
});
```
<a name="ioc"></a>
## Query Builder via IoC

```php
use DB;

Route::get('user-data', function() {
	$query = DB::table('users');

	return app('datatables')->queryBuilder($query)->make(true);
});
```
