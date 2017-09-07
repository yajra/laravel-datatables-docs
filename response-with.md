# Additional Data Response

You can add additional server data on your response by using `with` api.

<a name="key-value"></a>
## Adding response using key and value

```php
use DataTables;

Route::get('user-data', function() {
	$model = App\User::query();

	return DataTables::eloquent($model)
				->with('posts', 100)
				->with('comments', 20)
				->toJson();
});
```

<a name="array"></a>
## Adding response using array

```php
use DataTables;

Route::get('user-data', function() {
	$model = App\User::query();

	return DataTables::eloquent($model)
				->with([
					'posts' => 100,
					'comments' => 20,
				])
				->toJson();
});
```

<a name="response"></a>
## Example Response

```json
{
	"draw": 1,
	"recordsTotal": 10,
	"recordsFiltered": 3,
	"data": [{
		"id": 476,
		"name": "Esmeralda Kulas",
		"email": "abbott.cali@heaney.info",
		"created_at": "2016-07-31 23:26:14",
		"updated_at": "2016-07-31 23:26:14",
		"deleted_at": null,
		"superior_id": 0
	}, {
		"id": 6,
		"name": "Zachery Muller",
		"email": "abdullah.koelpin@yahoo.com",
		"created_at": "2016-07-31 23:25:43",
		"updated_at": "2016-07-31 23:25:43",
		"deleted_at": null,
		"superior_id": 1
	}],
	"posts": 100,
	"comments": 20
}
```
