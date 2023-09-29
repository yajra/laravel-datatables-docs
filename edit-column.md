# Edit Column

You can edit a column on your response by using `editColumn` api.

<a name="blade"></a>
## Edit Column with Blade Syntax

```php
use DataTables;

Route::get('user-data', function() {
	$model = App\User::query();

	return DataTables::eloquent($model)
				->editColumn('name', 'Hi {{$name}}!')
				->toJson();
});
```

<a name="closure"></a>
## Edit Column with Closure

```php
use DataTables;

Route::get('user-data', function() {
	$model = App\User::query();

	return DataTables::eloquent($model)
				->editColumn('name', function(User $user) {
					return 'Hi ' . $user->name . '!';
				})
				->toJson();
});
```

<a name="view"></a>
## Edit Column with View

> {tip} You can use view to render your added column by passing the view path as the second argument on `editColumn` api.

```php
use DataTables;

Route::get('user-data', function() {
	$model = App\User::query();

	return DataTables::eloquent($model)
				->editColumn('name', 'users.datatables.name')
				->toJson();
	
});
```

Then create your view on `resources/views/users/datatables/name.blade.php`.

```php
Hi {{ $name }}!
```

<a name="view-with-data"></a>
## Edit Column with View and Data

> {tip} You can use view to render your added column by passing the view path as the second argument on `editColumn` api.

```php
use DataTables;

Route::get('user-data', function() {
	$model = App\User::query();

	$externalData = 'External';

	return DataTables::eloquent($model)
				->editColumn('name', ['users.datatables.name', [
					'externalData' => $externalData,
				]])
				->toJson();
	
});
```

Then create your view on `resources/views/users/datatables/name.blade.php`.

```php
Hi {{ $name }}!

Here is some external data: {{ $externalData }}.
```


<a name="selected-column"></a>
## Edit only the requested Columns

> {tip} You can skip editing the columns that are not in your requested payload by using `editOnlySelectedColumns` before using `editColumn`

```php
use DataTables;

Route::get('user-data', function() {
	$model = App\User::query();

	return DataTables::eloquent($model)
                ->editColumn('id', function () {
                    return view('users.datatables.id'); // View will always be rendered
                })
                ->editOnlySelectedColumns()
                ->editColumn('name', function () {
                    return 'Hi ' . $user->name . '!'; // View will only be rendered if the column is in the payload
                               only if in the payload
                })
                ->toJson();
});
```
