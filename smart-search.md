# Runtime Smart Search

You can optionally enable/disable Datatables smart search feature by using `smart` api by passing `true` or `false` as the argument.
Default argument value is `true`.

```php
use Datatables;

Route::get('user-data', function() {
	$model = App\User::query();

	return Datatables::eloquent($model)
					->smart(false)
					->make(true);
});
```