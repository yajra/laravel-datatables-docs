# Installation

- [Installation](#installation)
	- [Requirements](#requirements)
    - [Installing Laravel-DataTables](#installing-laravel-datatables-oracle)
    - [Configuration](#configuration)

<a name="installation"></a>
## Installation

<a name="requirements"></a>
### Requirements

- [Laravel 5.4](https://github.com/laravel/framework)
- [jQuery DataTables v1.10.x](http://datatables.net/)

<a name="installing-laravel-datatables-oracle"></a>
### Installing Laravel DataTables

Laravel DataTables can be installed with [Composer](http://getcomposer.org/doc/00-intro.md). More details about this package in Composer can be found [here](https://packagist.org/packages/yajra/laravel-datatables-oracle).

Run the following command in your project to get the latest version of the package:

```
composer require yajra/laravel-datatables-oracle:^8.0
```

<a name="configuration"></a>
### Configuration

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

