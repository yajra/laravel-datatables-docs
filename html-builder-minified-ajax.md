---
title: Minified Ajax
description: Configure minified AJAX requests
---


# HTML Builder Minified AJAX

The `minifiedAjax()` method generates a shortened URL for DataTables AJAX requests.

## Basic Usage

```php
$html = $builder->minifiedAjax(route('users.data'));
```

### With Custom Data

```php
$html = $builder->minifiedAjax('', null, ['foo' => 'bar']);
```

### With JavaScript Script

```php
$html = $builder->minifiedAjax(route('users.data'), 'd.key = "value";');
```

## See Also

- [Html Builder](/docs/{{package}}/{{version}}/html-builder)
