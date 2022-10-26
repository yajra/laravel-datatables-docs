## DataTables Quick Starter

### Create a new Laravel project

```
laravel new datatables
cd datatables
```

### Setup Laravel UI

```shell
composer require laravel/ui --dev
php artisan ui bootstrap --auth
```

### Install Laravel DataTables

```shell
composer require yajra/laravel-datatables:^9.0
```

### Setup database and ENV configuration

Create a new database and update `.env` file and set the database credentials.

```shell
touch database/database.sqlite
```

```dotenv
DB_CONNECTION=sqlite
DB_DATABASE=/absolute/path/to/database/database.sqlite
```

```shell
php artisan migrate
```

### Install Laravel DataTables Vite Assets

```shell
npm i laravel-datatables-vite --save-dev
```

This will install the following packages:

1. Bootstrap Icons
2. DataTables with Buttons and Select plugins for Bootstrap 5
3. Laravel DataTables custom scripts

### Register the package js and css

Edit `resources/js/app.js` and add the following:

```js
import './bootstrap';
import 'laravel-datatables-vite';
```

Edit `resources/sass/app.scss` and add the following:

```postcss
// Fonts
@import url('https://fonts.bunny.net/css?family=Nunito');

// Variables
@import 'variables';

// Bootstrap
@import 'bootstrap/scss/bootstrap';

// DataTables
@import 'bootstrap-icons/font/bootstrap-icons.css';
@import "datatables.net-bs5/css/dataTables.bootstrap5.min.css";
@import "datatables.net-buttons-bs5/css/buttons.bootstrap5.min.css";
@import 'datatables.net-select-bs5/css/select.bootstrap5.css';
```

### Compile the assets

```
npm run dev
```

### Create and update UsersDataTable

Create a new DataTable class:

```shell
php artisan datatables:make Users
```

Then, update the `getColumns()` with the users fields:

```php
namespace App\DataTables;

use App\Models\User;
use Illuminate\Database\Eloquent\Builder as QueryBuilder;
use Yajra\DataTables\EloquentDataTable;
use Yajra\DataTables\Html\Builder as HtmlBuilder;
use Yajra\DataTables\Html\Button;
use Yajra\DataTables\Html\Column;
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
                    ->buttons([
                        Button::make('add'),
                        Button::make('excel'),
                        Button::make('csv'),
                        Button::make('pdf'),
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

    protected function filename(): string
    {
        return 'Users_'.date('YmdHis');
    }
}
```

### Create and update the users controller

Create a new controller and add the following:

```shell
php artisan make:controller UsersController
```

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

### Update the default app layout

Add `@stack('scripts')` before the body end tag of `resources/views/layouts/app.blade.php`

```
....
        <main class="py-4">
            @yield('content')
        </main>
    </div>
    @stack('scripts')
</body>
</html>
```

### Create users index file

Create new file: `resources/views/users/index.blade.php`.

```php
@extends('layouts.app')

@section('content')
    <div class="container">
        <div class="card">
            <div class="card-header">Manage Users</div>
            <div class="card-body">
                {{ $dataTable->table() }}
            </div>
        </div>
    </div>
@endsection

@push('scripts')
    {{ $dataTable->scripts(attributes: ['type' => 'module']) }}
@endpush
```

### Register users route

Update `routes/web.php`.

```php
use App\Http\Controllers\UsersController;

Route::get('/users', [UsersController::class, 'index'])->name('users.index');
```

### Create dummy data using tinker

```php
php artisan tinker

Psy Shell v0.9.9 (PHP 7.2.22 — cli) by Justin Hileman
>>> User::factory(100)->create()
```

### Access Users DataTables

http://datatables.test/users

