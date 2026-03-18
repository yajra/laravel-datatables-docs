# HTML Builder Config

The HTML Builder configuration allows you to set default table attributes.

## Installation

```bash
php artisan vendor:publish --tag=datatables-html
```

## Configuration File

The published config file is located at `config/datatables-html.php`.

## Configuration Options

```php
return [
    'table' => [
        'class' => 'table table-bordered',
        'id' => 'dataTable',
    ],
    'script' => 'datatables::script',
];
```

## See Also

- [Html Builder](/docs/{{package}}/{{version}}/html-builder)
