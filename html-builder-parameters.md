---
title: HTML Builder Parameters
description: Configure DataTables JavaScript behavior with fluent methods
---

# HTML Builder Parameters

Use fluent methods to configure DataTables JavaScript behavior.

---

<a name="basic"></a>
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
| `lengthMenu` | array | `[10, 25, 50, 100]` | Available page length options |
| `pagingType` | string | `simple_numbers` | Pagination style |
| `stripeClasses` | array | `[]` | CSS classes for row striping |
| `tabIndex` | int | `0` | Keyboard navigation tab index |
| `renderer` | string | `bootstrap` | Bootstrap renderer |
| `retrieve` | bool | `false` | Retrieve existing instance |
| `destroy` | bool | `false` | Destroy existing instance |

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

### Scroll Options

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `autoWidth` | bool | `false` | Enable automatic column width |
| `deferRender` | bool | `false` | Defer rendering for performance |
| `scrollX` | bool | `false` | Enable horizontal scrolling |
| `scrollY` | bool\|string | `false` | Enable vertical scrolling |
| `scrollCollapse` | bool | `false` | Collapse table on scroll |

### State Options

| Parameter | Type | Description |
|-----------|------|-------------|
| `stateSave` | bool | Save table state |
| `stateDuration` | int | State duration in seconds |
| `rowId` | string | Row ID attribute |

### Search Options

| Parameter | Type | Description |
|-----------|------|-------------|
| `searchDelay` | int | Search delay in milliseconds |
| `search` | array | Initial search settings |
| `searchCols` | array | Initial column search settings |

### Order Options

| Parameter | Type | Description |
|-----------|------|-------------|
| `order` | array | Default ordering |
| `orderMulti` | bool | Enable multi-column sorting |
| `orderCellsTop` | bool | Order on first cell only |
| `orderClasses` | bool | Highlight ordered column |

### Deferred Loading

| Parameter | Type | Description |
|-----------|------|-------------|
| `deferLoading` | int\|array | Defer data loading |
| `displayStart` | int | Initial display start |

### Layout

| Parameter | Type | Description |
|-----------|------|-------------|
| `layout` | array\|callable | Layout configuration |
| `dom` | string | DOM configuration (deprecated) |

---

<a name="order"></a>
## Ordering Methods

```php
$html = $builder
    // Single column order
    ->orderBy(0, 'desc')
    
    // Multiple column orders
    ->orders([[0, 'desc'], [1, 'asc']])
    
    // Fixed order (always applied)
    ->orderByFixed(0, 'asc');
```

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
    ->serverSide(true)
    ->autoWidth(true)
    ->scrollX(true)
    ->scrollY('500px')
    ->stateSave(true)
    ->orderBy(0, 'desc');
```

---

## See Also

- [HTML Builder](/docs/{{package}}/{{version}}/html-builder) - Main HTML Builder documentation
- [HTML Builder Callbacks](/docs/{{package}}/{{version}}/html-builder-callbacks) - JavaScript callbacks
- [HTML Builder Layout](/docs/{{package}}/{{version}}/html-builder-layout) - Layout configuration
