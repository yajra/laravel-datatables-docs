# Html Builder

DataTables has a built-in html builder that you can use to automatically generate your table mark-up and javascripts declarations.

<a name="dependency-injection"></a>
## Html Builder via Dependency Injection

You can use the `Builder` class by using Dependency Injection.

```php
use Yajra\DataTables\Html\Builder;

Route::get('users', function(Builder $builder) {
	//
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

```php filename=routes/web.php
use Yajra\DataTables\Facades\DataTables;
use Yajra\DataTables\Html\Builder;
use Yajra\DataTables\Html\Column;
use App\Models\User;

Route::get('users', function(Builder $builder) {
    if (request()->ajax()) {
        return DataTables::of(User::query())->toJson();
    }

    $html = $builder->columns([
        Column::make('id'),
        Column::make('name'),
        Column::make('email'),
        Column::make('created_at'),
        Column::make('updated_at'),
    ]);

    return view('users.index', compact('html'));
});
```


```php filename=resources/views/users/index.blade.php
@extends('app')

@section('contents')
    {{ $html->table() }}
@endsection

@push('scripts')
    {{ $html->scripts() }}
@endpush
```
