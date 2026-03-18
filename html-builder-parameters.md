---
title: HTML Builder Parameters
description: Configure DataTables JavaScript behavior with fluent methods
---

# HTML Builder Parameters

Use fluent methods to configure DataTables JavaScript behavior.

---

## Basic Usage

```php
$html = $builder
    ->paging(true)
    ->searching(true)
    ->info(false)
    ->searchDelay(350);
```

---

## Available Parameters

### Display Options

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `paging` | bool | `true` | Enable/disable pagination |
| `searching` | bool | `true` | Enable/disable search |
| `info` | bool | `true` | Show table information |
| `lengthChange` | bool | `true` | Allow changing page length |
| `ordering` | bool | `true` | Enable/disable ordering |
| `pageLength` | int | `10` | Number of records per page |

### AJAX Options

| Parameter | Type | Description |
|-----------|------|-------------|
| `ajax` | string\|object | AJAX configuration |
| `serverSide` | bool | Enable server-side processing |
| `processing` | bool | Show processing indicator |

### Language Options

| Parameter | Type | Description |
|-----------|------|-------------|
| `language` | object | Language configuration |

---

## Complete Example

```php
use Yajra\DataTables\Html\Builder;

$html = $builder
    ->paging(true)
    ->searching(true)
    ->info(true)
    ->lengthChange(true)
    ->pageLength(25)
    ->searchDelay(350)
    ->processing(true)
    ->serverSide(true);
```

---

## See Also

- [HTML Builder](/docs/{{package}}/{{version}}/html-builder) - Main HTML Builder documentation
- [HTML Builder Callbacks](/docs/{{package}}/{{version}}/html-builder-callbacks) - JavaScript callbacks
