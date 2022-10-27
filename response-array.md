# Array Response

The default response of the package is an array of objects. If you prefer to return an array response, use `make(false)`.

```php
use DataTables;

Route::get('user-data', function() {
	$model = App\User::query();

	return DataTables::eloquent($model)
				->addColumn('intro', 'Hi {{$name}}!')
				->make(false);
});
```

<a name="response"></a>
## Example Response

```json
{
	"draw": 1,
	"recordsTotal": 10,
	"recordsFiltered": 3,
	"data": [
		[414, "Abel Cole-name", "jklein@block.com", "2016-07-31 23:26:10", "2016-07-31 23:26:10"],
		[267, "Addie Satterfield-name", "cassin.eva@pouros.info", "2016-07-31 23:26:00", "2016-07-31 23:26:00"],
		[120, "Adeline Mayert-name", "rice.elian@abshire.com", "2016-07-31 23:25:50", "2016-07-31 23:25:50"]
	]
}
```
