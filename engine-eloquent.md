# Eloquent Data Source

You may use Laravel's Eloquent Model as data source for your dataTables.
You can look at `Yajra\DataTables\EloquentDataTable` class which handles the conversion of your Eloquent Model into a readable DataTable API response.

<a name="factory"></a>
## Eloquent via Factory

```php
use DataTables;

Route::get('user-data', function() {
	$model = App\User::query();

	return DataTables::of($model)->toJson();
});
```

<a name="facade"></a>
## Eloquent via Facade

```php
use DataTables;

Route::get('user-data', function() {
	$model = App\User::query();

	return DataTables::eloquent($model)->toJson();
});
```

<a name="dependency-injection"></a>
## Eloquent via Dependency Injection

```php
use Yajra\DataTables\DataTables;

Route::get('user-data', function(DataTables $dataTables) {
	$model = App\User::query();

	return $dataTables->eloquent($model)->toJson();
});
```
<a name="ioc"></a>
## Eloquent via IoC

```php
Route::get('user-data', function() {
    $model = App\User::query();

    return app('datatables')->eloquent($model)->toJson();
});
```

<a name="instance"></a>
## EloquentDataTable new Instance

```php
use Yajra\DataTables\EloquentDataTable;

Route::get('user-data', function() {
	$model = App\User::query();

	return (new EloquentDataTable($model))->toJson();
});
```
