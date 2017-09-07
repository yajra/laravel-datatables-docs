# Raw Columns

By default, Laravel DataTables protects us from XSS attack by esacping all our outputs.
In cases where you want to render an html content, please use `rawColumns` api.


```php
use DataTables;

Route::get('user-data', function() {
	$model = App\User::query();

	return DataTables::eloquent($model)
				->addColumn('link', '<a href="#">Html Column</a>')
				->addColumn('action', 'path.to.view')
				->rawColumns(['link', 'action'])
				->toJson();
});
```

