---
title: Minified Ajax
description: Configure minified AJAX requests for DataTables
---

# HTML Builder Minified AJAX

The `minifiedAjax()` method generates a shortened URL for DataTables AJAX requests.

---

## Basic Usage

```php
use Yajra\DataTables\Html\Builder;

$html = $builder->minifiedAjax(route('users.data'));
```

### With Custom Data

```php
use Yajra\DataTables\Html\Builder;

$html = $builder->minifiedAjax('', null, ['foo' => 'bar']);
```

### With JavaScript Script

```php
use Yajra\DataTables\Html\Builder;

$html = $builder->minifiedAjax(route('users.data'), 'd.key = "value";');
```

---

## How It Works

The `minifiedAjax` method:
1. Generates a short URL hash
2. Stores the full AJAX configuration on the server
3. Returns the hash as a URL parameter

This is useful for:
- Reducing URL length
- Handling complex data parameters
- Improving security by not exposing parameters in URL

---

## See Also

- [HTML Builder AJAX](/docs/{{package}}/{{version}}/html-builder-ajax) - AJAX configuration
- [HTML Builder Post AJAX](/docs/{{package}}/{{version}}/html-builder-post-ajax) - POST requests
