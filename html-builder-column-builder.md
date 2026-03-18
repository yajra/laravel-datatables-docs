---
title: Column Builder
description: Build columns using the Column builder
---

# HTML Builder - Column Builder

The Column Builder is a fluent interface for building column definitions.

---

<a name="basic"></a>
## Basic Usage

```php
use Yajra\DataTables\Html\Column;

Column::make('id')
    ->title('ID')
    ->data('id')
    ->name('id')
```

### Using Column::computed()

Computed columns are rendered on the client-side:

```php
use Yajra\DataTables\Html\Column;

Column::computed('action', 'Action')
    ->orderable(false)
    ->searchable(false)
    ->render('function(data, type, row) { ... }');
```

### Using Column::checkbox()

```php
use Yajra\DataTables\Html\Column;

Column::checkbox()
    ->title('<input type="checkbox" id="dataTablesCheckbox"/>');
```

---

## Common Methods

| Method | Description |
|--------|-------------|
| `make()` | Create a standard column |
| `computed()` | Create a computed column |
| `checkbox()` | Create a checkbox column |
| `index()` | Create an index column |
| `action()` | Create an action column |

---

## See Also

- [HTML Builder](/docs/{{package}}/{{version}}/html-builder) - Main HTML Builder documentation
- [HTML Builder Column](/docs/{{package}}/{{version}}/html-builder-column) - Detailed column options
