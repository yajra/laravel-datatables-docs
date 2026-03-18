---
title: Upgrade Guide
description: Migrate from previous versions of Laravel DataTables
---

# Upgrade Guide

This guide covers migrating from previous versions of Laravel DataTables.

---

<a name="v13"></a>
## Upgrading to v13.x

> [!NOTE]
> Laravel DataTables v13.x requires Laravel 13.x and PHP 8.3+.

### Core Package

```bash
composer require yajra/laravel-datatables-oracle:"^13.0"
php artisan vendor:publish --tag=datatables --force
```

### With HTML Builder

```bash
composer require yajra/laravel-datatables-html:"^13.0"
php artisan vendor:publish --tag=datatables-html --force
```

### With Buttons

```bash
composer require yajra/laravel-datatables-buttons:"^13.0"
php artisan vendor:publish --tag=datatables-buttons --force
```

### With Fractal

```bash
composer require yajra/laravel-datatables-fractal:"^13.0"
php artisan vendor:publish --tag=datatables-fractal --force
```

### With Export

```bash
composer require yajra/laravel-datatables-export:"^13.0"
php artisan vendor:publish --tag=datatables-export --force
```

### With Editor

```bash
composer require yajra/laravel-datatables-editor:"^13.0"
php artisan vendor:publish --tag=datatables-editor --force
```

---

## v13 Breaking Changes

### Namespace Changes

All packages now use the `Yajra\DataTables` namespace. Previous `Yajra\Datatables` namespace is no longer supported.

```php
// Old (no longer works)
use Yajra\Datatables\Facades\Datatables;

// New
use Yajra\DataTables\Facades\DataTables;
```

### Facade Changes

```php
// Old (no longer works)
Datatables::of($query)->toJson();

// New
DataTables::of($query)->toJson();
```

### Service Class Changes

The `dataTable()` method should return a new instance directly:

```php
// Old
public function dataTable() {
    return $this->datatables->eloquent($this->query());
}

// New
use Yajra\DataTables\EloquentDataTable;

public function dataTable($query): EloquentDataTable
{
    return new EloquentDataTable($query);
}
```

Or with dependency injection:

```php
// Alternative: method injection
use Yajra\DataTables\DataTables;

public function dataTable($query, DataTables $dataTables)
{
    return $dataTables->eloquent($query);
}
```

### XSS Protection

All columns are now escaped by default to protect against XSS attacks. To allow HTML content in columns, use `rawColumns`:

```php
// Old
DataTables::of(User::query())
    ->addColumn('href', '<a href="#">Html Content</a>')
    ->toJson();

// New
DataTables::of(User::query())
    ->addColumn('href', '<a href="#">Html Content</a>')
    ->rawColumns(['href'])
    ->toJson();
```

### Soft Deletes

`withTrashed()` and `onlyTrashed()` are removed. Use Laravel's native soft delete filtering instead:

```php
// Old
DataTables::eloquent($model)->withTrashed();

// New - use scopes or query filters
$model = User::query()->withTrashed(); // Native Laravel
```

---

## v8 to v9 Changes

### Removed Methods

- `DataTableContract` contract removed
- `DataTableScopeContract` renamed to `DataTableScope`
- `DataTableButtonsContract` renamed to `DataTableButtons`

---

<a name="v7-to-v8"></a>
## Upgrading from v7.x to v8.x

### Core Package

```bash
composer require yajra/laravel-datatables-oracle:8.*
php artisan vendor:publish --tag=datatables --force
```

### Buttons Plugin

```bash
composer require yajra/laravel-datatables-buttons:3.*
php artisan vendor:publish --tag=datatables-buttons --force
```

### HTML Plugin

```bash
composer require yajra/laravel-datatables-html:3.*
php artisan vendor:publish --tag=datatables-html --force
```

### Fractal

```bash
composer require yajra/laravel-datatables-fractal:1.*
php artisan vendor:publish --tag=datatables-fractal --force
```

---

<a name="v6-to-v7"></a>
## Upgrading from v6.x to v7.x

```bash
composer require yajra/laravel-datatables-oracle:^7.0
php artisan vendor:publish --tag=datatables --force
```

### Service Approach (Buttons Plugin)

```bash
composer require yajra/laravel-datatables-buttons:^1.0
php artisan vendor:publish --tag=datatables-buttons --force
```

### HTML Builder

```bash
composer require yajra/laravel-datatables-html:^1.0
php artisan vendor:publish --tag=datatables-html --force
```

---

<a name="v5-to-v6"></a>
## Upgrading from v5.x to v6.x

1. Change all occurrences of `yajra\Datatables` to `Yajra\Datatables`
2. Remove `Datatables` facade registration
3. Temporarily comment out `Yajra\Datatables\DatatablesServiceProvider`
4. Update package version on your `composer.json` to `yajra/laravel-datatables-oracle: ~6.0`
5. Uncomment the provider `Yajra\Datatables\DatatablesServiceProvider`

---

## See Also

- [Installation](/docs/{{package}}/{{version}}/installation) - Fresh installation guide
- [Quick Starter](/docs/{{package}}/{{version}}/quick-starter) - Build your first DataTable
