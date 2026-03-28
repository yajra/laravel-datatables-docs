---
title: HTML Builder AJAX
description: Configure DataTables AJAX data fetching
---

# HTML Builder AJAX

The `ajax()` method configures how DataTables fetches data from the server.

---

<a name="basic"></a>
## Basic Usage

### Using a Route

```php
use Yajra\DataTables\Html\Builder;

$html = $builder->ajax(route('users.data'));
```

### Using Current URL

```php
// Uses the current URL
$html = $builder->ajax('');
```

---

## AJAX Configuration

### Array Configuration

```php
use Yajra\DataTables\Html\Builder;

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

---

## POST AJAX

```php
use Yajra\DataTables\Html\Builder;

$html = $builder->postAjax(route('users.data'));
```

---

## Pipeline AJAX

```php
use Yajra\DataTables\Html\Builder;

$html = $builder->pipeline(route('users.data'), 5);
```

---

<a name="minified"></a>
## Minified AJAX

Minify the AJAX URL by removing unnecessary parameters:

```php
$html = $builder->minifiedAjax();

// With custom URL
$html = $builder->minifiedAjax(route('users.data'));

// With additional data
$html = $builder->minifiedAjax(route('users.data'), null, ['key' => 'value']);

// With custom AJAX parameters
$html = $builder->minifiedAjax(route('users.data'), null, [], ['cache' => true]);
```

---

<a name="form"></a>
## AJAX with Form Data

### Using Form Selector (GET)

Automatically include form data in the AJAX request:

```php
$html = $builder->ajaxWithForm(route('users.data'), '#search-form');
```

### Using Form Selector (POST)

```php
$html = $builder->postAjaxWithForm(route('users.data'), '#search-form');
```

---

<a name="url"></a>
## Get AJAX URL

Retrieve the configured AJAX URL:

```php
$url = $builder->getAjaxUrl();
```

---

## See Also

- [HTML Builder](/docs/{{package}}/{{version}}/html-builder) - Main HTML Builder documentation
- [HTML Builder Table](/docs/{{package}}/{{version}}/html-builder-table) - Table generation
