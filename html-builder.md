# Html Builder

DataTables has a built-in html builder that you can use to automatically generate your table mark-up and javascripts declarations.

<a name="dependency-injection"></a>
## Html Builder via Dependency Injection

You can use the `Builder` class by using Dependency Injection.

```php
use Yajra\DataTables\Html\Builder;

Route::get('users', function(Builder $builder) {
});
```

<a name="ioc"></a>
## Html Builder via IoC

```php
Route::get('users', function() {
	$builder = app('datatables.html');
});
```

<a name="datatables-intance"></a>
## Html Builder from DataTables instance

```php
use Yajra\DataTables\DataTables;

Route::get('users', function(DataTables $dataTable) {
	$builder = $dataTable->getHtmlBuilder();
});
```

<a name="example"></a>
## Html Builder Example

```php
use DataTables;
use Yajra\DataTables\Html\Builder;

Route::get('users', function(Builder $builder) {
	if (request()->ajax()) {
        return DataTables::of(User::query())->make(true);
    }

	$html = $builder->columns([
	        	['data' => 'id', 'name' => 'id', 'title' => 'Id'],
		        ['data' => 'name', 'name' => 'name', 'title' => 'Name'],
		        ['data' => 'email', 'name' => 'email', 'title' => 'Email'],
		        ['data' => 'created_at', 'name' => 'created_at', 'title' => 'Created At'],
		        ['data' => 'updated_at', 'name' => 'updated_at', 'title' => 'Updated At']),
	        ]);

	return view('users.index', compact('html'));
});
```

On your `resources/views/users/index.blade.php`.

```php
@extends('app')

@section('contents')
    {!! $html->table() !!}
@endsection

@push('scripts')
    {!! $html->scripts() !!}
@endpush
```
