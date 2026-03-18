# HTML Builder Post AJAX

Use POST method to submit the DataTables AJAX request.

## Basic Usage

```php
$html = $builder->postAjax(route('users.data'));
```

### With Custom Data

```php
$html = $builder->postAjax([
    'url' => route('users.data'),
    'data' => 'function(d) { d.key = "value"; }',
]);
```

## See Also

- [Html Builder](/docs/{{package}}/{{version}}/html-builder)
