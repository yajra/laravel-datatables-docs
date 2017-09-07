# Eloquent Model With Trashed

When our `Model` uses `SoftDeletes` trait of Laravel, we need to implicitly tell `DataTables` to include trashed records in the results.
To achieve this, we can use `withTrashed` api.

```php
use DataTables;

Route::get('user-data', function() {
	$model = App\User::query()->withTrashed();

	return DataTables::eloquent($model)
				->withTrashed()
				->make(true);
});
```
