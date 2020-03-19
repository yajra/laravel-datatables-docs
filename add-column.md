# Add Column

You can add a custom column on your response by using `addColumn` api.

> {note} added columns are assumed to be computed columns and not part of the database. Thus, search/sort will be disabled on those columns. If you need them, use the `editColumn` api instead.

<a name="blade"></a>
## Add Column with Blade Syntax

```php
use DataTables;

Route::get('user-data', function() {
	$model = App\User::query();

	return DataTables::eloquent($model)
				->addColumn('intro', 'Hi {{$name}}!')
				->toJson();
});
```

<a name="closure"></a>
## Add Column with Closure

```php
use DataTables;

Route::get('user-data', function() {
	$model = App\User::query();

	return DataTables::eloquent($model)
				->addColumn('intro', function(User $user) {
					return 'Hi ' . $user->name . '!';
				})
				->toJson();
});
```

<a name="view"></a>
## Add Column with View

> {tip} You can use view to render your added column by passing the view path as the second argument on `addColumn` api.

```php
use DataTables;

Route::get('user-data', function() {
	$model = App\User::query();

	return DataTables::eloquent($model)
				->addColumn('intro', 'users.datatables.intro')
				->toJson();
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
use DataTables;

Route::get('user-data', function() {
	$model = App\User::query();

	return DataTables::eloquent($model)
				->addColumn('intro', 'Hi {{$name}}!', 2)
				->toJson();
});
```
