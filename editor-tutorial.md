---
title: DataTables Editor Tutorial
description: Complete tutorial for building CRUD interfaces with DataTables Editor
---

# DataTables Editor Tutorial

> [!NOTE]
> This tutorial guides you through building a complete CRUD interface using Laravel DataTables Editor with **Laravel 13**. Estimated time: **15-20 minutes**.

---

## Prerequisites

This tutorial assumes you have completed the [Quick Starter](/docs/{{package}}/{{version}}/quick-starter) guide and have a working DataTables setup.

---

## Overview

By the end of this tutorial, you will have:
- A DataTable with inline Create, Edit, and Delete functionality
- Backend processing with validation
- A fully working CRUD interface

---

## Step 1: Obtain Editor License

> [!WARNING]
> The Editor library requires a [paid license](https://editor.datatables.net/purchase/index).

1. Purchase a license from DataTables Editor
2. Download the Editor package from your account
3. Copy and rename your `Editor.XX.zip` to `Editor.zip`
4. Move it to your project root folder

---

## Step 2: Configure Package.json

Register the postinstall script in your `package.json` to automatically install Editor assets:

```json
{
    "scripts": {
        "dev": "vite",
        "build": "vite build",
        "postinstall": "node node_modules/datatables.net-editor/install.js ./Editor.zip"
    }
}
```

---

## Step 3: Install Editor Assets

```bash
npm i datatables.net-editor datatables.net-editor-bs5
```

---

## Step 4: Register Editor Scripts

Update your `resources/js/app.js`:

```javascript
import './bootstrap';
import 'laravel-datatables-vite';

import "datatables.net-editor";
import Editor from "datatables.net-editor-bs5";

Editor(window, $);
```

---

## Step 5: Add Editor Styles

Update `resources/sass/app.scss`:

```scss
// Fonts
@import url('https://fonts.bunny.net/css?family=Nunito');

// Variables
@import 'variables';

// Bootstrap
@import 'bootstrap/scss/bootstrap';

// DataTables
@import 'bootstrap-icons/font/bootstrap-icons.css';
@import "datatables.net-bs5/css/dataTables.bootstrap5.css";
@import "datatables.net-buttons-bs5/css/buttons.bootstrap5.css";
@import "datatables.net-editor-bs5/css/editor.bootstrap5.css";
@import 'datatables.net-select-bs5/css/select.bootstrap5.css';
```

---

## Step 6: Recompile Assets

```bash
npm run dev
```

---

## Step 7: Create DataTable Class

Create or update `app/DataTables/UsersDataTable.php`:

```php
<?php
// app/DataTables/UsersDataTable.php

namespace App\DataTables;

use App\Models\User;
use Illuminate\Database\Eloquent\Builder as QueryBuilder;
use Yajra\DataTables\EloquentDataTable;
use Yajra\DataTables\Html\Builder as HtmlBuilder;
use Yajra\DataTables\Html\Button;
use Yajra\DataTables\Html\Column;
use Yajra\DataTables\Html\Editor\Editor;
use Yajra\DataTables\Html\Editor\Fields;
use Yajra\DataTables\Services\DataTable;

class UsersDataTable extends DataTable
{
    public function dataTable(QueryBuilder $query): EloquentDataTable
    {
        return (new EloquentDataTable($query))->setRowId('id');
    }

    public function query(User $model): QueryBuilder
    {
        return $model->newQuery();
    }

    public function html(): HtmlBuilder
    {
        return $this->builder()
                    ->setTableId('users-table')
                    ->columns($this->getColumns())
                    ->minifiedAjax()
                    ->orderBy(1)
                    ->selectStyleSingle()
                    ->editors([
                        Editor::make()
                              ->fields([
                                  Fields\Text::make('name'),
                                  Fields\Text::make('email'),
                              ]),
                    ])
                    ->buttons([
                        Button::make('create')->editor('editor'),
                        Button::make('edit')->editor('editor'),
                        Button::make('remove')->editor('editor'),
                        Button::make('excel'),
                        Button::make('csv'),
                        Button::make('pdf'),
                        Button::make('print'),
                        Button::make('reset'),
                        Button::make('reload'),
                    ]);
    }

    public function getColumns(): array
    {
        return [
            Column::make('id'),
            Column::make('name'),
            Column::make('email'),
            Column::make('created_at'),
            Column::make('updated_at'),
        ];
    }

    protected function filename(): string
    {
        return 'Users_' . date('YmdHis');
    }
}
```

---

## Step 8: Create Editor Class

Generate the Editor class using artisan:

```bash
php artisan datatables:editor Users --model
```

> [!TIP]
> This creates `app/DataTables/UsersDataTableEditor.php` with basic CRUD methods and generics.

---

## Step 9: Register Routes

Edit `routes/web.php`:

```php
<?php
// routes/web.php

use App\DataTables\UsersDataTable;
use App\DataTables\UsersDataTableEditor;
use Illuminate\Support\Facades\Route;

Route::get('/users', UsersDataTable::class)->name('users.index');
Route::post('/users', UsersDataTableEditor::class)->name('users.store');
```

> [!NOTE]
> For simple routes, you can pass the class directly. CSRF token is automatically handled by the DataTable scripts.

---

## Step 10: Create Blade View

Create `resources/views/users/index.blade.php`:

```blade
@extends('layouts.app')

@section('content')
<div class="container">
    {{ $dataTable->table() }}
</div>
@endsection

@push('scripts')
{{ $dataTable->scripts(attributes: ['type' => 'module']) }}
@endpush
```

---

## Next Steps

Now that you have a working CRUD interface:

1. **[Editor Rules](/docs/{{package}}/{{version}}/editor-rules)** - Add validation rules for your fields
2. **[Editor Events](/docs/{{package}}/{{version}}/editor-events)** - Hook into create, update, and delete operations
3. **[Editor Model](/docs/{{package}}/{{version}}/editor-model)** - Configure your model for Editor
