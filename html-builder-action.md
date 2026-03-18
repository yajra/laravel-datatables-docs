---
title: Add Action Column
description: Add action buttons to DataTables
---


# HTML Builder Action Column

The `addAction()` method provides a quick way to add an action column to your DataTable.

## Basic Usage

```php
$html = $builder->addAction();
```

### With Custom Attributes

```php
$html = $builder->addAction([
    'title' => 'Actions',
    'width' => '100px',
]);
```

## Default Attributes

```php
[
    'defaultContent' => '',
    'data' => 'action',
    'name' => 'action',
    'title' => 'Action',
    'render' => null,
    'orderable' => false,
    'searchable' => false,
    'exportable' => false,
    'printable' => true,
    'footer' => '',
]
```

## See Also

- [Html Builder](/docs/{{package}}/{{version}}/html-builder)
