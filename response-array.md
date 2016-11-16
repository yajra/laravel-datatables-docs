# Array Response

Array response is the default response of Datatables.

```php
use Datatables;

Route::get('user-data', function() {
	$model = App\User::query();

	return Datatables::eloquent($model)
				->addColumn('intro', 'Hi {{$name}}!')
				->make();
});
```