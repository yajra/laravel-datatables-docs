# Object Response

To convert the response of Datatables to an object, just pass `true` on `make` api.

```php
use Datatables;

Route::get('user-data', function() {
	$model = App\User::query();

	return Datatables::eloquent($model)
				->addColumn('intro', 'Hi {{$name}}!')
				->make(true);
});
```
