# Remove Column

You can remove a column on your response by using `removeColumn` api.

```php
use Datatables;

Route::get('user-data', function() {
	$model = App\User::query();

	return Datatables::eloquent($model)
				->removeColumn('password')
				->make(true);
});
```
