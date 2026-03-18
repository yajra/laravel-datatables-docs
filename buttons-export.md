# DataTable Buttons - Export

This guide covers the export buttons available in the main DataTables package. For queue-based exports with Livewire, see the [Export Usage](exports-usage.md) guide.

> **Note**: These buttons use client-side or server-side export functionality with yajra/DataTables Buttons plugin. For large datasets, consider using the [Queue Export Package](exports-installation.md) instead.

<a name="export-button-group"></a>
## Export Button Group

Enable the complete export button group (Excel, CSV, and PDF buttons) with a single setting:

```php
namespace App\DataTables;

use App\Models\User;
use Yajra\DataTables\Html\Button;
use Yajra\DataTables\Services\DataTable;

class UsersDataTable extends DataTable
{
    public function html()
    {
        return $this->builder()
                    ->columns($this->getColumns())
                    ->layout([
                        'topStart' => 'buttons',
                        'topEnd' => 'search',
                        'bottomStart' => 'info',
                        'bottomEnd' => 'paging',
                    ])
                    ->buttons([
                        Button::make('export'),
                    ]);
    }
}
```

<a name="individual-export-buttons"></a>
## Individual Export Buttons

<a name="export-as-excel"></a>
### Export as Excel

Enable only the Excel export button:

```php
public function html()
{
    return $this->builder()
                ->columns($this->getColumns())
                ->layout([
                    'topStart' => 'buttons',
                    'topEnd' => 'search',
                    'bottomStart' => 'info',
                    'bottomEnd' => 'paging',
                ])
                ->buttons([
                    Button::make('excel'),
                ]);
}
```

<a name="export-as-csv"></a>
### Export as CSV

Enable only the CSV export button:

```php
public function html()
{
    return $this->builder()
                ->columns($this->getColumns())
                ->layout([
                    'topStart' => 'buttons',
                    'topEnd' => 'search',
                    'bottomStart' => 'info',
                    'bottomEnd' => 'paging',
                ])
                ->buttons([
                    Button::make('csv'),
                ]);
}
```

<a name="export-as-pdf"></a>
### Export as PDF

Enable only the PDF export button:

```php
public function html()
{
    return $this->builder()
                ->columns($this->getColumns())
                ->layout([
                    'topStart' => 'buttons',
                    'topEnd' => 'search',
                    'bottomStart' => 'info',
                    'bottomEnd' => 'paging',
                ])
                ->buttons([
                    Button::make('pdf'),
                ]);
}
```

<a name="post-method-export"></a>
## POST Method Export

For large datasets or when using Internet Explorer, use POST method exports to avoid URL length limitations:

```php
public function html()
{
    return $this->builder()
                ->columns($this->getColumns())
                ->layout([
                    'topStart' => 'buttons',
                    'topEnd' => 'search',
                    'bottomStart' => 'info',
                    'bottomEnd' => 'paging',
                ])
                ->buttons([
                    Button::make('postExcel'),
                    Button::make('postCsv'),
                    Button::make('postPdf'),
                ]);
}
```

<a name="required-route-setup"></a>
### Required Route Setup

Add a POST route for the export endpoint in your routes file:

```php
use App\Http\Controllers\DataTableController;

// GET route for initial page load
Route::resource('sample', DataTableController::class);

// POST route for export functionality
Route::post('sample/export', [DataTableController::class, 'index'])
    ->name('sample.export');
```

<a name="available-button-options"></a>
## Available Button Options

| Button | Description |
|--------|-------------|
| `export` | Export button group (Excel, CSV, PDF) |
| `excel` | Export to Excel format |
| `csv` | Export to CSV format |
| `pdf` | Export to PDF format |
| `postExcel` | Export to Excel using POST |
| `postCsv` | Export to CSV using POST |
| `postPdf` | Export to PDF using POST |

<a name="utility-buttons"></a>
## Utility Buttons

<a name="print-button"></a>
### Print Button

Enable print preview functionality:

```php
public function html()
{
    return $this->builder()
                ->columns($this->getColumns())
                ->layout([
                    'topStart' => 'buttons',
                    'topEnd' => 'search',
                    'bottomStart' => 'info',
                    'bottomEnd' => 'paging',
                ])
                ->buttons([
                    Button::make('print'),
                ]);
}
```

<a name="reset-button"></a>
### Reset Button

Reset the table to its original state:

```php
public function html()
{
    return $this->builder()
                ->columns($this->getColumns())
                ->layout([
                    'topStart' => 'buttons',
                    'topEnd' => 'search',
                    'bottomStart' => 'info',
                    'bottomEnd' => 'paging',
                ])
                ->buttons([
                    Button::make('reset'),
                ]);
}
```

<a name="reload-button"></a>
### Reload Button

Reload the table data without resetting filters:

```php
public function html()
{
    return $this->builder()
                ->columns($this->getColumns())
                ->layout([
                    'topStart' => 'buttons',
                    'topEnd' => 'search',
                    'bottomStart' => 'info',
                    'bottomEnd' => 'paging',
                ])
                ->buttons([
                    Button::make('reload'),
                ]);
}
```

<a name="complete-button-configuration-example"></a>
## Complete Button Configuration Example

```php
public function html()
{
    return $this->builder()
                ->columns($this->getColumns())
                ->layout([
                    'topStart' => 'buttons',
                    'topEnd' => 'search',
                    'bottomStart' => 'info',
                    'bottomEnd' => 'paging',
                ])
                ->buttons([
                    Button::make('export'),
                    Button::make('print'),
                    Button::make('reset'),
                    Button::make('reload'),
                ]);
}
```

<a name="buttons-extension-configuration"></a>
## Buttons Extension Configuration

For advanced button customization, use the fluent buttons method with extended configuration:

```php
public function html()
{
    return $this->builder()
                ->columns($this->getColumns())
                ->layout([
                    'topStart' => 'buttons',
                    'topEnd' => 'search',
                    'bottomStart' => 'info',
                    'bottomEnd' => 'paging',
                ])
                ->buttons([
                    Button::make('collection')
                        ->text('<i class="fa fa-download"></i> Export')
                        ->buttons([
                            Button::make('excel')
                                ->text('Export to Excel')
                                ->title('Users Report'),
                            Button::make('csv')
                                ->text('Export to CSV'),
                            Button::make('pdf')
                                ->text('Export to PDF')
                                ->orientation('landscape'),
                        ]),
                    Button::make('print'),
                    Button::make('reset'),
                    Button::make('reload'),
                ]);
}
```

<a name="comparison-buttons-vs-queue-export"></a>
## Comparison: Buttons vs Queue Export

| Feature | Buttons Export | Queue Export |
|---------|---------------|--------------|
| Dataset size | Small to medium | Large |
| Processing | Synchronous | Asynchronous |
| Timeout risk | High for large data | None |
| User experience | Immediate download | Background job + polling |
| Server load | High during export | Distributed over time |
| Setup complexity | Simple | Requires queue setup |

<a name="related-documentation"></a>
## Related Documentation

- [Export Installation](exports-installation.md) - Queue export package setup
- [Export Usage](exports-usage.md) - Queue export usage
- [Export Options](exports-options.md) - Customize export formatting
- [Export Columns](export-column.md) - Configure export columns
