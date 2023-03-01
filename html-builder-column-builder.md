# Html Builder - Column Builder

Column Builder is a fluent class that we can use to build our columns.

<a name="upgrading"></a>
## Upgrading Your Existing Column Definitions

### FROM

```php
$column = [
	'name' => 'id',
	'data' => 'id',
	'title' => 'Id',
	'searchable' => true,
	'orderable' => true,
	'render' => 'function(){}',
	'footer' => 'Id',
	'exportable' => true,
	'printable' => true,
];
```

### TO

```php
use Yajra\DataTables\Html\Column;

$column = Column::make('id')
        ->title('Id')
        ->searchable(true)
        ->orderable(true)
       ->render('\'<div class="editor-attiva" >\' + (full[\'deleted_at\'] == null ? \'<i class="fas fa-check-circle client-is-active"></i>Attivo\' : \'<i class="fas fa-times-circle"></i>Disattivo\') + \'</div>\';\'\'' )
        ->footer('Id')
        ->exportable(true)
        ->printable(true);
```

