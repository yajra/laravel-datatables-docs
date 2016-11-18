# Order by NULLS LAST

This api will set Datatables to perform ordering with `NULLS LAST` option.

```php
use Datatables;

Route::get('user-data', function() {
	$model = App\User::query();

	return Datatables::eloquent($model)
			->orderByNullsLast()
			->make(true);
});
```

> You must update [nulls_last_sql](/docs/{{package}}/{{version}}/general-settings#nulls-last-sql) config and use a pattern that matches your DB Driver.
