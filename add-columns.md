# Add Columns

Add mutated / hidden columns.

<a name="blade"></a>
## Add hidden model columns

```php
use DataTables;

Route::get('user-data', function() {
	$model = App\User::query();

	return DataTables::eloquent($model)
				->addColumns(['foo','bar','buzz'=>"red"])
				->toJson();
});
```

<a name="response"></a>
## Example Response

```json
{
	"draw": 2,
	"recordsTotal": 10,
	"recordsFiltered": 3,
	"data": [{
		"id": 476,
		"name": "Esmeralda Kulas",
		"email": "abbott.cali@heaney.info",
		"created_at": "2016-07-31 23:26:14",
		"updated_at": "2016-07-31 23:26:14",
		"deleted_at": null,
		"superior_id": 0,
		"foo":"value",
		"bar":"value",
		"buzz":"red"
	}, {
		"id": 6,
		"name": "Zachery Muller",
		"email": "abdullah.koelpin@yahoo.com",
		"created_at": "2016-07-31 23:25:43",
		"updated_at": "2016-07-31 23:25:43",
		"deleted_at": null,
		"superior_id": 1,
		"foo":"value",
        "bar":"value",
        "buzz":"red"
	}]
}
```