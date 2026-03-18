# HTML Builder Table

The `table()` method generates the HTML table markup for your DataTable.

## Basic Usage

### Simple Table

```php
echo $html->table();
```

### Table with Custom Attributes

```php
echo $html->table([
    'class' => 'table table-striped table-bordered',
    'id' => 'users-table',
]);
```

### Table with Footer

```php
echo $html->table(['class' => 'table'], true);
```

### Table with Search Headers

```php
echo $html->table([], false, true);
```

## Method Signature

```php
public function table(array $attributes = [], bool $drawFooter = false, bool $drawSearch = false): HtmlString
```

### Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `$attributes` | array | `[]` | HTML table attributes |
| `$drawFooter` | bool | `false` | Whether to draw `<tfoot>` |
| `$drawSearch` | bool | `false` | Whether to draw search filter row |

## Complete Example

```php
Route::get('users', function(Builder $builder) {
    if (request()->ajax()) {
        return DataTables::of(User::query())->toJson();
    }

    $html = $builder->columns([
        Column::make('id'),
        Column::make('name'),
        Column::make('email'),
    ]);

    return view('users.index', compact('html'));
});
```

In your Blade template:

```blade
@extends('app')

@section('contents')
    {{ $html->table(['class' => 'table table-bordered']) }}
@endsection

@push('scripts')
    {{ $html->scripts() }}
@endpush
```

## See Also

- [Html Builder](/docs/{{package}}/{{version}}/html-builder)
