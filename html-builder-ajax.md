# Html Builder Ajax

Ajax is an option that you set to tell `dataTable` where to fetch it's data.

See [datatables.net](https://datatables.net/) official documentation for [`ajax option`](https://datatables.net/reference/option/ajax) for details.

**Syntax**
```php
$builder->ajax($attributes);
```

## Ajax Parameter
Ajax parameter (`$attributes`) can either be a string or an array.

**String Attributes**

When the attribute passed is a `string`. The builder will treat this as the `URL` where we fetch our data.

```php
$builder->ajax(route('users.data'));
```

> {tip} Setting ajax to `null` or `empty string` will use the current url where Datatables was used.

**Array Attributes**

Accepted options are `url` and `data`.

```php
$builder->ajax([
	'url' => route('users.index'),
	'type' => 'GET',
	'data' => 'function(d) { d.key = "value"; }',
])
```

### URL Option
URL option represents the `url` where dataTables will fetch it's json data.

### Type Option
Type option represents the type of request (`GET/POST`)  that we will use when sending a request to the server.

### Data Option
Data option is a `js` string that you can use to append custom data when sending the request to the server.



