# Export Usage

## Purging exported files

On `app\Console\Kernel.php`, register the purge command

```php
$schedule->command('datatables:purge-export')->weekly();
```

## Export Filename

You can set the export filename by setting the property.

```php
<livewire:export-button :table-id="$dataTable->getTableId()" filename="my-table.xlsx" />
<livewire:export-button :table-id="$dataTable->getTableId()" filename="my-table.csv" />

<livewire:export-button :table-id="$dataTable->getTableId()" :filename="$filename" />
```
