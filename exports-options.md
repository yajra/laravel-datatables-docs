# Export Options

## Export Type

You can set the export type by setting the property to `csv` or `xlsx`. Default value is `xlsx`.

```php
<livewire:export-button :table-id="$dataTable->getTableId()" type="xlsx" />
<livewire:export-button :table-id="$dataTable->getTableId()" type="csv" />
```

## Set Excel Sheet Name

Option 1: You can set the Excel sheet name by setting the property.

```php
<livewire:export-button :table-id="$dataTable->getTableId()" sheet-name="Monthly Report" />
```

Option 2: You can also set the Excel sheet name by overwriting the method.

```php
protected function sheetName() : string
{
    return "Yearly Report";
}
```

## Formatting Columns

You can format the column by setting it via Column definition on you DataTable service class.

```php
Column::make('mobile')->exportFormat('00000000000'),
```

The format above will treat mobile numbers as text with leading zeroes.

## Numeric Fields Formatting

The package will auto-detect numeric fields and can be used with custom formats.

```php
Column::make('total')->exportFormat('0.00'),
Column::make('count')->exportFormat('#,##0'),
Column::make('average')->exportFormat('#,##0.00'),
```

## Date Fields Formatting

The package will auto-detect date fields when used with a valid format or is a DateTime instance.

```php
Column::make('report_date')->exportFormat('mm/dd/yyyy'),
Column::make('created_at'),
Column::make('updated_at')->exportFormat(NumberFormat::FORMAT_DATE_DATETIME),
```

## Valid Date Formats

Valid date formats can be adjusted on `datatables-export.php` config file.

```php
    'date_formats' => [
        'mm/dd/yyyy',
        NumberFormat::FORMAT_DATE_DATETIME,
        NumberFormat::FORMAT_DATE_YYYYMMDD,
        NumberFormat::FORMAT_DATE_XLSX22,
        NumberFormat::FORMAT_DATE_DDMMYYYY,
        NumberFormat::FORMAT_DATE_DMMINUS,
        NumberFormat::FORMAT_DATE_DMYMINUS,
        NumberFormat::FORMAT_DATE_DMYSLASH,
        NumberFormat::FORMAT_DATE_MYMINUS,
        NumberFormat::FORMAT_DATE_TIME1,
        NumberFormat::FORMAT_DATE_TIME2,
        NumberFormat::FORMAT_DATE_TIME3,
        NumberFormat::FORMAT_DATE_TIME4,
        NumberFormat::FORMAT_DATE_TIME5,
        NumberFormat::FORMAT_DATE_TIME6,
        NumberFormat::FORMAT_DATE_TIME7,
        NumberFormat::FORMAT_DATE_XLSX14,
        NumberFormat::FORMAT_DATE_XLSX15,
        NumberFormat::FORMAT_DATE_XLSX16,
        NumberFormat::FORMAT_DATE_XLSX17,
        NumberFormat::FORMAT_DATE_YYYYMMDD2,
        NumberFormat::FORMAT_DATE_YYYYMMDDSLASH,
    ]
```

## Force Numeric Field As Text Format

Option to force auto-detected numeric value as text format.

```php
Column::make('id')->exportFormat('@'),
Column::make('id')->exportFormat(NumberFormat::FORMAT_GENERAL),
Column::make('id')->exportFormat(NumberFormat::FORMAT_TEXT),
```
