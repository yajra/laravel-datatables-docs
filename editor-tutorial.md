# Creating a Laravel Full CRUD with DataTables Editor.

Before we begin, please be reminded that the Editor library that we are going to use here requires a paid license.
See [DataTables Editor](https://editor.datatables.net/purchase/index) for details.

## Pre-requisites

This tutorial requires https://yajrabox.com/docs/laravel-datatables/master/quick-starter.

## Install DataTables Editor assets.

    yarn add datatables.net-editor datatables.net-editor-bs4

## Editor License

Copy and rename your `Editor.XX.zip` to `Editor.zip` and move it to project folder.

## Register postinstall script to package.json

    "scripts": {
        "postinstall": "node ./node_modules/datatables.net-editor/install.js ./Editor.zip",
        "dev": "npm run development",
        .....
    },

## Register editor script on `resources/js/bootstrap.js`

    try {
        window.Popper = require('popper.js').default;
        window.$ = window.jQuery = require('jquery');

        require('bootstrap');
        require('datatables.net-bs4');
        require('datatables.net-buttons-bs4');
        require('datatables.net-editor-bs4');
    } catch (e) {}


## Recompile assets.

    yarn
    yarn watch / dev / prod


## Update UsersDataTable and register the Editor buttons.

> Note: CREATE button is in conflict with `buttons.server-side.js`. You need to remove the create button or rename it to something else like `add` button.

    DataTable.ext.buttons.add = {
        className: 'buttons-add',

        text: function (dt) {
            return '<i class="fa fa-plus"></i> ' + dt.i18n('buttons.add', 'Create');
        },

        action: function (e, dt, button, config) {
            window.location = window.location.href.replace(/\/+$/, "") + '/create';
        }
    };

### UsersDataTable.php

Create a new editor instance and add some fields for name and email.

```php
use Yajra\DataTables\Html\Editor\Editor;
use Yajra\DataTables\Html\Editor\Fields;

...
    public function html()
    {
        return $this->builder()
                    ->setTableId('users-table')
                    ->columns($this->getColumns())
                    ->minifiedAjax()
                    ->dom('Bfrtip')
                    ->orderBy(1)
                    ->buttons(
                        Button::make('create')->editor('editor'),
                        Button::make('edit')->editor('editor'),
                        Button::make('remove')->editor('editor'),
                        Button::make('export'),
                        Button::make('print'),
                        Button::make('reset'),
                        Button::make('reload')
                    )
                    ->editor(
                        Editor::make()
                            ->fields([
                                Fields\Text::make('name'),
                                Fields\Text::make('email'),
                                Fields\Password::make('password'),
                            ])
                    );
    }
```

## Create Editor Class to handle CRUD actions.

```
php artisan datatables:editor Users
```

## Register Editor Route

Edit `routes/web.php` and register the store user route.

```
Route::post('/users', 'UsersController@store')->name('users.store');
```

## Update users controller

```
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
