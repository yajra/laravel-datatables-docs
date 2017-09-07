# White listing columns

Sorting and searching will only work on columns explicitly defined as whitelist.

```php
use DataTables;

Route::get('user-data', function() {
	$model = App\User::query();

	return DataTables::eloquent($model)
				->whitelist(['name', 'email'])
				->toJson();
});
```
