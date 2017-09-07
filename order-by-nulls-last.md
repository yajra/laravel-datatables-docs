# Order by NULLS LAST

This api will set DataTables to perform ordering with `NULLS LAST` option.

```php
use DataTables;

Route::get('user-data', function() {
	$model = App\User::query();

	return DataTables::eloquent($model)
			->orderByNullsLast()
			->make(true);
});
```

> You must update [nulls_last_sql](/docs/{{package}}/{{version}}/general-settings#nulls-last-sql) config and use a pattern that matches your DB Driver.
