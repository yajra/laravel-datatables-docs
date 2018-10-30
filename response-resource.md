# Resource Response

DataTables response using laravel model resource.

```php
use DataTables;

$users = App\User::paginate(10);

$resource = App\Http\Resources\UserResource::collection($users);

return DataTables::of($resource)->toJson();
```

<a name="response"></a>
## Example Response

```json
{
	"draw": 10,
	"recordsTotal": 10,
	"recordsFiltered": 10,
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
	}, ...]
}
```
