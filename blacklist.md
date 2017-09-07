# Black listing columns

Sorting and searching will not work on columns explicitly defined as blacklisted.

```php
use Datatables;

Route::get('user-data', function() {
	$model = App\User::query();

	return Datatables::eloquent($model)
				->blacklist(['password', 'name'])
				->make(true);
});
```