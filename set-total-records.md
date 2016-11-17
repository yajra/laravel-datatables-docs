# Set Total Records

In some cases, we need to manually set the total records of our `Datatables` and skip its internal counting functionality.
To achieve this, we can use `setTotalRecords($count)` api.

```php
use Datatables;

Route::get('user-data', function() {
	$model = App\User::query();

	return Datatables::eloquent($model)
				->setTotalRecords(100)
				->make(true);
});
```