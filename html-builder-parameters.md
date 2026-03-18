# HTML Builder Parameters

Parameters are the options you pass to configure DataTables JavaScript behavior. These options control features like pagination, searching, ordering, and more.

## Basic Usage

```php
$html = $builder->parameters([
    'paging' => true,
    'searching' => true,
    'info' => false,
    'searchDelay' => 350,
]);
```

## Available Parameters

### Display Options

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `paging` | bool | `true` | Enable/disable pagination |
| `searching` | bool | `true` | Enable/disable search |
| `info` | bool | `true` | Show table information |
| `lengthChange` | bool | `true` | Allow changing page length |
| `ordering` | bool | `true` | Enable/disable ordering |
| `order` | array | `[]` | Initial sort order |
| `pageLength` | int | `10` | Number of records per page |
| `displayStart` | int | `0` | Start position for pagination |
| `deferRender` | bool | `false` | Defer row rendering |
| `scrollX` | bool\|string | `false` | Enable horizontal scrolling |
| `scrollY` | string | `''` | Enable vertical scrolling |

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
| `language.url` | string | Path to language file |

### Performance Options

| Parameter | Type | Description |
|-----------|------|-------------|
| `searchDelay` | int | Delay before searching (ms) |
| `deferLoading` | int\|array | Defer initial loading |

## Complete Examples

### Basic Configuration

```php
$html = $builder->parameters([
    'paging' => true,
    'searching' => true,
    'info' => false,
    'lengthChange' => false,
]);
```

### Advanced Configuration

```php
$html = $builder->parameters([
    'paging' => true,
    'searching' => true,
    'info' => true,
    'lengthChange' => true,
    'pageLength' => 25,
    'lengthMenu' => [[10, 25, 50, 100], [10, 25, 50, 100]],
    'searchDelay' => 350,
    'processing' => true,
    'serverSide' => true,
]);
```

### Language Configuration

```php
$html = $builder->parameters([
    'language' => [
        'url' => url('js/dataTables/language.json'),
    ],
]);
```

### Custom Language

```php
$html = $builder->parameters([
    'language' => [
        'emptyTable' => 'No data available',
        'info' => 'Showing _START_ to _END_ of _TOTAL_ entries',
        'infoEmpty' => 'Showing 0 to 0 of 0 entries',
        'infoFiltered' => '(filtered from _MAX_ total entries)',
        'lengthMenu' => 'Show _MENU_ entries',
        'loadingRecords' => 'Loading...',
        'processing' => 'Processing...',
        'search' => 'Search:',
        'zeroRecords' => 'No matching records found',
    ],
]);
```

### Scroll Configuration

```php
$html = $builder->parameters([
    'scrollX' => true,
    'scrollY' => '400px',
    'scrollCollapse' => true,
    'paging' => false,
]);
```

### Order Configuration

```php
$html = $builder->parameters([
    'order' => [[0, 'asc'], [1, 'desc']],
]);
```

### Defer Loading

```php
$html = $builder->parameters([
    'deferLoading' => 57, // Load 57 records initially
]);
```

## Fluent Methods

The Builder also provides fluent methods for common parameters:

```php
$html = $builder
    ->paging(true)
    ->searching(true)
    ->info(false)
    ->lengthChange(false)
    ->pageLength(25)
    ->searchDelay(350)
    ->processing(true)
    ->serverSide(true);
```

## See Also

- [Html Builder](/docs/{{package}}/{{version}}/html-builder)
- [Html Builder Callbacks](/docs/{{package}}/{{version}}/html-builder-callbacks)
