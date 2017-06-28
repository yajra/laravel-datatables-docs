# Buttons Plugin Installation

Run the following command in your project to get the latest version of the plugin:

`composer require yajra/laravel-datatables-buttons:^2.0`

Open the file ```config/app.php``` and then add following service provider.

```php
'providers' => [
    // ...
    Yajra\Datatables\DatatablesServiceProvider::class,
    Yajra\Datatables\ButtonsServiceProvider::class,
],
```

After completing the step above, use the following command to publish configuration & assets:

```
php artisan vendor:publish --tag=datatables-buttons
```
