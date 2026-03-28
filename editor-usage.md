---
title: Using DataTables Editor
description: Integrate DataTables Editor with routes and frontend
---


# Using DataTables Editor

> [!NOTE]
> This guide shows how to integrate your Editor class with routes and frontend.

---
 
<a name="overview"></a>
## Overview
 
DataTables Editor processes all actions (create, edit, remove, forceDelete, restore, upload, and custom actions) via **POST requests**. The Editor handles:
- Validation
- Database transactions
- Response formatting
 
---
 
<a name="supported-actions"></a>
## Supported Actions
 
DataTables Editor supports the following actions:
 
| Action | Method | Description |
|--------|--------|-------------|
| Create | `create()` | Creates new records |
| Edit | `edit()` | Updates existing records |
| Remove | `remove()` | Soft deletes records (if model uses SoftDeletes) |
| Force Delete | `forceDelete()` | Permanently deletes records (requires SoftDeletes) |
| Restore | `restore()` | Restores soft deleted records (requires SoftDeletes) |
| Upload | `upload()` | Handles file uploads |
| Custom Actions | Defined in `$customActions` | Custom business logic actions |
 
> [!NOTE]
> - The forceDelete action uses the same validation rules as the remove action.
> - The restore action uses the same validation rules as the edit action.
> - Custom actions are defined in the `$customActions` property and are handled by the custom action's own logic.
 
---

<a name="complete-setup-flow"></a>
## Complete Setup Flow
 
```
┌──────────────┐     POST      ┌─────────────────┐
│  Frontend    │ ───────────►  │  Laravel Route  │
│  DataTables  │               │  /editor        │
└──────────────┘               └────────┬────────┘
                                          │
                                          ▼
                                 ┌─────────────────┐
                                 │  Editor Class   │
                                 │  process()      │
                                 └────────┬────────┘
                                          │
                     ┌────────────────────┼────────────────────┼────────────────────┐
                     ▼                    ▼                    ▼                    ▼
             ┌──────────────┐    ┌──────────────┐    ┌──────────────┐    ┌──────────────┐
             │ createRules  │    │ editRules    │    │ removeRules  │    │ Custom     │
             └──────────────┘    └──────────────┘    └──────────────┘    │  Actions   │
                                                                        └──────────────┘
```

---

<a name="step-1"></a>
## Step 1: Create Your Editor

Generate using the artisan command:

```bash
php artisan datatables:editor Users --model
```

Or create manually:

```php
<?php
// app/DataTables/UsersDataTableEditor.php

namespace App\DataTables;

use App\Models\User;
use Illuminate\Database\Eloquent\Model;
use Illuminate\Validation\Rule;
use Yajra\DataTables\DataTablesEditor;

/** @extends DataTablesEditor<User> **/
class UsersDataTableEditor extends DataTablesEditor
{
    protected $model = User::class;

    public function createRules(): array
    {
        return [
            'name'  => 'required',
            'email' => 'required|email|unique:users',
        ];
    }

    public function editRules(Model $model): array
    {
        return [
            'email' => 'sometimes|required|email|' . Rule::unique('users')->ignore($model->getKey()),
        ];
    }

    public function removeRules(Model $model): array
    {
        return [];
    }
}
```

---

<a name="step-2"></a>
## Step 2: Setup Editor Model

Configure your model with fillable attributes:

```php
<?php
// app/Models/User.php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class User extends Model
{
    protected $fillable = [
        'name',
        'email',
        'password',
    ];
}
```

> [!TIP]
> See [Editor Model](/docs/{{package}}/{{version}}/editor-model) for detailed configuration.

---

<a name="step-3"></a>
## Step 3: Register Route

Add a POST route to handle all Editor actions:

```php
<?php
// routes/web.php

use App\DataTables\UsersDataTable;
use App\DataTables\UsersDataTableEditor;
use Illuminate\Support\Facades\Route;

Route::get('/users', UsersDataTable::class)->name('users.index');
Route::post('/users', UsersDataTableEditor::class)->name('users.store');
```

> [!TIP]
> For simple use cases with no additional transformations, you can pass the class directly to the route.

Or with a controller for more control:

```php
<?php
// app/Http/Controllers/UserController.php

namespace App\Http\Controllers;

use App\DataTables\UsersDataTable;
use App\DataTables\UsersDataTableEditor;

class UserController extends Controller
{
    public function index(UsersDataTable $dataTable)
    {
        return $dataTable->render('users.index');
    }

    public function store(UsersDataTableEditor $editor)
    {
        return $editor->process(request());
    }
}
```

---

## Step 4: CSRF Token

> [!NOTE]
> CSRF token is automatically handled by `$dataTable->scripts()`, no additional configuration needed.

---

## Step 5: Configure Editor Buttons

Update your DataTable HTML builder:

```php
<?php
// app/DataTables/UsersDataTable.php

use Yajra\DataTables\Html\Builder as HtmlBuilder;
use Yajra\DataTables\Html\Button;
use Yajra\DataTables\Html\Editor;

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
                ]);
}
```

---

## Step 6: Render in Blade

Add the DataTable to your Blade view:

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

> [!NOTE]
> The Editor initialization script is automatically included in `$dataTable->scripts()`.

---

## Frontend Generator

> [!TIP]
> Use the [DataTables Editor Generator](https://editor.datatables.net/generator/index) to quickly generate frontend code.

1. Visit the [Editor Generator](https://editor.datatables.net/generator/index)
2. Configure your table fields
3. Copy the generated JavaScript
4. Paste into your Blade template

---

## See Also

| Guide | Description |
|-------|-------------|
| [Editor Installation](/docs/{{package}}/{{version}}/editor-installation) | Install the Editor plugin |
| [Editor Tutorial](/docs/{{package}}/{{version}}/editor-tutorial) | Complete step-by-step tutorial |
| [Editor Rules](/docs/{{package}}/{{version}}/editor-rules) | Validation rules reference |
| [Editor Events](/docs/{{package}}/{{version}}/editor-events) | Event hooks documentation |
| [Editor Model](/docs/{{package}}/{{version}}/editor-model) | Model configuration |
| [DataTables Editor](https://editor.datatables.net/examples/index) | Official Editor documentation |
