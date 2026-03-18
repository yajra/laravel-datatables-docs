---
title: HTML Builder Macro
description: Create custom HTML builder macros
---


# HTML Builder Macro

The `macro()` method allows you to extend the HTML Builder with custom functionality.

## Basic Usage

```php
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
$html = $builder->addEditColumn()->ajax(route('users.data'));
```

## See Also

- [Html Builder](/docs/{{package}}/{{version}}/html-builder)
