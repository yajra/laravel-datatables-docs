# HTML Builder AJAX

The `ajax()` method configures how DataTables fetches data from the server.

## Basic Usage

### Using a Route

```php
$html = $builder->ajax(route('users.data'));
```

### Using Current URL

```php
// Uses the current URL
$html = $builder->ajax('');
```

## AJAX Configuration

### Array Configuration

```php
$html = $builder->ajax([
    'url' => route('users.data'),
    'type' => 'GET',
    'data' => 'function(d) { d.key = "value"; }',
]);
```

### Available Options

| Option | Type | Description |
|--------|------|-------------|
| `url` | string | URL to fetch data from |
| `type` | string | HTTP method (GET/POST) |
| `data` | string | JavaScript function for custom data |
| `dataSrc` | string | Property in JSON response containing data |

## POST AJAX

```php
$html = $builder->postAjax(route('users.data'));
```

## Pipeline AJAX

```php
$html = $builder->pipeline(route('users.data'), 5);
```

## See Also

- [Html Builder](/docs/{{package}}/{{version}}/html-builder)
