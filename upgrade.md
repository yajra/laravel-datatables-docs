---
title: Upgrade Guide
description: Migrate from previous versions of Laravel DataTables
---

# Upgrade Guide

This guide covers migrating from previous versions of Laravel DataTables.

---

<a name="v13"></a>
## Upgrading to v13.x

Laravel DataTables v13.x is a straightforward upgrade with **no breaking changes**. This release adds support for Laravel 13 and PHP 8.3+.

> [!IMPORTANT]
> **Requirements:**
> - Laravel 13.x
> - PHP 8.3+

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

### What's New in v13

- **Laravel 13 Support**: Full compatibility with Laravel 13
- **PHP 8.3 Support**: Compatible with PHP 8.3+
- **Seamless Upgrade**: No breaking changes from v9

---

<a name="v8-to-v9"></a>
## Upgrading from v8.x to v9.x

> [!NOTE]
> This section is kept for historical reference when upgrading from v8.

### Removed Methods

- `DataTableContract` contract removed
- `DataTableScopeContract` renamed to `DataTableScope`
- `DataTableButtonsContract` renamed to `DataTableButtons`

---

<a name="v7-to-v8"></a>
## Upgrading from v7.x to v8.x

> [!NOTE]
> This section is kept for historical reference when upgrading from v7.

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

> [!NOTE]
> This section is kept for historical reference when upgrading from v6.

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

> [!NOTE]
> This section is kept for historical reference when upgrading from v5.

1. Change all occurrences of `yajra\Datatables` to `Yajra\Datatables`
2. Remove `Datatables` facade registration
3. Temporarily comment out `Yajra\Datatables\DatatablesServiceProvider`
4. Update package version on your `composer.json` to `yajra/laravel-datatables-oracle: ~6.0`
5. Uncomment the provider `Yajra\Datatables\DatatablesServiceProvider`

---

## See Also

- [Installation](/docs/{{package}}/{{version}}/installation) - Fresh installation guide
- [Quick Starter](/docs/{{package}}/{{version}}/quick-starter) - Build your first DataTable
