# Creating a Laravel Full CRUD with DataTables Editor.

Before we begin, please be reminded that the Editor library that we are going to use here requires a paid license.
See [DataTables Editor](https://editor.datatables.net/purchase/index) for details.

## Pre-requisites

This tutorial requires https://yajrabox.com/docs/laravel-datatables/10.0/quick-starter.

## Editor License

Copy and rename your `Editor.XX.zip` to `Editor.zip` and move it to project folder.

## Register postinstall script to package.json

```json
    "scripts": {
        "dev": "vite",
        "build": "vite build",
        "postinstall": "node node_modules/datatables.net-editor/install.js ./Editor.zip"
    },
```

## Install DataTables Editor assets.

```sh
npm i datatables.net-editor datatables.net-editor-bs5
```

## Register editor script on `resources/js/app.js`

```js
import './bootstrap';
import 'laravel-datatables-vite';

import "datatables.net-editor";
import Editor from "datatables.net-editor-bs5";
Editor(window, $);
```

## Add editor styles on `resources/sass/app.scss`.

```css
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

## Recompile assets.

```sh
npm run dev
```

### UsersDataTable.php

Create a new editor instance and add some fields for name and email.

```php
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
    /**
     * Build DataTable class.
     *
     * @param  QueryBuilder  $query  Results from query() method.
     * @return \Yajra\DataTables\EloquentDataTable
     */
    public function dataTable(QueryBuilder $query): EloquentDataTable
    {
        return (new EloquentDataTable($query))
            ->addColumn('action', 'users.action')
            ->setRowId('id');
    }

    /**
     * Get query source of dataTable.
     *
     * @param  \App\Models\User  $model
     * @return \Illuminate\Database\Eloquent\Builder
     */
    public function query(User $model): QueryBuilder
    {
        return $model->newQuery();
    }

    /**
     * Optional method if you want to use html builder.
     *
     * @return \Yajra\DataTables\Html\Builder
     */
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

    /**
     * Get the dataTable columns definition.
     *
     * @return array
     */
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

    /**
     * Get filename for export.
     *
     * @return string
     */
    protected function filename(): string
    {
        return 'Users_'.date('YmdHis');
    }
}
```

## Create Editor Class to handle CRUD actions.

```sh
php artisan datatables:editor Users
```

## Register Editor Route

Edit `routes/web.php` and register the store user route.

```php
Route::get('/users', [App\Http\Controllers\UsersController::class, 'index'])->name('users.index');
Route::post('/users', [App\Http\Controllers\UsersController::class, 'store'])->name('users.store');
```

## Update users controller

```php
namespace App\Http\Controllers;

use Illuminate\Http\Request;
use App\DataTables\UsersDataTable;
use App\DataTables\UsersDataTableEditor;

class UsersController extends Controller
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

## See your editor in action.
