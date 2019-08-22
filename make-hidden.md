# Hide attributes from Json

When you are working with Eloquent Object, you can apply the `makeHidden()` ([Laravel documentation](https://laravel.com/docs/master/eloquent-serialization#hiding-attributes-from-json)) function before converting your object toArray().

It can prevent overriding attributes to be compute and increase the performance of converting the object into an array.

```php
use DataTables;

Route::get('user-data', function() {
	$model = App\User::with('posts.comment')->query();

	return DataTables::eloquent($model)
				->makeHidden('posts')
				->toJson();
});
```
