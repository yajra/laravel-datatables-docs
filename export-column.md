---
title: Export Column
description: Configure columns for export functionality
---


# Export Columns

This guide explains how to configure which columns are included in exports and how to customize their headers.

<a name="overview"></a>
## Overview

By default, exports use the same columns defined in your DataTable's HTML configuration. However, you can customize which columns appear in exports and their display names.

<a name="defining-export-columns"></a>
## Defining Export Columns

<a name="option-1-simple-array"></a>
### Option 1: Simple Array

Use a simple array to specify column data keys:

```php
/**
 * Define export columns.
 * Only these columns will be included in the export.
 */
protected $exportColumns = [
    'id',
    'name',
    'email',
    'created_at',
];
```

<a name="option-2-with-custom-titles"></a>
### Option 2: With Custom Titles

Use an array of objects to customize column headers in the export:

```php
/**
 * Define export columns with custom titles.
 */
protected $exportColumns = [
    ['data' => 'id', 'title' => 'ID'],
    ['data' => 'name', 'title' => 'Full Name'],
    ['data' => 'email', 'title' => 'Email Address'],
    ['data' => 'created_at', 'title' => 'Registration Date'],
];
```

<a name="option-3-mixed-definition"></a>
### Option 3: Mixed Definition

You can mix simple and object definitions:

```php
protected $exportColumns = [
    ['data' => 'id', 'title' => 'ID'],  // Custom title
    'name',                              // Uses 'name' as both data and title
    'email',                             // Uses 'email' as both data and title
    ['data' => 'created_at', 'title' => 'Date'],  // Custom title
];
```

<a name="complete-example"></a>
## Complete Example

```php
<?php

namespace App\DataTables;

use App\Models\User;
use Yajra\DataTables\Html\Column;
use Yajra\DataTables\Services\DataTable;
use Yajra\DataTables\WithExportQueue;

class UsersDataTable extends DataTable
{
    use WithExportQueue;

    /**
     * Build the DataTable class.
     */
    public function dataTable($query)
    {
        return datatables()
            ->eloquent($query)
            ->addColumn('action', 'users.datatables.action')
            ->rawColumns(['action']);
    }

    /**
     * Define the HTML columns (shown in the UI).
     */
    protected function getColumns(): array
    {
        return [
            Column::make('name'),
            Column::make('email'),
            Column::make('role'),
            Column::make('created_at'),
            Column::make('action')
                ->title('Actions')
                ->exportable(false)  // Exclude from export
                ->printable(false),
            Column::make('id'),
        ];
    }

    /**
     * Define export columns (may differ from HTML columns).
     */
    protected $exportColumns = [
        ['data' => 'id', 'title' => 'User ID'],
        ['data' => 'name', 'title' => 'Full Name'],
        ['data' => 'email', 'title' => 'Email Address'],
        ['data' => 'role', 'title' => 'User Role'],
        ['data' => 'created_at', 'title' => 'Registration Date'],
    ];

    // ... other methods
}
```

<a name="column-properties"></a>
## Column Properties

<a name="exportable"></a>
### Exportable

Control whether a column appears in exports:

```php
// Exclude from export
Column::make('action')->exportable(false),

// Include in export (default)
Column::make('name')->exportable(true),
```

<a name="printable"></a>
### Printable

Control whether a column appears in print view:

```php
Column::make('action')->printable(false),
```

<a name="custom-export-rendering"></a>
### Custom Export Rendering

Apply custom formatting for exports:

```php
Column::make('status')
    ->exportRender(fn ($model, $value) => ucfirst($value)),

Column::make('is_active')
    ->exportRender(fn ($model, $value) => $value ? 'Yes' : 'No'),
```

<a name="relationship-columns"></a>
## Relationship Columns

You can export columns from relationships:

```php
protected $exportColumns = [
    ['data' => 'id', 'title' => 'ID'],
    ['data' => 'name', 'title' => 'Name'],
    ['data' => 'department.name', 'title' => 'Department'],
    ['data' => 'company.name', 'title' => 'Company'],
];
```

<a name="nested-relationships-with-join"></a>
### Nested Relationships with Join

For more complex relationship exports:

```php
protected function getColumns(): array
{
    return [
        Column::make('name'),
        Column::make('department.name')->title('Department'),
        Column::make('company.name')->title('Company'),
        Column::make('id'),
    ];
}

protected $exportColumns = [
    ['data' => 'id', 'title' => 'ID'],
    ['data' => 'name', 'title' => 'Name'],
    ['data' => 'department.name', 'title' => 'Department'],
    ['data' => 'company.name', 'title' => 'Company'],
];
```

<a name="common-patterns"></a>
## Common Patterns

<a name="exclude-action-columns"></a>
### Exclude Action Columns

```php
protected $exportColumns = [
    ['data' => 'name', 'title' => 'Name'],
    ['data' => 'email', 'title' => 'Email'],
    ['data' => 'created_at', 'title' => 'Created'],
];
```

<a name="different-columns-for-export-vs-display"></a>
### Different Columns for Export vs Display

```php
// HTML table (shown in browser)
protected function getColumns(): array
{
    return [
        Column::make('name'),
        Column::make('actions')
            ->title('')
            ->exportable(false),
        Column::make('id'),
    ];
}

// Export columns
protected $exportColumns = [
    ['data' => 'id', 'title' => 'ID'],
    ['data' => 'name', 'title' => 'Name'],
];
```

<a name="calculated-columns"></a>
### Calculated Columns

For columns with custom content:

```php
Column::make('full_name')
    ->exportRender(fn ($model) => $model->first_name . ' ' . $model->last_name),
```

<a name="related-documentation"></a>
## Related Documentation

- [Installation](exports-installation.md) - Initial setup
- [Export Usage](exports-usage.md) - Basic usage guide
- [Export Options](exports-options.md) - Column formatting options
