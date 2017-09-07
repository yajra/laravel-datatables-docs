# Manual Order

You may optionally disable the default ordering function of DataTables and write you own using `order` api.


```php
use DataTables;

Route::get('user-data', function() {
	$model = App\User::query();

	return DataTables::eloquent($model)
				->order(function ($query) {
		            if (request()->has('name')) {
		                $query->orderBy('name', 'asc');
		            }

		            if (request()->has('email')) {
		                $query->orderBy('email', 'desc');
		            }
		        })
				->make(true);
});
```
