---
title: Quick Starter
description: Build your first DataTable in 15 minutes with this complete guide
---

# Quick Starter

This guide will walk you through building a complete DataTable in 15 minutes.

---

<a name="step-1"></a>
## Step 1: Installing Laravel & DataTables

### Quick Installation

If you have already installed [Laravel Installer](https://laravel.com/docs#your-first-laravel-project) on your local machine, you may create a new project via Laravel command:

```bash
laravel new datatables
```

After the project has been created, we will then install [Laravel UI](https://github.com/laravel/ui) and [Yajra DataTables](https://github.com/yajra/laravel-datatables):

```bash
cd datatables

composer require laravel/ui --dev
php artisan ui bootstrap --auth

composer require yajra/laravel-datatables
```

> **Note**: Laravel UI is compatible with Laravel 10 and below. For Laravel 11+, consider using [Laravel Breeze](https://laravel.com/docs/starter-kits#breeze) instead.

For simplicity, you may use SQLite to store your application's data. To instruct Laravel to use SQLite instead of MySQL, update your new application's `.env` file and remove all of the `DB_*` environment variables except for the `DB_CONNECTION` variable, which should be set to `sqlite`:

```bash
touch database/database.sqlite
```

```env filename=.env
DB_CONNECTION=sqlite
```

---

<a name="step-2"></a>
## Step 2: Install Laravel DataTables Vite

Next, we will install [Laravel DataTables Vite](https://github.com/yajra/laravel-datatables-vite) to simplify our frontend setup:

```bash
npm i laravel-datatables-vite --save-dev
```

This will install the following packages:

```
- Bootstrap Icons
- DataTables with Buttons and Select plugins for Bootstrap 5
- Laravel DataTables custom scripts
```

Once installed, we can now configure our scripts and CSS needed for our application:

```js filename=resources/js/app.js
import './bootstrap';
import 'laravel-datatables-vite';
```

```css filename=resources/css/app.css
/* Fonts */
@import url('https://fonts.bunny.net/css?family=Nunito');

/* DataTables */
@import 'bootstrap-icons/font/bootstrap-icons.css';
@import "datatables.net-bs5/css/dataTables.bootstrap5.min.css";
@import "datatables.net-buttons-bs5/css/buttons.bootstrap5.min.css";
@import 'datatables.net-select-bs5/css/select.bootstrap5.css';
```

> **Note**: Laravel 11+ uses Vite with CSS by default. If you prefer SCSS, install the `sass` package: `npm i -D sass`

We just need to start the Vite development server to automatically recompile our JS, CSS and refresh the browser when we make changes to our Blade templates:

```bash
npm run dev
```

---

<a name="step-3"></a>
## Step 3: Setup a Users DataTable

Open a new terminal in your `datatables` project directory and run the following command:

```bash
php artisan datatables:make Users
```

Next, we will configure our `UsersDataTable` and add the columns that we want to display:

```php filename=app/DataTables/UsersDataTable.php
<?php

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

---

<a name="step-4"></a>
## Step 4: Setup a Users Controller, View & Route

```bash
php artisan make:controller UsersController
```

```php filename=app/Http/Controllers/UsersController.php
<?php

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
<?php

use App\Http\Controllers\UsersController;
use Illuminate\Support\Facades\Route;

Route::get('/users', [UsersController::class, 'index'])->name('users.index');
```

---

<a name="step-5"></a>
## Step 5: Update the Default App Layout

To be able to load our custom scripts, we need to add `@stack('scripts')` before the end of `body` tag in our `app.blade.php` layout:

```blade filename=resources/views/layouts/app.blade.php
<!DOCTYPE html>
<html lang="{{ str_replace('_', '-', app()->getLocale()) }}">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <meta name="csrf-token" content="{{ csrf_token() }}">
    <title>{{ config('app.name', 'Laravel') }}</title>
    <!-- ... other head content ... -->
</head>
<body>
    <div id="app">
        <main class="py-4">
            @yield('content')
        </main>
    </div>
    @stack('scripts')
</body>
</html>
```

---

## Step 6: Migrate and Seed Test Data

```bash
php artisan migrate
php artisan tinker
```

```php
>>> User::factory(100)->create()
```

Our application should now be ready to run:

```bash
php artisan serve
```

Once you have started the Artisan development server, your application will be accessible in your web browser at [http://localhost:8000](http://localhost:8000).

We can now visit our [`/users`](http://localhost:8000/users) route and see our users table.

<img src="https://yajrabox.com/img/screenshots/quick-starter.png" alt="Laravel DataTables Users" class="rounded-lg border dark:border-none shadow-lg mb-6" />

---

## See Also

- [HTML Builder](/docs/{{package}}/{{version}}/html-builder) - More advanced table configuration
- [Buttons Plugin](/docs/{{package}}/{{version}}/buttons-installation) - Add export functionality
- [Eloquent Data Source](/docs/{{package}}/{{version}}/engine-eloquent) - Working with Eloquent models
