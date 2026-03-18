---
title: Add Checkbox Column
description: Add checkbox column for row selection
---


# HTML Builder Checkbox Column

The `addCheckbox()` method provides a quick way to add a checkbox column to your DataTable.

## Basic Usage

```php
$html = $builder->addCheckbox();
```

### With Custom Attributes

```php
$html = $builder->addCheckbox([
    'title' => '<input type="checkbox" id="dataTablesCheckbox"/>',
]);
```

## Default Attributes

```php
[
    'defaultContent' => '<input type="checkbox" />',
    'title' => '<input type="checkbox" id="dataTablesCheckbox"/>',
    'data' => 'checkbox',
    'name' => 'checkbox',
    'orderable' => false,
    'searchable' => false,
    'exportable' => false,
    'printable' => true,
    'width' => '10px',
]
```

## See Also

- [Html Builder](/docs/{{package}}/{{version}}/html-builder)
