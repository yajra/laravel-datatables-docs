# XSS filtering

To prevent XSS attack on some of your columns, you can use `escapeColumns` api.

<a name="selected"></a>
## Escape selected fields

```php
return Datatables::eloquent(Role::select())
		    ->escapeColumns(['name'])
		    ->make(true);
```
<a name="all"></a>
## Escape all columns

```php
return Datatables::eloquent(Role::select())
		    ->escapeColumns()
		    ->make(true);
```

<a name="none"></a>
## Remove escaping of all columns

```php
return Datatables::eloquent(Role::select())
		    ->escapeColumns([])
		    ->make(true);
```

<a name="index"></a>
## Escape by output index

```php
return Datatables::eloquent(Role::select())
		    ->escapeColumns([0])
		    ->make();
 ```