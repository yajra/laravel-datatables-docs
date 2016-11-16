# Edit Column

You can edit a column on your response by using `editColumn` api.

<a name="blade"></a>
## Edit Column with Blade Syntax

```php
use Datatables;

Route::get('user-data', function() {
	$model = App\User::query();

	return Datatables::eloquent($model)
				->editColumn('name', 'Hi {{$name}}!')
				->make(true);
});
```

<a name="closure"></a>
## Edit Column with Closure

```php
use Datatables;

Route::get('user-data', function() {
	$model = App\User::query();

	return Datatables::eloquent($model)
				->editColumn('name', function(User $user) {
					return 'Hi ' . $user->name . '!';
				})
				->make(true);
});
```

<a name="view"></a>
## Edit Column with View

> {tip} You can use view to render your added column by passing the view path as the second argument on `editColumn` api.

```php
use Datatables;

Route::get('user-data', function() {
	$model = App\User::query();

	return Datatables::eloquent($model)
				->editColumn('name', 'users.datatables.into')
				->make(true);
});
```

Then create your view on `resources/views/users/datatables/name.blade.php`.
```php
Hi {{ $name }}!
```
