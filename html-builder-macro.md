# Html Builder Macro

You can extend DataTables HTML Builder using `macro`.

## Example macro:
```php
use Yajra\Datatables\Html\Builder;
use Yajra\Datatables\Html\Column;

Builder::macro('addEditColumn', function () {
	$attributes = [
		'title'          => 'Edit',
		'data'           => 'edit',
		'name'           => '',
		'orderable'      => false,
		'searchable'     => false,
	];

	$this->collection->push(new Column($attributes));

	return $this;
});

```

## Usage
```php
$builder = new Builder;
$builder->addEditColumn()->ajax()->parameters([]);

```