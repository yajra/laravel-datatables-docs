# Installation

- [Installation](#installation)
	- [Requirements](#requirements)
    - [Installing Laravel-DataTables](#installing-laravel-datatables)
    - [Configuration](#configuration)

<a name="installation"></a>
## Installation

<a name="requirements"></a>
### Requirements

- [Laravel 11](https://github.com/laravel/framework)
- [DataTables v2.x](http://datatables.net/)

<a name="installing-laravel-datatables"></a>
### Installing Laravel DataTables

Laravel DataTables can be installed with [Composer](http://getcomposer.org/doc/00-intro.md). More details about this package in Composer can be found [here](https://packagist.org/packages/yajra/laravel-datatables-oracle).

Run the following command in your project to get the latest version of the package:

```bash
composer require yajra/laravel-datatables-oracle:"^11.0"
```

If you use most of the DataTables plugins like Buttons & HTML, you can use the all-in-one installer package.

```bash
composer require yajra/laravel-datatables:"^11.0"
```

<a name="configuration"></a>
### Configuration
> This step is optional if you are using Laravel 5.5+

Open the file ```config/app.php``` or ```bootstrap/providers.php``` for Laravel 11 then add the following service provider.

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

