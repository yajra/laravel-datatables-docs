# Installation

- [Installation](#installation)
	- [Requirements](#requirements)
    - [Installing Laravel-DataTables](#installing-laravel-datatables)
    - [Configuration](#configuration)

<a name="installation"></a>
## Installation

<a name="requirements"></a>
### Requirements

- [Laravel 9|10](https://github.com/laravel/framework)
- [jQuery DataTables v1.10.x](http://datatables.net/)

<a name="installing-laravel-datatables"></a>
### Installing Laravel DataTables

Laravel DataTables can be installed with [Composer](http://getcomposer.org/doc/00-intro.md). More details about this package in Composer can be found [here](https://packagist.org/packages/yajra/laravel-datatables-oracle).

Run the following command in your project to get the latest version of the package:

#### Laravel 9

```bash
composer require yajra/laravel-datatables-oracle:"^9.0"
```

#### Laravel 10

```bash
composer require yajra/laravel-datatables-oracle:"^10.0"
```

If you are using most of the DataTables plugins like Buttons & Html, you can alternatively use the all-in-one installer package.

```bash
composer require yajra/laravel-datatables:^10.0
```

<a name="configuration"></a>
### Configuration
> This step is optional if you are using Laravel 5.5+

Open the file ```config/app.php``` and then add following service provider.

```php
'providers' => [
    // ...
    Yajra\DataTables\DataTablesServiceProvider::class,
],
```

After completing the step above, use the following command to publish configuration & assets:

```
php artisan vendor:publish --tag=datatables
```

