# Query Builder Extension

Datatables instance allows you to proxy a database query call.

```php
use Datatables;

Route::get('user-data', function() {
	$model = App\User::query();

	$dataTable = Datatables::eloquent($model);
	$dataTable->where('company_id', 2);

	return $dataTable->make(true);
});
```