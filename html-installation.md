# HTML Builder Plugin

The HTML Builder is a Laravel package that provides a fluent interface for generating DataTables HTML markup and JavaScript configuration.

## Installation

```bash
composer require yajra/laravel-datatables-html:"^13.0"
```

After installation, publish the configuration:

```bash
php artisan vendor:publish --tag=datatables-html
```

## Basic Usage

```php
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
    ]);

    return view('users.index', compact('html'));
});
```

In your Blade template:

```blade
@extends('app')

@section('contents')
    {{ $html->table() }}
@endsection

@push('scripts')
    {{ $html->scripts() }}
@endpush
```

## See Also

- [Html Builder](/docs/{{package}}/{{version}}/html-builder)
