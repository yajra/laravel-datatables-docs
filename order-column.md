# Order Column

In some cases, you may want to use a custom order sql for a specific column. To achieve this, you can use `orderColumn` api.

> {tip} Order column has a special variable `$1` which is being replace as the order direction value of the request.

In this example, we will order the column name with nulls as last result.

```php
use DataTables;

Route::get('user-data', function() {
	$model = App\User::query();

	return DataTables::eloquent($model)
				->orderColumn('name', '-name $1')
				->toJson();
});
```

Here is another example of orderColumn using closure.
```php
use DataTables;

Route::get('user-data', function() {
	$model = App\User::query();

	return DataTables::eloquent($model)
				->orderColumn('name', function($query, $order) {
                      $query->orderBy('status', $order);
                 });
});
```