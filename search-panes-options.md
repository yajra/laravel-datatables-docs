---
title: SearchPanes Options
description: SearchPanes configuration options
---

# SearchPanes Options

This guide covers advanced SearchPanes configuration options.

---

## Basic Configuration

```php
<?php

use Yajra\DataTables\Html\Builder;
use Yajra\DataTables\Html\SearchPane;

public function html(): Builder
{
    return $this->builder()
        ->searchPanes(
            SearchPane::make()
                ->hideCount()        // Hides the count next to the options
                ->hideTotal()        // Hides the total available without filters
                ->clear(false)       // Hides the clear button
                ->controls(false)    // Disable controls for filter search
                ->layout('columns-2') // Search panes in one row
                ->order(['user_id', 'age'])  // Order of search panes
                ->orderable(false)   // Hide order icons
                ->panes([])          // Additional search panes
                ->dtOpts([           // DataTables options for panes
                    'paging' => true,
                    'pagingType' => 'numbers'
                ])
        );
}
```

---

## SearchPane Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `hideCount` | bool | `false` | Hide count next to options |
| `hideTotal` | bool | `false` | Hide total count without filters |
| `clear` | bool | `true` | Show/hide clear button |
| `controls` | bool | `true` | Enable/disable search controls |
| `layout` | string | `'columns-1'` | Layout of panes |
| `order` | array | `[]` | Order of panes |
| `orderable` | bool | `true` | Show order icons |
| `panes` | array | `[]` | Additional custom panes |
| `dtOpts` | array | `[]` | DataTables options for panes |

---

## Layout Options

| Layout | Description |
|--------|-------------|
| `columns-1` | 1 column of panes |
| `columns-2` | 2 columns of panes |
| `columns-3` | 3 columns of panes |
| `columns-4` | 4 columns of panes |

---

## See Also

- [SearchPanes Getting Started](/docs/{{package}}/{{version}}/search-panes-starter) - Basic setup
- [SearchPanes Hide Columns](/docs/{{package}}/{{version}}/search-panes-hide-columns) - Exclude columns
