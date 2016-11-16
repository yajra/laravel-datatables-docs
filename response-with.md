# Additional Data Response

You can add additional server data on your response by using `with` api.

<a name="key-value"></a>
## Adding response using key and value

```php
use Datatables;

Route::get('user-data', function() {
	$model = App\User::query();

	return Datatables::eloquent($model)
				->with('posts', 100)
				->with('comments', 20)
				->make(true);
});
```

<a name="array"></a>
## Adding response using array

```php
use Datatables;

Route::get('user-data', function() {
	$model = App\User::query();

	return Datatables::eloquent($model)
				->with([
					'posts' => 100,
					'comments' => 20,
				])
				->make(true);
});
```