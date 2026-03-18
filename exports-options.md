---
title: Export Options
description: Configuration options for exports
---


# Export Options

This guide covers advanced customization options for exports including export types, sheet names, and column formatting.

<a name="export-types"></a>
<a name="types"></a>
## Export Types

You can set the export file type using the `type` property. Available options are `xlsx` (Excel) and `csv`. The default is `xlsx`.

<a name="via-livewire-component"></a>
### Via Livewire Component

```blade
{{-- Excel export (default) --}}
<livewire:export-button :table-id="$dataTable->getTableId()" type="xlsx" />

{{-- CSV export --}}
<livewire:export-button :table-id="$dataTable->getTableId()" type="csv" />
```

<a name="excel-sheet-name"></a>
## Excel Sheet Name

<a name="via-component-property"></a>
### Via Component Property

Set the Excel sheet name using the `sheet-name` property:

```blade
<livewire:export-button :table-id="$dataTable->getTableId()" sheet-name="Monthly Report" />
```

<a name="via-datatable-method-override"></a>
### Via DataTable Method Override

For a more permanent solution, override the `sheetName()` method in your DataTable class:

```php
<?php

namespace App\DataTables;

use App\Models\User;
use Yajra\DataTables\Services\DataTable;
use Yajra\DataTables\WithExportQueue;

class UsersDataTable extends DataTable
{
    use WithExportQueue;

    /**
     * Customize the Excel sheet name.
     * Note: Excel sheet names have a 31 character limit.
     */
    protected function sheetName(): string
    {
        return 'Yearly Report';
    }

    // ... rest of your DataTable methods
}
```

<a name="column-formatting"></a>
## Column Formatting

You can customize how columns are formatted in exports using the `exportFormat()` method in your column definitions.

<a name="text-format-preserve-leading-zeroes"></a>
### Text Format (Preserve Leading Zeroes)

Use text format for IDs, phone numbers, or any value where leading zeroes should be preserved:

```php
Column::make('id')->exportFormat('@'),
Column::make('mobile')->exportFormat('00000000000'),
Column::make('phone')->exportFormat(NumberFormat::FORMAT_TEXT),
```

<a name="numeric-fields-formatting"></a>
### Numeric Fields Formatting

Numeric columns can be formatted with custom decimal places and thousand separators:

```php
Column::make('price')->exportFormat('0.00'),
Column::make('count')->exportFormat('#,##0'),
Column::make('average')->exportFormat('#,##0.00'),
Column::make('percentage')->exportFormat('0.00%'),
Column::make('currency')->exportFormat('$#,##0.00'),
```

<a name="date-fields-formatting"></a>
### Date Fields Formatting

Date columns can be formatted using standard Excel date formats:

```php
// Simple date format
Column::make('report_date')->exportFormat('mm/dd/yyyy'),

// Date and time
Column::make('created_at')->exportFormat(NumberFormat::FORMAT_DATE_DATETIME),

// Other common formats
Column::make('start_date')->exportFormat(NumberFormat::FORMAT_DATE_YYYYMMDD),
Column::make('end_date')->exportFormat(NumberFormat::FORMAT_DATE_DDMMYYYY),
```

<a name="valid-date-formats"></a>
### Valid Date Formats

The following date formats are supported for auto-detection:

```php
'date_formats' => [
    'mm/dd/yyyy',
    // Standard Excel formats
    NumberFormat::FORMAT_DATE_DATETIME,
    NumberFormat::FORMAT_DATE_YYYYMMDD,
    NumberFormat::FORMAT_DATE_XLSX22,
    NumberFormat::FORMAT_DATE_DDMMYYYY,
    NumberFormat::FORMAT_DATE_DMMINUS,
    NumberFormat::FORMAT_DATE_DMYMINUS,
    NumberFormat::FORMAT_DATE_DMYSLASH,
    NumberFormat::FORMAT_DATE_MYMINUS,
    // Time formats
    NumberFormat::FORMAT_DATE_TIME1,
    NumberFormat::FORMAT_DATE_TIME2,
    NumberFormat::FORMAT_DATE_TIME3,
    NumberFormat::FORMAT_DATE_TIME4,
    NumberFormat::FORMAT_DATE_TIME5,
    NumberFormat::FORMAT_DATE_TIME6,
    NumberFormat::FORMAT_DATE_TIME7,
    // Legacy Excel formats
    NumberFormat::FORMAT_DATE_XLSX14,
    NumberFormat::FORMAT_DATE_XLSX15,
    NumberFormat::FORMAT_DATE_XLSX16,
    NumberFormat::FORMAT_DATE_XLSX17,
    NumberFormat::FORMAT_DATE_YYYYMMDD2,
    NumberFormat::FORMAT_DATE_YYYYMMDDSLASH,
]
```

<a name="force-text-format-for-numeric-values"></a>
### Force Text Format for Numeric Values

To ensure numeric-looking values are treated as text (useful for IDs, codes, etc.):

```php
// Using text format
Column::make('sku')->exportFormat('@'),
Column::make('product_code')->exportFormat(NumberFormat::FORMAT_GENERAL),
Column::make('account_number')->exportFormat(NumberFormat::FORMAT_TEXT),
```

<a name="custom-export-rendering"></a>
## Custom Export Rendering

You can provide a custom callback for exporting specific column values:

```php
Column::make('status')
    ->exportRender(fn ($model, $value) => ucfirst($value)),

Column::make('price')
    ->exportRender(fn ($model, $value) => number_format($value, 2)),
```

<a name="complete-example"></a>
## Complete Example

Here's a comprehensive example showing multiple formatting options:

```php
<?php

namespace App\DataTables;

use App\Models\Order;
use Yajra\DataTables\Html\Column;
use Yajra\DataTables\Services\DataTable;
use Yajra\DataTables\WithExportQueue;
use PhpOffice\PhpSpreadsheet\Style\NumberFormat;

class OrdersDataTable extends DataTable
{
    use WithExportQueue;

    protected function sheetName(): string
    {
        return 'Orders Report';
    }

    protected function getColumns(): array
    {
        return [
            Column::make('id')->exportFormat('@'), // Text format for ID
            Column::make('order_number')->exportFormat('@'),
            Column::make('customer_name'),
            Column::make('total')->exportFormat('$#,##0.00'), // Currency format
            Column::make('quantity')->exportFormat('#,##0'),
            Column::make('order_date')->exportFormat('mm/dd/yyyy'),
            Column::make('created_at')->exportFormat(NumberFormat::FORMAT_DATE_DATETIME),
            Column::make('status')
                ->exportRender(fn ($model, $value) => ucfirst($value)),
        ];
    }
}
```

<a name="configuration-reference"></a>
## Configuration Reference

These options can be configured globally in `config/datatables-export.php`:

```php
return [
    // Default date format when no specific format is set
    'default_date_format' => 'yyyy-mm-dd',
    
    // Auto-detect date fields using these formats
    'date_formats' => [
        'mm/dd/yyyy',
        NumberFormat::FORMAT_DATE_DATETIME,
        // ... other formats
    ],
    
    // Treat values with these formats as text
    'text_formats' => [
        NumberFormat::FORMAT_TEXT,
        NumberFormat::FORMAT_GENERAL,
    ],
];
```

<a name="related-documentation"></a>
## Related Documentation

- [Installation](exports-installation.md) - Initial setup
- [Export Usage](exports-usage.md) - Basic usage guide
- [Export Columns](export-column.md) - Configure export-specific columns
