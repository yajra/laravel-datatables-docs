# White listing columns

Sorting and searching will only work on columns explicitly defined as whitelist.

```php
use Datatables;

Route::get('user-data', function() {
	$model = App\User::query();

	return Datatables::eloquent($model)
				->whitelist(['name', 'email'])
				->make(true);
});
```