# Array Response

Array response is the default response of Datatables.

```php
use Datatables;

Route::get('user-data', function() {
	$model = App\User::query();

	return Datatables::eloquent($model)
				->addColumn('intro', 'Hi {{$name}}!')
				->make();
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