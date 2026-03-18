# HTML Builder Minified AJAX

The `minifiedAjax()` method generates a shortened URL for DataTables AJAX requests. This helps prevent URL length issues that can occur with large DataTables configurations.

## Why Use Minified AJAX?

Standard DataTables AJAX URLs can become very long (up to 1500+ characters) with complex configurations. Minified AJAX reduces this to approximately 500 characters by removing unnecessary query parameters.

## Basic Usage

### Basic Minified AJAX

```php
$html = $builder->minifiedAjax(route('users.data'));
```

### Using Current URL

```php
// Uses the current URL
$html = $builder->minifiedAjax('');
```

### With Custom Data

```php
$html = $builder->minifiedAjax('', null, ['foo' => 'bar']);
```

### With JavaScript Script

```php
$html = $builder->minifiedAjax(route('users.data'), 'd.key = "value";');
```

## Method Signature

```php
public function minifiedAjax($url, $script = '', $data = [])
```

### Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `$url` | string | `''` | URL to fetch data from |
| `$script` | string | `''` | JavaScript code for data function |
| `$data` | array | `[]` | Additional data to append |

## Complete Examples

### Basic Example

```php
Route::get('users', function(Builder $builder) {
    if (request()->ajax()) {
        return DataTables::of(User::query())->toJson();
    }

    $html = $builder
        ->columns([...])
        ->minifiedAjax(route('users.data'));

    return view('users.index', compact('html'));
});
```

### With Custom Data

```php
$html = $builder->minifiedAjax(
    route('users.data'),
    null,
    ['status' => 'active', 'role' => 'admin']
);
```

### With JavaScript Script

```php
$html = $builder->minifiedAjax(
    route('users.data'),
    'd.search = $("#search").val();'
);
```

### Combined Example

```php
$html = $builder->minifiedAjax(
    route('users.data'),
    'd.status = "active";',
    ['custom' => 'value']
);
```

## See Also

- [Html Builder](/docs/{{package}}/{{version}}/html-builder)
- [Html Builder AJAX](/docs/{{package}}/{{version}}/html-builder-ajax)
