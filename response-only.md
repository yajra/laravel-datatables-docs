# Only Response

Get only selected columns in response.

```php
use DataTables;

Route::get('user-data', function() {
	$model = App\User::query();

	return DataTables::eloquent($model)
				->only(['id','name'])
				->toJson();
});
```

<a name="response"></a>
## Example Response

```json
{
	"draw": 2,
	"recordsTotal": 10,
	"recordsFiltered": 2,
	"data": [{
		"id": 476,
		"name": "Esmeralda Kulas"
	}, {
		"id": 6,
		"name": "Zachery Muller"
	}]
}
```
