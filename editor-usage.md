# Using DataTables Editor

<a name="using-editor"></a>

> {info} This guide shows how to integrate your Editor class with routes and frontend.

---

<a name="overview"></a>
## Overview

DataTables Editor processes all actions (create, edit, remove) via **POST requests**. The Editor handles:
- Validation
- Database transactions
- Response formatting

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
                    ┌────────────────────┼────────────────────┐
                    ▼                    ▼                    ▼
            ┌──────────────┐    ┌──────────────┐    ┌──────────────┐
            │ createRules  │    │ editRules    │    │ removeRules  │
            └──────────────┘    └──────────────┘    └──────────────┘
```

---

<a name="step-1-create-editor"></a>
## Step 1: Create Your Editor

Generate using the artisan command:

```bash
php artisan datatables:editor Users --model
```

Or create manually:

```php
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

<a name="step-2-setup-editor-model"></a>
## Step 2: Setup Editor Model

Configure your model with fillable attributes:

```php
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

> {tip} See [Editor Model](/docs/{{package}}/{{version}}/editor-model) for detailed configuration.

---

<a name="step-3-register-route"></a>
## Step 3: Register Route

Add a POST route to handle all Editor actions:

```php
// routes/web.php
use App\DataTables\UsersDataTable;
use App\DataTables\UsersDataTableEditor;

Route::get('/users', UsersDataTable::class)->name('users.index');
Route::post('/users', UsersDataTableEditor::class)->name('users.store');
```

> {tip} For simple use cases with no additional transformations, you can pass the class directly to the route.

Or with a controller for more control:

```php
use App\Http\Controllers\UserController;

Route::get('/users', [UserController::class, 'index'])->name('users.index');
Route::post('/users', [UserController::class, 'store'])->name('users.store');
```

Full controller example:

```php
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

<a name="step-4-csrf-token"></a>
## Step 4: CSRF Token

> {info} CSRF token is automatically handled by `$dataTable->scripts()`, no additional configuration needed.

---

<a name="step-5-configure-editor-buttons"></a>
## Step 5: Configure DataTable Editor Buttons

Update your DataTable HTML builder:

```php
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

<a name="step-6-render-in-blade"></a>
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

> {info} The Editor initialization script is automatically included in `$dataTable->scripts()`.

---

<a name="frontend-generator"></a>
## Frontend Generator

> {tip} Use the [DataTables Editor Generator](https://editor.datatables.net/generator/index) to quickly generate frontend code.

1. Visit the [Editor Generator](https://editor.datatables.net/generator/index)
2. Configure your table fields
3. Copy the generated JavaScript
4. Paste into your Blade template

---

<a name="related-documentation"></a>
## Related Documentation

| Guide | Description |
|-------|-------------|
| [Editor Installation](/docs/{{package}}/{{version}}/editor-installation) | Install the Editor plugin |
| [Editor Tutorial](/docs/{{package}}/{{version}}/editor-tutorial) | Complete step-by-step tutorial |
| [Editor Rules](/docs/{{package}}/{{version}}/editor-rules) | Validation rules reference |
| [Editor Events](/docs/{{package}}/{{version}}/editor-events) | Event hooks documentation |
| [Editor Model](/docs/{{package}}/{{version}}/editor-model) | Model configuration |
| [DataTables Editor](https://editor.datatables.net/examples/index) | Official Editor documentation |
