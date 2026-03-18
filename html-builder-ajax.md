# HTML Builder AJAX

The `ajax()` method configures how DataTables fetches data from the server. It supports various configuration options including URL, request type, and custom data handling.

## Basic Usage

### Using a Route

```php
$html = $builder->ajax(route('users.data'));
```

### Using a URL

```php
$html = $builder->ajax(url('users/data'));
```

### Using Current URL

```php
// Uses the current URL
$html = $builder->ajax('');
```

## AJAX Configuration Options

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
| `contentType` | string | Content type for POST requests |
| `headers` | object | Additional HTTP headers |

## POST AJAX

Use `postAjax()` for POST requests:

```php
$html = $builder->postAjax(route('users.data'));
```

Or with custom configuration:

```php
$html = $builder->postAjax([
    'url' => route('users.data'),
    'data' => 'function(d) { d.custom = "value"; }',
]);
```

## Minified AJAX

The `minifiedAjax()` method shortens URLs by removing unnecessary query parameters:

```php
$html = $builder->minifiedAjax(route('users.data'));
```

### With Custom Data

```php
$html = $builder->minifiedAjax('', null, ['foo' => 'bar']);
```

### With Script

```php
$html = $builder->minifiedAjax(route('users.data'), 'd.key = "value";');
```

## Pipeline AJAX

Use the DataTables pipeline plugin for large datasets:

```php
$html = $builder->pipeline(route('users.data'), 5);
```

## AJAX with Form Data

Submit form data with AJAX requests:

```php
$html = $builder->postAjaxWithForm(route('users.data'), '#search-form');
```

## Complete Examples

### Basic AJAX

```php
Route::get('users', function(Builder $builder) {
    if (request()->ajax()) {
        return DataTables::of(User::query())->toJson();
    }

    $html = $builder
        ->columns([...])
        ->ajax(route('users.data'));

    return view('users.index', compact('html'));
});
```

### AJAX with Custom Data

```php
$html = $builder->ajax([
    'url' => route('users.data'),
    'type' => 'GET',
    'data' => 'function(d) {
        d.status = "active";
        d.role = "admin";
    }',
]);
```

### AJAX with Custom DataSrc

```php
$html = $builder->ajax([
    'url' => route('users.data'),
    'dataSrc' => 'data',
]);
```

### POST AJAX with Custom Headers

```php
$html = $builder->postAjax([
    'url' => route('users.data'),
    'headers' => [
        'X-Custom-Header' => 'value',
    ],
]);
```

## See Also

- [Html Builder](/docs/{{package}}/{{version}}/html-builder)
- [Html Builder Post AJAX](/docs/{{package}}/{{version}}/html-builder-post-ajax)
- [Html Builder Minified AJAX](/docs/{{package}}/{{version}}/html-builder-minified-ajax)
