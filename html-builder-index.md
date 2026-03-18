---
title: Add Index Column
description: Add index/row number column
---

# HTML Builder Index Column

The `addIndex()` method provides a quick way to add an index column to your DataTable.

---

## Basic Usage

```php
use Yajra\DataTables\Html\Builder;

$html = $builder->addIndex();
```

### With Custom Attributes

```php
use Yajra\DataTables\Html\Builder;

$html = $builder->addIndex([
    'title' => '#',
    'width' => '50px',
]);
```

---

## Default Attributes

| Attribute | Default | Description |
|-----------|---------|-------------|
| `data` | `DT_RowIndex` | Column data key |
| `name` | `DT_RowIndex` | Column name |
| `title` | `#` | Column header |
| `orderable` | `false` | Enable/disable sorting |
| `searchable` | `false` | Enable/disable search |

---

## See Also

- [HTML Builder](/docs/{{package}}/{{version}}/html-builder) - Main HTML Builder documentation
- [HTML Builder Column](/docs/{{package}}/{{version}}/html-builder-column) - Column configuration
- [Index Column](/docs/{{package}}/{{version}}/index-column) - Server-side index column
