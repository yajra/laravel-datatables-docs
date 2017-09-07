# Add Column

You can add a custom column on your response by using `addColumn` api.

<a name="blade"></a>
## Add Column with Blade Syntax

```php
use Datatables;

Route::get('user-data', function() {
	$model = App\User::query();

	return Datatables::eloquent($model)
				->addColumn('intro', 'Hi {{$name}}!')
				->make(true);
});
```

<a name="closure"></a>
## Add Column with Closure

```php
use Datatables;

Route::get('user-data', function() {
	$model = App\User::query();

	return Datatables::eloquent($model)
				->addColumn('intro', function(User $user) {
					return 'Hi ' . $user->name . '!';
				})
				->make(true);
});
```

<a name="view"></a>
## Add Column with View

> {tip} You can use view to render your added column by passing the view path as the second argument on `addColumn` api.

```php
use Datatables;

Route::get('user-data', function() {
	$model = App\User::query();

	return Datatables::eloquent($model)
				->addColumn('intro', 'users.datatables.intro')
				->make(true);
});
```

Then create your view on `resources/views/users/datatables/intro.blade.php`.
```php
Hi {{ $name }}!
```

<a name="order"></a>
## Add Column with specific order

> {tip} Just pass the column order as the third argument of `addColumn` api.

```php
use Datatables;

Route::get('user-data', function() {
	$model = App\User::query();

	return Datatables::eloquent($model)
				->addColumn('intro', 'Hi {{$name}}!', 2)
				->make(true);
});
```
