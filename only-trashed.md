# Eloquent Model With Only Trashed

When our `Model` uses `SoftDeletes` trait of Laravel, we need to implicitly tell `Datatables` to include only trashed records in the results.
To achieve this, we can use `onlyTrashed` api.

```php
use Datatables;

Route::get('user-data', function() {
	$model = App\User::onlyTrashed()->query();

	return Datatables::eloquent($model)
				->onlyTrashed()
				->make(true);
});
```