# Skip Paging

To skip paging of `Datatables`, we can use `skipPaging` api or just set `paging: false` on our javascript.

## Using PHP
```php
use Datatables;

Route::get('user-data', function() {
	$model = App\User::withTrashed()->query();

	return Datatables::eloquent($model)
				->skipPaging()
				->make(true);
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