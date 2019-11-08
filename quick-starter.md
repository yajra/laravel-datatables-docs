# Laravel DataTables Quick Starter

## Create new project

```
laravel new datatables
cd datatables
cp .env.example .env
php artisan key:generate
```

## Setup database and ENV configuration

Create a new database and update `.env` file and set the datbase credentials.

## Install package and publish assets

```
composer require yajra/laravel-datatables
php artisan vendor:publish --tag=datatables-buttons

```

## Setup Laravel UI

```
composer require laravel/ui --dev
php artisan ui bootstrap --auth
```

## Install Datatables.net assets

```
yarn add datatables.net-bs4 datatables.net-buttons-bs4
```

## Register datatables.net assets in bootstrap.js

Edit `resources/js/bootstrap.js` and add the following:

    require('bootstrap');
    require('datatables.net-bs4');
    require('datatables.net-buttons-bs4');

## Compile assets

```
yarn dev / watch / prod
```

## Create controller and DataTable class

```
php artisan make:controller UsersController
php artisan datatables:make Users
```

### UsersController

```php
namespace App\Http\Controllers;

use App\DataTables\UsersDataTable;

class UsersController extends Controller
{
    public function index(UsersDataTable $dataTable)
    {
        return $dataTable->render('users.index');
    }
}
```

### UsersDataTable

```php
namespace App\DataTables;

use App\User;
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
     * @param mixed $query Results from query() method.
     * @return \Yajra\DataTables\DataTableAbstract
     */
    public function dataTable($query)
    {
        return datatables()
            ->eloquent($query)
            ->addColumn('action', 'users.action');
    }

    /**
     * Get query source of dataTable.
     *
     * @param \App\User $model
     * @return \Illuminate\Database\Eloquent\Builder
     */
    public function query(User $model)
    {
        return $model->newQuery();
    }

    /**
     * Optional method if you want to use html builder.
     *
     * @return \Yajra\DataTables\Html\Builder
     */
    public function html()
    {
        return $this->builder()
                    ->setTableId('users-table')
                    ->columns($this->getColumns())
                    ->minifiedAjax()
                    ->dom('Bfrtip')
                    ->orderBy(1)
                    ->buttons(
                        Button::make('create'),
                        Button::make('export'),
                        Button::make('print'),
                        Button::make('reset'),
                        Button::make('reload')
                    );
    }

    /**
     * Get columns.
     *
     * @return array
     */
    protected function getColumns()
    {
        return [
            Column::computed('action')
                  ->exportable(false)
                  ->printable(false)
                  ->width(60)
                  ->addClass('text-center'),
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
    protected function filename()
    {
        return 'Users_' . date('YmdHis');
    }
}
```

## Update app layout

Add scripts before the body end tag of `resources/views/layouts/app.blade.php`

```
    <script src="{{ mix('js/app.js') }}"></script>
    <script src="{{ asset('vendor/datatables/buttons.server-side.js') }}"></script>
    @stack('scripts')
```

## Create users index file

Create new file: `resources/views/users/index.blade.php`.

```php
@extends('layouts.app')

@section('content')
    {{$dataTable->table()}}
@endsection

@push('scripts')
    {{$dataTable->scripts()}}
@endpush
```

## Register users route

Update `routes/web.php`.

```php
Route::get('/users', 'UsersController@index')->name('users.index');
```

## Create dummy data using tinker

```php
php artisan tinker

Psy Shell v0.9.9 (PHP 7.2.22 — cli) by Justin Hileman
>>> factory('App\User', 100)->create()
```

## Access Users DataTables

http://datatables.test/users

