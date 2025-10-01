# Html Builder - Column Control

Column Control is an extension for DataTables that allows adding column-specific controls such as ordering and searching.

See [datatables.net](https://datatables.net/) official documentation for [`column control config`](https://datatables.net/extensions/columncontrol/config) for details.

Column Control can be configured using the `columnControl` method. It is available in both `Yajra\DataTables\Html\Builder` and `Yajra\DataTables\Html\Column` classes.
```php
$builder->columnControl($attributes);
$column->columnControl($attributes);
```
The difference is that when using it in the `Yajra\DataTables\Html\Column` class, the configuration will only apply to that specific column. Whereas using it in the `Yajra\DataTables\Html\Builder` class will apply the configuration to all columns.

Helper methods are also available for easier configuration. Here are some examples:

**Add search input to each column header**
```php
$builder->columnControlSearch();
// instead of
$builder->columnControl([
    [
        'target' => 'thead:1', // second row of the thead
        'content' => ['search'],
    ]
]);
```

**Add search input to each column footer**
```php
$builder->columnControlFooterSearch();
// instead of
$builder->columnControl([
    [
        'target' => 'tfoot',
        'content' => ['search'],
    ]
]);
```

**Add content to header or footer**
```php
$content = ['search'];
$target = 1;

$builder
    ->columnControlHeader($content, $target)
    ->columnControlFooter($content, $target);
```

See [available contents](https://datatables.net/reference/content/) for more options.
