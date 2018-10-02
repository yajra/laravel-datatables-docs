# Print Column

You can print a column customised header if manually set.

<a name="print-column-with-title"></a>
## Print Columns with Custom Title

```php
protected $printColumns = [
    ['data' => 'name', 'title' => 'Name'],
    ['data' => 'email', 'title' => 'Registered Email'],
];
```

<a name="print-column"></a>
## Print Columns

```php
protected $printColumns = [
    'name',
    'email',
];
```