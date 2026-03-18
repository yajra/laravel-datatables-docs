# HTML Builder Index Column

The `addIndex()` method provides a quick way to add an index column to your DataTable.

## Basic Usage

```php
$html = $builder->addIndex();
```

### With Custom Attributes

```php
$html = $builder->addIndex([
    'title' => '#',
    'width' => '50px',
]);
```

## See Also

- [Html Builder](/docs/{{package}}/{{version}}/html-builder)
