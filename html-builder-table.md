# Html Builder Table

Table api accepts two parameters: `$builder->table(array $attributes, $footer = false)`
- `$attributes` represents an array that will be converted as your `<table></table>` attributes.
- `$footer` will include/remove `<tfoot></tfoot>` on your `table` markup.

<a name="example"></a>
## Table Example with Footer

```php
use DataTables;
use Yajra\DataTables\Html\Builder;

Route::get('users', function(Builder $builder) {
	if (request()->ajax()) {
        return DataTables::of(User::query())->make(true);
    }

	$html = $builder->columns([
	        	['data' => 'id', 'footer' => 'Id'],
		        ['data' => 'name', 'footer' => 'Name'],
		        ['data' => 'email', 'footer' => 'Email'],
		        ['data' => 'created_at', 'footer' => 'Created At'],
		        ['data' => 'updated_at', 'footer' => 'Updated At'),
	        ]);

	return view('users.index', compact('html'));
});
```

On your `resources/views/users/index.blade.php`.

```php
@extends('app')

@section('contents')
    {!! $html->table(['class' => 'table table-bordered'], true) !!}
@endsection

@push('scripts')
    {!! $html->scripts() !!}
@endpush
```
