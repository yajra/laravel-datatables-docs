# Set Filtered Records

In some cases, we need to manually set the filtered records count of our `DataTables` and skip its internal counting functionality.
To achieve this, we can use `setFilteredRecords($count)` api.

```php
use DataTables;

Route::get('user-data', function() {
	$model = App\User::query();

	return DataTables::eloquent($model)
				->setFilteredRecords(100)
				->toJson();
});
```
