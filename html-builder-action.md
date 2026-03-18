---
title: Add Action Column
description: Add action buttons to DataTables
---

# HTML Builder Action Column

The `addAction()` method provides a quick way to add an action column to your DataTable.

---

<a name="basic"></a>
## Basic Usage

```php
use Yajra\DataTables\Html\Builder;

$html = $builder->addAction();
```

### With Custom Attributes

```php
use Yajra\DataTables\Html\Builder;

$html = $builder->addAction([
    'title' => 'Actions',
    'width' => '100px',
]);
```

---

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

---

## See Also

- [HTML Builder](/docs/{{package}}/{{version}}/html-builder) - Main HTML Builder documentation
- [HTML Builder Column](/docs/{{package}}/{{version}}/html-builder-column) - Column configuration
