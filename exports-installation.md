# Export Plugin Installation

Github: https://github.com/yajra/laravel-datatables-export

This package is a plugin of Laravel DataTables for handling server-side exporting using Queue, OpenSpout and Livewire.

## Quick Installation

```
composer require yajra/laravel-datatables-export:"^12.0"
```

The package also requires batch job:

```
php artisan queue:batches-table
php artisan migrate
```

## Service Provider (Optional since Laravel 5.5+)

```
Yajra\DataTables\ExportServiceProvider::class
```

## Configuration and Assets (Optional)

```
$ php artisan vendor:publish --tag=datatables-export --force
```
