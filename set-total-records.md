# Set Total Records

In some cases, we need to manually set the total records of our `DataTables` and skip its internal counting functionality.
To achieve this, we can use `setTotalRecords($count)` api.

```php
use DataTables;

Route::get('user-data', function() {
	$model = App\User::query();

	return DataTables::eloquent($model)
				->setTotalRecords(100)
				->make(true);
});
```
