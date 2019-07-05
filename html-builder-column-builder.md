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
        ->render('function(){}')
        ->footer('Id')
        ->exportable(true)
        ->printable(true);
```

