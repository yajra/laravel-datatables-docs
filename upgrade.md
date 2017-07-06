# Upgrade Guide

<a name="v7-to-v8"></a>
## Upgrading from v7.x to v8.x
To upgrade Laravel DataTables from version 7.x to version 8.x:


```bash
composer require yajra/laravel-datatables-oracle:8.*
php artisan vendor:publish --tag=datatables --force
```

If you are using service approach / buttons plugin:
```bash
composer require yajra/laravel-datatables-buttons:3.*
php artisan vendor:publish --tag=datatables-buttons --force
```

If you are using html plugin:
```bash
composer require yajra/laravel-datatables-html:3.*
php artisan vendor:publish --tag=datatables-html --force
```

If you are using fractal:
```bash
composer require yajra/laravel-datatables-fractal:1.*
php artisan vendor:publish --tag=datatables-fractal --force
```

<a name="namespace"></a>
## [v8] Namespace
The package namespace was updated from `Yajra\Datatables` to `Yajra\DataTables`. 
> Use sublime's find and replace all feature to update all affected files.

<a name="facade"></a>
## [v8] Facade
DataTables Facade was renamed to `Yajra\DataTables\Facades\DataTables`. If you want to continue using your old facade, just register the alias on your `config/app.php` file.

```php
'Datatables' => Yajra\DataTables\Facades\DataTables::class
```

<a name="factory"></a>
## [v8] DataTables Factory class
DataTables factory class is now renamed to `DataTables` from `Datatables`. If you are injecting `Yajra\Datatables\Datatables` on your code, you must update it to `Yajra\DataTables\DataTables`.

`DataTables::of()` method is now an alias of new `DataTables::make()` method to match Laravel's factory api structure.

<a name="buttons"></a>
## [v8] DataTables Buttons Changes
- The package namespace was updated from `Yajra\Datatables` to `Yajra\DataTables`. 
> Use sublime's find and replace all feature to update all affected files.
- Constructor dependencies were removed.
- You need to instanstiate the DataTable class within the `dataTable()` method:
	```php
	// FROM
	public function dataTable() {
		return $this->datatables->eloquent($this->query());
	}
	```
	```php
	// TO
	use Yajra\DataTables\EloquentDataTable;
	public function dataTable($query) {
		return new EloquentDataTable($query);
	}
	```

	Or inject the factory using method injection.
	```php
	use Yajra\DataTables\DataTables;
	public function dataTable($query, DataTables $dataTables) {
		return $dataTables->eloquent($query);
	}
	```
- Query method results are automatically injected on `dataTable($query)` api.
	```php
	use Yajra\DataTables\DataTables;
	public function dataTable($query, DataTables $dataTables) {
		return $dataTables->eloquent($query);
	}

	public function query() {
		return Model::query();
	}
	```
- The following methods now supports method injection:

	Action Buttons: `csv(), pdf(), excel(), printPreview()`

	Builder Methods: `ajax(), dataTable(), query()`


<a name="html"></a>
## [v8] DataTables Html Changes
The package namespace was updated from `Yajra\Datatables` to `Yajra\DataTables`. 
> Use sublime's find and replace all feature to update all affected files.

<a name="trashed"></a>
## [v8] DataTables Trashed
DataTables now supports `SoftDeletes` hence, there is no need to use `withTrashed` and `onlyTrashed`.


<a name="removed"></a>
## [v8] Functionalities Removed
- Removed `filterColumn` api magic query method in favor of closure.
- Removed support on older `snake_case` methods.
- Removed silly implementation of proxying query builder calls via magic method. 
- Removed unused methods.
- Removed `withTrashed` and `onlyTrashed` api.

<a name="v6-to-v7"></a>
## Upgrading from v6.x to v7.x
To upgrade Laravel Datatables from version 6.x to version 7.x:

```sh
composer require yajra/laravel-datatables-oracle:^7.0
php artisan vendor:publish --tag=datatables --force
```

### Service Approach
Service class is now extracted to own plugin, `Buttons Plugin`. If you are using the service approach, you need to perform the following:

```sh
composer require yajra/laravel-datatables-buttons:^1.0
```

Register `Yajra\Datatables\ButtonsServiceProvider::class` on `config/app.php` and publish config.


```php
php artisan vendor:publish --tag=datatables-buttons --force
```


### Html Builder
HTML builder is now extracted to own plugin, If you are using Datatables html builder, you need to perform the following:
> HTML Builder plugin is a prerequisite of Buttons plugin. You can optionally skip this part if already installed the Buttons plugin.

```sh
composer require yajra/laravel-datatables-html:^1.0
php artisan vendor:publish --tag=datatables-html --force
```


### XSS Protection
All columns are now escaped by default to protect us from XSS attack. To allow columns to have an html content, use `rawColumns` api.

```php
Datatables::of(User::query())
	->addColumn('href', '<a href="#">Html Content</a>')
	->rawColumns(['href'])
	->make(true);
```

  
<a name="v5-to-v6"></a>
## Upgrading from v5.x to v6.x
- Change all occurrences of `yajra\Datatables` to `Yajra\Datatables`. (Use Sublime's find and replace all for faster update).
- Remove `Datatables` facade registration.
- Temporarily comment out `Yajra\Datatables\DatatablesServiceProvider`.
- Update package version on your composer.json and use `yajra/laravel-datatables-oracle: ~6.0`
- Uncomment the provider `Yajra\Datatables\DatatablesServiceProvider`.
