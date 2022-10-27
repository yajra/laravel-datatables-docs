# Quick Starter

<a name="installation"></a>
## <b>01.</b> Installing Laravel & DataTables

### Quick Installation

If you have already installed [Laravel Installer](https://laravel.com/docs#your-first-laravel-project) on your local machine, you may create a new project via laravel command:

```shell
laravel new datatables
```

After the project has been created, we will then install [Laravel UI](https://github.com/laravel/ui) and [Yajra DataTables](https://github.com/yajra/laravel-datatables)

```shell
cd datatables
 
composer require laravel/ui --dev
php artisan ui bootstrap --auth

composer require yajra/laravel-datatables:^9.0
```

For simplicity, you may use SQLite to store your application's data. To instruct Laravel to use SQLite instead of MySQL, update your new application's `.env` file and remove all of the `DB_*` environment variables except for the `DB_CONNECTION` variable, which should be set to `sqlite`:

```shell
touch database/database.sqlite
```

```dotenv filename=.env
DB_CONNECTION=sqlite
```

<a name="datatables-with-vite"></a>
## <b>02.</b> Install Laravel DataTables Vite

Next, we will install [Laravel DataTables Vite](https://github.com/yajra/laravel-datatables-vite) to simplify our frontend setup.

```shell
npm i laravel-datatables-vite --save-dev
```

This will install the following packages:

```
  - Bootstrap Icons
  - DataTables with Buttons and Select plugins for Bootstrap 5
  - Laravel DataTables custom scripts
```

Once installed, we can now configure our scripts and css needed for our application.

```js filename=resources/js/app.js
import './bootstrap';
import 'laravel-datatables-vite';
```

```postcss filename=resources/sass/app.scss
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

We just need to start the Vite development server to automatically recompile our JS, CSS and refresh the browser when we make changes to our Blade templates:

```shell
npm run dev
```

<a name="setup-users-datatable"></a>
## <b>03.</b> Setup a Users DataTable

Open a new terminal in your `datatables` project directory and run the following command:

```shell
php artisan datatables:make Users
```

Next, we will configure our `UsersDataTable` and add the columns that we want to display.

```php filename=app/DataTables/UsersDataTable.php
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
        return 'Users_'.date('YmdHis');
    }
}
```

<a name="setup-users-controller"></a>
## <b>04.</b> Setup a Users Controller, View & Route

```shell
php artisan make:controller UsersController
```

```php filename=app/Http/Controllers/UsersController.php
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

```blade filename=resources/views/users/index.blade.php
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

```php filename=routes/web.php
use App\Http\Controllers\UsersController;

Route::get('/users', [UsersController::class, 'index'])->name('users.index');
```

<a name="update-default-layout"></a>
## <b>05.</b> Update the default app layout

To be able to load our custom scripts, we need to add `@stack('scripts')` before the end of `body` tag in our `app.blade.php` layout.

```blade filename=resources/views/layouts/app.blade.php
....
        <main class="py-4">
            @yield('content')
        </main>
    </div>
    @stack('scripts')
</body>
</html>
```

<a name="migrate-and-seed"></a>
## <b>06.</b> Migrate and Seed Test Data

```shell
php artisan migrate
php artisan tinker
```

```php
Psy Shell v0.9.9 (PHP 7.2.22 — cli) by Justin Hileman
>>> User::factory(100)->create()
```

Our application should now be ready to run.

```shell
php artisan serve
```

Once you have started the Artisan development server, your application will be accessible in your web browser at [http://localhost:8000]([http://localhost:8000).

We can now visit our [`/users`](http://localhost:8000/users) route and see our users table.

<img src="/img/screenshots/quick-starter.png" alt="Laravel DataTables Users" class="rounded-lg border dark:border-none shadow-lg" />
