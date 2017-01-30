# Upgrade Guide

<a name="v6-to-v7"></a>
## Upgrading from v6.x to v7.x
- composer require yajra/laravel-datatables-oracle:^7.0
- composer require yajra/laravel-datatables-buttons:^1.0
- php artisan vendor:publish --tag=datatables --force
- php artisan vendor:publish --tag=datatables-buttons --force

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
