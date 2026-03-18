---
title: Post Ajax
description: Configure POST AJAX requests for DataTables
---

# HTML Builder Post AJAX

Use POST method to submit the DataTables AJAX request.

---

## Basic Usage

```php
use Yajra\DataTables\Html\Builder;

$html = $builder->postAjax(route('users.data'));
```

### With Custom Data

```php
use Yajra\DataTables\Html\Builder;

$html = $builder->postAjax([
    'url' => route('users.data'),
    'data' => 'function(d) { d.key = "value"; }',
]);
```

---

## When to Use POST

| Use Case | Method | Reason |
|----------|--------|--------|
| Small datasets | GET | Simpler, cacheable |
| Large datasets | POST | Avoid URL length limits |
| Internet Explorer | POST | Better compatibility |
| Sensitive data | POST | Data not in URL |

---

## See Also

- [HTML Builder AJAX](/docs/{{package}}/{{version}}/html-builder-ajax) - AJAX configuration
- [HTML Builder Minified AJAX](/docs/{{package}}/{{version}}/html-builder-minified-ajax) - Minified AJAX
