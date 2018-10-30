# Html Builder Post Ajax

Use post method to submit the dataTables ajax request.

See [datatables.net](https://datatables.net/) official documentation for [`ajax option`](https://datatables.net/reference/option/ajax) for details.

**Syntax**
```php
$builder->postAjax($attributes);
```

## Post Ajax Parameter
Ajax parameter (`$attributes`) can either be a string or an array.

**String Attributes**

When the attribute passed is a `string`. The builder will treat this as the `URL` where we fetch our data.

```php
$builder->postAjax(route('users.data'));
```

> {tip} Setting ajax to `null` or `empty string` will use the current url where DataTables was used.

**Array Attributes**

Attributes are the same with the `ajax()` method request valid parameters.

```php
$builder->postAjax([
    'url' => route('users.data'),
])
```


