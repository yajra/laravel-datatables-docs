# Export Columns

You can export a column customised header if manually set.

<a name="export-column-with-title"></a>
## Export Columns with Custom Title

```php
protected $exportColumns = [
    ['data' => 'name', 'title' => 'Name'],
    ['data' => 'email', 'title' => 'Registered Email'],
];
```

<a name="export-column"></a>
## Export Columns

```php
protected $exportColumns = [
    'name',
    'email',
];
```