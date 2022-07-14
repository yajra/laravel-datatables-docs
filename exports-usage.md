# Export Usage

## Usage

1. Add the export-button livewire component on your view file that uses dataTable class.

```php
<livewire:export-button :table-id="$dataTable->getTableId()" />
```

2. On your `DataTable` class, use `WithExportQueue`

```php
use Yajra\DataTables\WithExportQueue;

class PermissionsDataTable extends DataTable
{
    use WithExportQueue;
    
    ...
}
```

3. Run your queue worker. Ex: `php artisan queue:work`
