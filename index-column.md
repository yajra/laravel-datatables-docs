# Index Column

In some cases, you need to track the index of the records on your response.
To achieve this, you can add an index column on your response by using `addIndexColumn` api.

```php
use DataTables;

Route::get('user-data', function() {
	$model = App\User::query();

	return DataTables::eloquent($model)
				->addIndexColumn()
				->toJson();
});
```

Using `addIndexColumn` will add another column on your response with a column name that is set on [`index_column`](/docs/{{package}}/{{version}}/general-settings#index-column) configuration.
The default index column name is `DT_RowIndex`
