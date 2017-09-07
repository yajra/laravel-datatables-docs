# Eloquent Model With Only Trashed

When our `Model` uses `SoftDeletes` trait of Laravel, we need to implicitly tell `DataTables` to include only trashed records in the results.
To achieve this, we can use `onlyTrashed` api.

```php
use DataTables;

Route::get('user-data', function() {
	$model = App\User::onlyTrashed()->query();

	return DataTables::eloquent($model)
				->onlyTrashed()
				->toJson();
});
```
