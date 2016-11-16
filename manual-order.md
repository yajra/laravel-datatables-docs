# Manual Order

You may optionally disable the default ordering function of Datatables and write you own using `order` api.


```php
use Datatables;

Route::get('user-data', function() {
	$model = App\User::query();

	return Datatables::eloquent($model)
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