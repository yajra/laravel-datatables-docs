# Simple Pagination

Simple pagination aims to improve dataTables response time by skipping the total records count query and settings its value equals to the filtered total records.


## Example


```php
use DataTables;

Route::get('user-data', function() {
	$model = App\User::query();

	return DataTables::eloquent($model)
				->simplePagination()
				->toJson();
});
```
