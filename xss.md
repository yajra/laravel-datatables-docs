# XSS filtering

Since v7.0, all dataTable response are encoded to prevent XSS attack. In case you need to display html on your columns, you can use `rawColumns` api.

> `action` column is allowed as raw by default.

<a name="raw"></a>

## Raw Columns
```php
return Datatables::eloquent(Role::select())
		    ->rawColumns(['name', 'action'])
		    ->make(true);
```

# Other XSS methods

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