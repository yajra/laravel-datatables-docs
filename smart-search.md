# Runtime Smart Search

You can optionally enable/disable DataTables smart search feature by using `smart` api by passing `true` or `false` as the argument.
Default argument value is `true`.

```php
use DataTables;

Route::get('user-data', function() {
	$model = App\User::query();

	return DataTables::eloquent($model)
					->smart(false)
					->toJson();
});
```
