# Buttons Plugin

A Laravel DataTables plugin for handling server-side exporting of table as csv, excel, pdf, etc.

<a name="installation"></a>
## Installation

Run the following command in your project to get the latest version of the plugin:

```bash
composer require yajra/laravel-datatables-buttons:"^12.0"
```

<a name="configuratio"></a>
## Configuration

> This step is optional if you are using Laravel 5.5+

Open the file ```config/app.php``` and then add following service provider.

```php
'providers' => [
    // ...
    Yajra\DataTables\DataTablesServiceProvider::class,
    Yajra\DataTables\ButtonsServiceProvider::class,
],
```

After completing the step above, use the following command to publish configuration & assets:

```bash
php artisan vendor:publish --tag=datatables-buttons
```
