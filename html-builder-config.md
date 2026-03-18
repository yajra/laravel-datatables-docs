---
title: HTML Builder Config
description: Configure default HTML Builder settings
---

# HTML Builder Config

The HTML Builder configuration allows you to set default table attributes.

---

## Installation

```bash
php artisan vendor:publish --tag=datatables-html
```

---

## Configuration File

The published config file is located at `config/datatables-html.php`.

---

## Configuration Options

```php
<?php

return [
    'table' => [
        'class' => 'table table-bordered',
        'id' => 'dataTable',
    ],
    'script' => 'datatables::script',
];
```

### Configuration Reference

| Option | Type | Description |
|--------|------|-------------|
| `table.class` | string | Default CSS classes for tables |
| `table.id` | string | Default table ID |
| `script` | string | Blade template path for scripts |
| `scriptNonce` | string | CSP nonce for inline scripts |

---

## See Also

- [HTML Builder](/docs/{{package}}/{{version}}/html-builder) - Main HTML Builder documentation
- [HTML Installation](/docs/{{package}}/{{version}}/html-installation) - Installation guide
