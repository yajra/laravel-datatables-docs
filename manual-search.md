# Manual Search

You can optionally write your own manual filtering functions and thus disabling global search of the package.
To achieve this, you can use the `filter` api.

<a name="without-global-search"></a>
## Manual Searching without Global Search

```php
use DataTables;

Route::get('user-data', function() {
	$model = App\User::query();

	return DataTables::eloquent($model)
				->filter(function ($query) {
		            if (request()->has('name')) {
		                $query->where('name', 'like', "%{request('name')}%");
		            }

		            if (request()->has('email')) {
		                $query->where('email', 'like', "%{request('email')}%");
		            }
		        })
				->toJson();
});
```

<a name="with-global-search"></a>
## Manual Searching with Global Search

> {tip} To enable global search with filter api, just set the 2nd argument to `true`.

```php
use DataTables;

Route::get('user-data', function() {
	$model = App\User::query();

	return DataTables::eloquent($model)
				->filter(function ($query) {
		            if (request()->has('name')) {
		                $query->where('name', 'like', "%{request('name')}%");
		            }

		            if (request()->has('email')) {
		                $query->where('email', 'like', "%{request('email')}%");
		            }
		        }, true)
				->toJson();
});
```
