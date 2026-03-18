---
title: SearchPanes Hide Columns
description: Exclude columns from SearchPanes
---

# Exclude Columns from SearchPanes

Some columns you might not want in your SearchPanes. To hide them, add `->searchPanes(false)` in your column definition.

---

<a name="basic"></a>
## Basic Usage

```php
<?php

use Yajra\DataTables\Html\Builder;
use Yajra\DataTables\Html\Column;

public function html(): Builder
{
    return $this->builder()
        ->columns($this->getColumns());
}

protected function getColumns(): array
{
    return [
        Column::make('id')
            ->searchPanes(false),  // Exclude from SearchPanes
        Column::make('name'),
        Column::make('email'),
        Column::make('status'),
    ];
}
```

---

## Common Use Cases

### Exclude ID Column

```php
Column::make('id')
    ->searchPanes(false),
```

### Exclude Action Column

```php
Column::make('action')
    ->searchPanes(false),
```

### Exclude Computed Columns

```php
Column::computed('full_name')
    ->searchPanes(false),
```

---

## Available Options

| Option | Description |
|--------|-------------|
| `->searchPanes(false)` | Exclude from SearchPanes |
| `->searchPanes(true)` | Include in SearchPanes (default) |

---

## See Also

- [SearchPanes Getting Started](/docs/{{package}}/{{version}}/search-panes-starter) - Basic setup
- [SearchPanes Options](/docs/{{package}}/{{version}}/search-panes-options) - More configuration
