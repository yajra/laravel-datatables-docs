---
title: Column Builder
description: Build columns using the Column builder
---


# HTML Builder - Column Builder

The Column Builder is a fluent interface for building column definitions.

## Basic Usage

```php
use Yajra\DataTables\Html\Column;

Column::make('id')
    ->title('ID')
    ->data('id')
    ->name('id');
```

### Using Column::computed()

```php
Column::computed('action', 'Action')
    ->orderable(false)
    ->searchable(false)
    ->render('function(data, type, row) { ... }');
```

### Using Column::checkbox()

```php
Column::checkbox()
    ->title('<input type="checkbox" id="dataTablesCheckbox"/>');
```

## See Also

- [Html Builder](/docs/{{package}}/{{version}}/html-builder)
