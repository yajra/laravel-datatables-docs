---
title: Extended DataTable
description: Extend DataTable with additional functionality
---

# Extended DataTable

We can extend and reuse our DataTable class inside our controller by using `before` and `response` callback.

---

<a name="quick"></a>
## Quick Example

```php
<?php
// routes/web.php

use App\DataTables\RolesDataTable;
use Yajra\DataTables\DataTableAbstract;
use Illuminate\Support\Facades\Route;

Route::get('datatable', function(RolesDataTable $dataTable) {
    return $dataTable
        ->before(function (DataTableAbstract $dataTable) {
            return $dataTable->addColumn('test', 'added inside controller');
        })
        ->response(function (\Illuminate\Support\Collection $response) {
            $response['test'] = 'Append Data';

            return $response;
        })
        ->withHtml(function(\Yajra\DataTables\Html\Builder $builder) {
            $builder->columns(['id', 'name', 'etc...']);
        })
        ->with('key', 'value')
        ->with([
            'key2' => 'value2',
            'key3' => 'value3',
        ])
        ->render('path.to.view');
});
```

---

<a name="callbacks"></a>
## Available Callbacks

### before Callback

```php
->before(function (DataTableAbstract $dataTable) {
    // Add columns, modify query, etc.
    return $dataTable;
})
```

### response Callback

```php
->response(function (\Illuminate\Support\Collection $response) {
    // Modify the response
    $response['custom_field'] = 'value';
    return $response;
})
```

### withHtml Callback

```php
->withHtml(function (\Yajra\DataTables\Html\Builder $builder) {
    $builder->columns(['id', 'name']);
})
```

---

## See Also

- [Buttons With Parameters](/docs/{{package}}/{{version}}/buttons-with) - Pass parameters to DataTable
- [Buttons Installation](/docs/{{package}}/{{version}}/buttons-installation) - Install buttons plugin
