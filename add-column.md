# Add Column

You can add a custom column to your response by using `addColumn` on the `DataTable` instance.

<a name="blade"></a>
## Add Column with Blade Syntax

```php
use DataTables;

Route::get('user-data', function() {
	$model = App\User::query();

	return DataTables::eloquent($model)
				->addColumn('intro', 'Hi {{ $name }}!')
				->toJson();
});
```

> {tip} If you are building a DataTable for your `User` model which has the attributes `first_name` and `last_name`, you can directly access these properties in the blade template with `{{ $first_name }}` and `{{ $last_name }}`. You can also use `{{ $model->first_name }}` or `{{ $model->last_name }}` respectively, which is helpful if you need to call a method on the model like `{{ $model->getFullName() }}`.

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

You can also use a blade template to render a column by passing the view path as the second argument to `addColumn`.

```php
use DataTables;

Route::get('user-data', function() {
	$model = App\User::query();

	return DataTables::eloquent($model)
				->addColumn('intro', 'users.datatables.intro')
				->toJson();
});
```

After creating the view in `resources/views/users/datatables/intro.blade.php`, you can then access all the properties of the DataTable row directly in your view:
```php
Hi {{ $name }}!
```

> {tip} Similar to columns defined by an inline blade template, you can use `{{ $model->getFullName() }}` in views if you need access to the full model in order to call a method on it or similar.

<a name="order"></a>
## Add Column with specific order

You can use a specific order for a column by passing the order as third argument to `addColumn`:

```php
use DataTables;

Route::get('user-data', function() {
	$model = App\User::query();

	return DataTables::eloquent($model)
				->addColumn('intro', 'Hi {{$name}}!', 2)
				->toJson();
});
```
