# Export Plugin Installation

Github: https://github.com/yajra/laravel-datatables-export

Run the following command in your project to get the latest version of the plugin:

`composer require yajra/laravel-datatables-export:^0.12`

## Configuration

> This step is optional if you are using Laravel 5.5

Open the file ```config/app.php``` and then add following service provider.

```php
'providers' => [
    // ...
    Yajra\DataTables\DataTablesServiceProvider::class,
    Yajra\DataTables\ExportServiceProvider::class,
],
```

After completing the step above, use the following command to publish configuration & assets:

```
php artisan vendor:publish --tag=datatables-export --force
```
