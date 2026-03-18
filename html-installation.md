# HTML Builder Plugin

The HTML Builder is a Laravel package that provides a fluent interface for generating DataTables HTML markup and JavaScript configuration.

## Installation

### Via Composer

Run the following command in your project to get the latest version of the plugin:

```bash
composer require yajra/laravel-datatables-html:"^13.0"
```

### Package Source

GitHub: https://github.com/yajra/laravel-datatables-html

## Configuration

### Laravel 5.5+

The package uses auto-discovery, so no manual service provider registration is required.

### Laravel < 5.5

Open the file `config/app.php` and add the service provider:

```php
'providers' => [
    // ...
    Yajra\DataTables\DataTablesServiceProvider::class,
    Yajra\DataTables\HtmlServiceProvider::class,
],
```

### Publishing Configuration and Assets

After installation, publish the configuration and assets:

```bash
php artisan vendor:publish --tag=datatables-html
```

This will publish:
- Configuration file to `config/datatables-html.php`
- Views to `resources/views/vendor/datatables`

## Configuration File

After publishing, you can configure default table attributes in `config/datatables-html.php`:

```php
return [
    'table' => [
        'class' => 'table table-bordered',
        'id' => 'dataTable',
    ],
    'script' => 'datatables::script',
];
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

- [Html Builder Documentation](/docs/{{package}}/{{version}}/html-builder)
