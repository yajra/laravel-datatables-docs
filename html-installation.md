# HTML Builder Plugin

## Installation

Run the following command in your project to get the latest version of the plugin:

`composer require yajra/laravel-DataTables-html:^2.0`

Open the file ```config/app.php``` and then add following service provider.

```php
'providers' => [
    // ...
    Yajra\DataTables\DataTablesServiceProvider::class,
    Yajra\DataTables\HtmlServiceProvider::class,
],
```

After completing the step above, use the following command to publish configuration & assets:

```
php artisan vendor:publish --tag=DataTables-html
```
