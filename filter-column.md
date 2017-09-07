# Filter Column

In some cases, we need to implement a custom search for a specific column.
To achieve this, you can use `filterColumn` api.

```php
use DataTables;
use DB;

Route::get('user-data', function() {
	$model = App\User::select([
			'id',
            DB::raw("CONCAT(users.first_name,'-',users.last_name) as fullname"),
            'email',
            'created_at',
            'updated_at',
        ]);

	return DataTables::eloquent($model)
				->filterColumn('fullname', function($query, $keyword) {
					$sql = "CONCAT(users.first_name,'-',users.last_name)  like ?";
	                $query->whereRaw($sql, ["%{$keyword}%"]);
	            })
				->make(true);
});
```
