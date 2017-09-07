# Query Builder Extension

DataTables instance allows you to proxy a database query call.

```php
use DataTables;

Route::get('user-data', function() {
	$model = App\User::query();

	$dataTable = DataTables::eloquent($model);
	$dataTable->where('company_id', 2);

	return $dataTable->make(true);
});
```
