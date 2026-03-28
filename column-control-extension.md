---
title: ColumnControl Extension
description: Configure the DataTables ColumnControl extension for advanced column filtering
---

# ColumnControl Extension

The ColumnControl extension provides a powerful column-based filtering interface for DataTables. It allows users to filter data using various control types with logic operators.

---

<a name="installation"></a>
## Installation

The ColumnControl extension is included with the HTML Builder. Ensure you have the required DataTables assets:

```html
<!-- CSS -->
<link rel="stylesheet" href="https://cdn.datatables.net/columncontrol/1.0.0/css/columnControl.dataTables.min.css">

<!-- JS -->
<script src="https://cdn.datatables.net/columncontrol/1.0.0/js/dataTables.columnControl.min.js"></script>
```

---

<a name="basic-usage"></a>
## Basic Usage

Enable ColumnControl in your HTML Builder:

```php
use Yajra\DataTables\Html\Builder;

public function html(): Builder
{
    return $this->builder()
                ->columns($this->getColumns())
                ->columnControl();
}
```

---

<a name="positions"></a>
## Control Positions

ColumnControl can be positioned in the header or footer:

```php
// Header position (default)
->columnControlHeader()

// Footer position
->columnControlFooter()

// Both positions
->columnControlHeader()
->columnControlFooter()
```

---

<a name="search"></a>
## Search Configuration

Enable search functionality in ColumnControl:

```php
// Enable search in header
->columnControlSearch()

// Enable search in footer
->columnControlFooterSearch()

// Enable search dropdown
->columnControlSearchDropdown()
```

---

<a name="spacers"></a>
## Spacers

Add spacing between controls:

```php
->columnControlSpacer()
```

---

<a name="titles"></a>
## Column Titles

Display column titles in the control:

```php
->columnControlTitle()
```

---

<a name="column-configuration"></a>
## Column-Level Configuration

Configure ColumnControl options per column using the `Column` class:

```php
use Yajra\DataTables\Html\Column;

public function getColumns(): array
{
    return [
        Column::make('id')
            ->columnControl()
            ->columnControlSearch(),

        Column::make('name')
            ->columnControl()
            ->columnControlTitle(),

        Column::make('status')
            ->columnControl()
            ->columnControlFooterSearch(),
    ];
}
```

---

<a name="control-types"></a>
## Control Types

### Text Controls

Text-based filtering with operators:

```php
Column::make('name')
    ->columnControl()
    ->columnControlSearch([
        'type' => 'text',
        'options' => ['equal', 'not', 'contains', 'notContains', 'starts', 'ends'],
    ])
```

### Numeric Controls

Numeric comparison filtering:

```php
Column::make('age')
    ->columnControl()
    ->columnControlSearch([
        'type' => 'num',
        'options' => ['equal', 'not', 'greater', 'less', 'greaterOrEqual', 'lessOrEqual'],
    ])
```

### Date Controls

Date-based filtering:

```php
Column::make('created_at')
    ->columnControl()
    ->columnControlSearch([
        'type' => 'date',
        'options' => ['equal', 'notEqual', 'greater', 'less', 'greaterOrEqual', 'lessOrEqual'],
    ])
```

---

<a name="supported-operators"></a>
## Supported Operators

### Text Operators

| Operator | Description |
|----------|-------------|
| `equal` | Exact match |
| `not` | Not equal |
| `contains` | Contains substring |
| `notContains` | Does not contain |
| `starts` | Starts with |
| `ends` | Ends with |
| `empty` | Is empty |
| `notEmpty` | Is not empty |

### Numeric Operators

| Operator | Description |
|----------|-------------|
| `equal` | Equal to |
| `not` | Not equal to |
| `greater` | Greater than |
| `less` | Less than |
| `greaterOrEqual` | Greater than or equal |
| `lessOrEqual` | Less than or equal |

### Date Operators

| Operator | Description |
|----------|-------------|
| `equal` | Date equal |
| `notEqual` | Date not equal (excludes NULL) |
| `greater` | After date |
| `less` | Before date |
| `greaterOrEqual` | On or after date |
| `lessOrEqual` | On or before date |

---

<a name="complete-example"></a>
## Complete Example

```php
<?php

namespace App\DataTables;

use App\Models\User;
use Yajra\DataTables\Html\Builder;
use Yajra\DataTables\Html\Column;
use Yajra\DataTables\Services\DataTable;

class UsersDataTable extends DataTable
{
    public function html(): Builder
    {
        return $this->builder()
                    ->setTableId('users-table')
                    ->columns($this->getColumns())
                    ->minifiedAjax()
                    ->orderBy(1)
                    ->columnControl()
                    ->columnControlHeader()
                    ->columnControlSearch()
                    ->columnControlTitle();
    }

    protected function getColumns(): array
    {
        return [
            Column::make('id')
                ->columnControl(),

            Column::make('name')
                ->columnControl()
                ->columnControlSearch(),

            Column::make('email')
                ->columnControl()
                ->columnControlSearch(),

            Column::make('status')
                ->columnControl()
                ->columnControlSearch([
                    'type' => 'text',
                    'options' => ['equal', 'not'],
                ]),

            Column::make('age')
                ->columnControl()
                ->columnControlSearch([
                    'type' => 'num',
                    'options' => ['greater', 'less', 'equal'],
                ]),

            Column::make('created_at')
                ->columnControl()
                ->columnControlSearch([
                    'type' => 'date',
                    'options' => ['greater', 'less', 'equal'],
                ]),
        ];
    }
}
```

---

## See Also

- [Column Control Search](/docs/{{package}}/{{version}}/column-control-search) - Backend search handling
- [Filter Column](/docs/{{package}}/{{version}}/filter-column) - Custom column filtering
- [SearchPanes Extension](/docs/{{package}}/{{version}}/search-panes-starter) - SearchPanes integration
