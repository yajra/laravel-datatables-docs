---
title: Buttons Quick Starter
description: Get started with DataTables Buttons plugin
---

# Buttons Quick Starter

This guide helps you get started with the DataTables Buttons plugin.

---

<a name="create-users-datatable"></a>
<a name="create"></a>
## Create Users DataTable

```bash
php artisan datatables:make Users
```

---

<a name="update-users-datatable"></a>
<a name="update"></a>
## Update UsersDataTable

Update `UsersDataTable` class and set the columns and parameters needed to render our DataTable:

```php
<?php
// app/DataTables/UsersDataTable.php

namespace App\DataTables;

use App\Models\User;
use Yajra\DataTables\Html\Column;
use Yajra\DataTables\Html\Button;
use Yajra\DataTables\Services\DataTable;

class UsersDataTable extends DataTable
{
    public function html()
    {
        return $this->builder()
                    ->columns($this->getColumns())
                    ->layout([
                        'topStart' => 'buttons',
                        'topEnd' => 'search',
                        'bottomStart' => 'info',
                        'bottomEnd' => 'paging',
                    ])
                    ->buttons([
                        Button::make('export'),
                        Button::make('print'),
                        Button::make('reset'),
                        Button::make('reload'),
                    ]);
    }

    protected function getColumns(): array
    {
        return [
            Column::make('id'),
            Column::make('name'),
            Column::make('email'),
            Column::make('created_at'),
            Column::make('updated_at'),
        ];
    }
}
```

---

<a name="example-route"></a>
<a name="route"></a>
## Example Route

```php
<?php
// routes/web.php

use App\DataTables\UsersDataTable;
use Illuminate\Support\Facades\Route;

Route::get('users', function(UsersDataTable $dataTable) {
    return $dataTable->render('users.index');
});
```

---

<a name="example-view"></a>
<a name="view"></a>
## Example View

Create a view at `resources/views/users/index.blade.php`:

```blade
@extends('layouts.app')

@section('content')
    {!! $dataTable->table() !!}
@endsection

@push('scripts')
    {!! $dataTable->scripts() !!}
@endpush
```

---

## See Also

- [Buttons Installation](/docs/{{package}}/{{version}}/buttons-installation) - Install buttons plugin
- [Buttons Export](/docs/{{package}}/{{version}}/buttons-export) - Export options
- [Buttons Custom](/docs/{{package}}/{{version}}/buttons-custom) - Custom buttons
