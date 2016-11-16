# Eloquent Data Source

You may use Laravel's Eloquent Model as data source for your dataTables.
You can look at `Yajra\Datatables\Enginges\EloquentEngine` class which handles the conversion of your Eloquent Model into a readbale DataTable API response.

<a name="factory"></a>
## Eloquent via Factory

```php
use Datatables;
use DB;

Route::get('user-data', function() {
	$model = App\User::query();

	return Datatables::of($model)->make(true);
});
```

<a name="facade"></a>
## Eloquent via Facade

```php
use Datatables;

Route::get('user-data', function() {
	$model = App\User::query();

	return Datatables::eloquent($model)->make(true);
});
```

<a name="dependency-injection"></a>
## Eloquent via Dependency Injection

```php
use Yajra\Datatables\Datatables;

Route::get('user-data', function(Datatables $datatables) {
	$model = App\User::query();

	return $datatables->eloquent($model)->make(true);
});
```
<a name="ioc"></a>
## Eloquent via IoC

```php
Route::get('user-data', function() {
	$model = App\User::query();

	return app('datatables')->eloquent($model)->make(true);
});
```
