# Remove Column

You can remove a column on your response by using `removeColumn` api.

```php
use DataTables;

Route::get('user-data', function() {
	$model = App\User::query();

	return DataTables::eloquent($model)
				->removeColumn('password')
				->make(true);
});
```
