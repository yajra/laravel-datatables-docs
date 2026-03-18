# HTML Builder Post AJAX

The `postAjax()` method configures DataTables to use POST method for fetching data from the server. This is useful when you need to send larger amounts of data or when you want to avoid URL length limitations.

## Basic Usage

### Using a Route

```php
$html = $builder->postAjax(route('users.data'));
```

### Using a URL

```php
$html = $builder->postAjax(url('users/data'));
```

### Using Current URL

```php
// Uses the current URL
$html = $builder->postAjax('');
```

## POST AJAX Configuration

### Array Configuration

```php
$html = $builder->postAjax([
    'url' => route('users.data'),
]);
```

### With Custom Data

```php
$html = $builder->postAjax([
    'url' => route('users.data'),
    'data' => 'function(d) { d.key = "value"; }',
]);
```

## Complete Examples

### Basic POST AJAX

```php
Route::get('users', function(Builder $builder) {
    if (request()->ajax()) {
        return DataTables::of(User::query())->toJson();
    }

    $html = $builder
        ->columns([...])
        ->postAjax(route('users.data'));

    return view('users.index', compact('html'));
});
```

### POST AJAX with Custom Data

```php
$html = $builder->postAjax([
    'url' => route('users.data'),
    'data' => 'function(d) {
        d.status = "active";
        d.role = "admin";
    }',
]);
```

### POST AJAX with Form Data

```php
$html = $builder->postAjaxWithForm(route('users.data'), '#search-form');
```

## See Also

- [Html Builder](/docs/{{package}}/{{version}}/html-builder)
- [Html Builder AJAX](/docs/{{package}}/{{version}}/html-builder-ajax)
