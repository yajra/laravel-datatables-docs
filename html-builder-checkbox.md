---
title: Add Checkbox Column
description: Add checkbox column for row selection
---

# HTML Builder Checkbox Column

The `addCheckbox()` method provides a quick way to add a checkbox column to your DataTable.

---

<a name="basic"></a>
## Basic Usage

```php
use Yajra\DataTables\Html\Builder;

$html = $builder->addCheckbox();
```

### With Custom Attributes

```php
use Yajra\DataTables\Html\Builder;

$html = $builder->addCheckbox([
    'title' => '<input type="checkbox" id="dataTablesCheckbox"/>',
]);
```

---

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

---

## Common Use Cases

### Bulk Actions

```javascript
// Get selected row IDs
var selectedRows = table.rows({ selected: true }).ids().toArray();
```

---

## See Also

- [HTML Builder](/docs/{{package}}/{{version}}/html-builder) - Main HTML Builder documentation
- [HTML Builder Column](/docs/{{package}}/{{version}}/html-builder-column) - Column configuration
