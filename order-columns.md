# Order Columns

In some cases, you may want to use a custom order sql for a set of columns. To achieve this, you can use `orderColumns` api.

### Special Variable & Placeholder
- `orderColumns` has a special variable `$1` which will be replaced with the order direction of the column.
- `orderColumns` has a special placecholder `:column` which will be replaced with the column name set in the first parameter.

### Example
In this example, we will order the column name with nulls as last result.

```php
use Datatables;

Route::get('user-data', function() {
	$model = App\User::query();

	return Datatables::eloquent($model)
				->orderColumns(['name', 'email'], '-:column $1')
				->make(true);
});
```