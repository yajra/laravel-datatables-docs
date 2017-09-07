# Skip Paging

To skip paging of `DataTables`, we can use `skipPaging` api or just set `paging: false` on our javascript.

## Using PHP
```php
use DataTables;

Route::get('user-data', function() {
	$model = App\User::withTrashed()->query();

	return DataTables::eloquent($model)
				->skipPaging()
				->toJson();
});
```

## Using Javascript
```
<script>
$(document).ready(function() {
	$('#example').DataTable({
		serverSide: true,
		processing: true,
		ajax: '',
		paging: false
	});
});
</script>
```
