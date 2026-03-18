---
title: HTML Builder Macro
description: Create custom HTML builder macros
---

# HTML Builder Macro

The `macro()` method allows you to extend the HTML Builder with custom functionality.

---

<a name="basic"></a>
## Basic Usage

```php
<?php
// app/Providers/AppServiceProvider.php

use Yajra\DataTables\Html\Builder;
use Yajra\DataTables\Html\Column;

Builder::macro('addEditColumn', function () {
    $attributes = [
        'title' => 'Edit',
        'data' => 'edit',
        'name' => '',
        'orderable' => false,
        'searchable' => false,
    ];

    $this->collection->push(new Column($attributes));

    return $this;
});
```

### Using the Macro

```php
use Yajra\DataTables\Html\Builder;

$html = $builder->addEditColumn()->ajax(route('users.data'));
```

---

## Creating Custom Macros

### Action Buttons Macro

```php
use Yajra\DataTables\Html\Builder;
use Yajra\DataTables\Html\Column;

Builder::macro('addActions', function () {
    $this->collection->push(new Column([
        'data' => 'action',
        'name' => 'action',
        'title' => 'Actions',
        'orderable' => false,
        'searchable' => false,
        'exportable' => false,
        'render' => 'function() { return renderActionButtons(data); }',
    ]));

    return $this;
});
```

### Status Column Macro

```php
Builder::macro('addStatusColumn', function ($title = 'Status') {
    $this->collection->push(new Column([
        'data' => 'status',
        'name' => 'status',
        'title' => $title,
    ]));

    return $this;
});
```

---

## See Also

- [HTML Builder](/docs/{{package}}/{{version}}/html-builder) - Main HTML Builder documentation
- [HTML Builder Column](/docs/{{package}}/{{version}}/html-builder-column) - Column configuration
