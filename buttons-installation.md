---
title: Buttons Plugin Installation
description: Install and configure the Buttons plugin for DataTables export functionality
---


# Buttons Plugin Installation

A Laravel DataTables plugin for handling server-side exporting of tables as CSV, Excel, PDF, and more.

---

<a name="installation"></a>
## Installation

Run the following command in your project to get the latest version of the plugin:

```bash
composer require yajra/laravel-datatables-buttons:"^13.0"
```

---

<a name="configuration"></a>
<a name="config"></a>
## Configuration

> [!NOTE]
> This step is optional. The package works with default configuration out of the box.

Publish the configuration file to customize button behavior:

```bash
php artisan vendor:publish --tag=datatables-buttons
```

This creates `config/datatables-buttons.php` with default settings:

```php
<?php

return [
    /*
     * DataTable namespace configuration.
     */
    'namespace' => [
        'base'  => 'DataTables',
        'model' => '',
    ],

    /*
     * PDF generator to use: 'excel' or 'snappy'.
     */
    'pdf_generator' => 'excel',

    /*
     * Snappy PDF options (when using snappy generator).
     */
    'snappy' => [
        'options' => [
            'no-outline'    => true,
            'margin-left'   => '0',
            'margin-right'  => '0',
            'margin-top'    => '10mm',
            'margin-bottom' => '10mm',
        ],
        'orientation' => 'landscape',
    ],
];
```

---

## Available Export Types

| Type | Description | Required Package |
|------|-------------|------------------|
| `excel` | Excel export (.xlsx) | maatwebsite/excel |
| `csv` | CSV export (.csv) | Built-in |
| `pdf` | PDF export (.pdf) | dompdf, mpdf, or snappy |
| `print` | Print view | Built-in |

---

## Quick Start

Add export buttons to your DataTable:

```php
use Yajra\DataTables\Html\Builder as HtmlBuilder;
use Yajra\DataTables\Html\Button;

public function html(): HtmlBuilder
{
    return $this->builder()
                ->setTableId('users-table')
                ->columns($this->getColumns())
                ->minifiedAjax()
                ->orderBy(1)
                ->buttons([
                    Button::make('excel'),
                    Button::make('csv'),
                    Button::make('pdf'),
                    Button::make('print'),
                    Button::make('reset'),
                    Button::make('reload'),
                ]);
}
```

---

## See Also

- [Buttons Configuration](/docs/{{package}}/{{version}}/buttons-config) - Detailed configuration options
- [Buttons Export](/docs/{{package}}/{{version}}/buttons-export) - Export button options
- [Buttons Custom](/docs/{{package}}/{{version}}/buttons-custom) - Custom button actions
- [HTML Builder](/docs/{{package}}/{{version}}/html-builder) - Table configuration
