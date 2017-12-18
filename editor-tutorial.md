# Creating a Laravel Full CRUD with DataTables Editor.

Before we begin, please be reminded that the Editor library that we are going to use here requires a paid license.
See [DataTables Editor](https://editor.datatables.net/purchase/index) for details.

## Create a new laravel app

Using [laravel installer](https://laravel.com/docs/5.5/installation), run the command below using terminal.

```bash
laravel new editor
```

Once installed, lets configure our application and use `sqlite` as our database by updating `DB_CONNECTION=sqlite`.
We also need to remove the following lines as it is not needed for sqlite.

```php
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=homestead
DB_USERNAME=homestead
DB_PASSWORD=secret
```

Next, create our database and run the migration. We will then seed some users using tinker.

```bash
cd editor
touch database/database.sqlite
php artisan migrate
php artisan tinker
>>> factory(App\User::class, 20)->create();
```

## Install DataTables Editor library.

In this step, we will only need to install `yajra/laravel-datatables-editor`.

```bash
composer require yajra/laravel-datatables-editor
```

## Create Controllers, DataTables and Editor classes.

```php
php artisan make:controller UsersController
php artisan datatables:make Users
php artisan datatables:editor Users
```

These commands will generate the following files:
- app\Http\Controllers\UsersController.php
- app\DataTables\UsersDataTable.php
- app\DataTables\UsersDataTablesEditor.php

## Setup Controller

We will only need `index` and `store` method for our controller. But before that, lets register our route first on `routes\web.php`

```php
Route::resource('users', 'UsersController');
```

Will now add the methods on our controller.

```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use App\DataTables\UsersDataTable;
use App\DataTables\UsersDataTablesEditor;

class UsersController extends Controller
{
    public function index(UsersDataTable $dataTable)
    {
        return $dataTable->render('users.index');
    }

    public function store(UsersDataTablesEditor $editor)
    {
        return $editor->process(request());
    }
}
```

## Setup DataTable

```php
<?php

namespace App\DataTables;

use App\User;
use Yajra\DataTables\Services\DataTable;

class UsersDataTable extends DataTable
{
    /**
     * Build DataTable class.
     *
     * @param mixed $query Results from query() method.
     * @return \Yajra\DataTables\DataTableAbstract
     */
    public function dataTable($query)
    {
        return datatables($query)->setRowId('id')->addColumn('password', '');
    }

    /**
     * Get query source of dataTable.
     *
     * @param \App\User $model
     * @return \Illuminate\Database\Eloquent\Builder
     */
    public function query(User $model)
    {
        return $model->newQuery()->select('id', 'name', 'email');
    }

    /**
     * Optional method if you want to use html builder.
     *
     * @return \Yajra\DataTables\Html\Builder
     */
    public function html()
    {
        return $this->builder()
                    ->columns($this->getColumns())
                    ->minifiedAjax()
                    ->parameters([
                        'dom' => 'Bfrtip',
                        'order' => [1, 'asc'],
                        'select' => [
                            'style' => 'os',
                            'selector' => 'td:first-child',
                        ],
                        'buttons' => [
                            ['extend' => 'create', 'editor' => 'editor'],
                            ['extend' => 'edit', 'editor' => 'editor'],
                            ['extend' => 'remove', 'editor' => 'editor'],
                        ]
                    ]);
    }

    /**
     * Get columns.
     *
     * @return array
     */
    protected function getColumns()
    {
        return [
            [
                'data' => null,
                'defaultContent' => '',
                'className' => 'select-checkbox',
                'title' => '',
                'orderable' => false,
                'searchable' => false
            ],
            'id',
            'name',
            'email',
        ];
    }

    /**
     * Get filename for export.
     *
     * @return string
     */
    protected function filename()
    {
        return 'users_' . time();
    }
}
```

## Setup Editor

```php
<?php

namespace App\DataTables;

use App\User;
use Illuminate\Validation\Rule;
use Yajra\DataTables\DataTablesEditor;
use Illuminate\Database\Eloquent\Model;

class UsersDataTablesEditor extends DataTablesEditor
{
    protected $model = User::class;

    /**
     * Get create action validation rules.
     *
     * @return array
     */
    public function createRules()
    {
        return [
            'email' => 'required|email|unique:users',
            'name'  => 'required',
        ];
    }

    /**
     * Get edit action validation rules.
     *
     * @param Model $model
     * @return array
     */
    public function editRules(Model $model)
    {
        return [
            'email' => 'sometimes|required|email|' . Rule::unique($model->getTable())->ignore($model->getKey()),
            'name'  => 'sometimes|required',
        ];
    }

    /**
     * Get remove action validation rules.
     *
     * @param Model $model
     * @return array
     */
    public function removeRules(Model $model)
    {
        return [];
    }

    public function creating(Model $model, array $data)
    {
        $data['password'] = bcrypt($data['password']);

        return $data;
    }

    public function updating(Model $model, array $data)
    {
        if (empty($data['password'])) {
            unset($data['password']);
        } else {
            $data['password'] = bcrypt($data['password']);
        }

        return $data;
    }
}
```

## Setup View

Lets create our users admin panel view at `resources/views/users/index.blade.php`.

```php
<!doctype html>
<html lang="{{ app()->getLocale() }}">
    <head>
        <meta charset="utf-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1">

        <title>Laravel DataTables Editor</title>

        <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/twitter-bootstrap/3.3.7/css/bootstrap.min.css">
        <link rel="stylesheet" href="https://cdn.datatables.net/1.10.16/css/dataTables.bootstrap.min.css">
        <link rel="stylesheet" href="https://cdn.datatables.net/buttons/1.5.0/css/buttons.bootstrap.min.css">
        <link rel="stylesheet" href="https://cdn.datatables.net/select/1.2.4/css/select.bootstrap.min.css">
        <link rel="stylesheet" href="/plugins/editor/css/dataTables.editor.css">
        <link rel="stylesheet" href="/plugins/editor/css/editor.bootstrap.css">

        <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.2.1/jquery.min.js"></script>
        <script src="https://cdnjs.cloudflare.com/ajax/libs/twitter-bootstrap/3.3.7/js/bootstrap.min.js"></script>
        <script src="https://cdn.datatables.net/1.10.16/js/jquery.dataTables.min.js"></script>
        <script src="https://cdn.datatables.net/buttons/1.5.0/js/dataTables.buttons.min.js"></script>
        <script src="https://cdn.datatables.net/select/1.2.4/js/dataTables.select.min.js"></script>
        <script src="{{asset('plugins/editor/js/dataTables.editor.js')}}"></script>

        <script src="https://cdn.datatables.net/1.10.16/js/dataTables.bootstrap.min.js"></script>
        <script src="https://cdn.datatables.net/buttons/1.5.0/js/buttons.bootstrap.min.js"></script>

        <script src="{{asset('plugins/editor/js/editor.bootstrap.min.js')}}"></script>
    </head>
    <body>
        <div class="container">
            {{$dataTable->table(['id' => 'users'])}}
        </div>

        <script>
            $(function() {
                $.ajaxSetup({
                    headers: {
                        'X-CSRF-TOKEN': '{{csrf_token()}}'
                    }
                });

                var editor = new $.fn.dataTable.Editor({
                    ajax: "/users",
                    table: "#users",
                    display: "bootstrap",
                    fields: [
                        {label: "Name:", name: "name"},
                        {label: "Email:", name: "email"},
                        {label: "Password:", name: "password", type: "password"}
                    ]
                });

                $('#users').on('click', 'tbody td:not(:first-child)', function (e) {
                    editor.inline(this);
                });

                {{$dataTable->generateScripts()}}
            })
        </script>
    </body>
</html>
```

## Register and Download your trial Editor library

You can download the library assets [here](https://editor.datatables.net/download/) once you successfully registered to DataTables.
We would only need the JS and CSS so I suggest you only download those files.
Once downloaded, copy the library to `public/plugins/editor` of your laravel public directory.

## Run your application

Using valet, visit your site using `http://editor.dev/users` and see the full CRUD in action.

## Gist

See this [gist](https://gist.github.com/yajra/38ee8d2521fa419309dfabcb667d9c2a) for the complete source code of added files.
