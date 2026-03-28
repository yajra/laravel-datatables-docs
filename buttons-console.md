---
title: DataTables Console
description: Artisan commands for generating DataTables classes
---

# DataTable Console Commands

Artisan is the command-line interface included with Laravel. It provides a number of helpful commands that can assist you while you build your application.

---

<a name="list"></a>
## List Available Commands

```bash
php artisan list
```

---

<a name="create"></a>
## Creating a DataTable Service Class

Create a DataTable service class:

```bash
php artisan datatables:make Posts
```

This will create a `PostsDataTable` class in the `app/DataTables` directory.

---

<a name="example"></a>
## DataTable Class Example

```php
<?php
// app/DataTables/PostsDataTable.php

namespace App\DataTables;

use App\Models\User;
use Yajra\DataTables\Html\Column;
use Yajra\DataTables\Services\DataTable;

class PostsDataTable extends DataTable
{
    public function dataTable($query)
    {
        return $this->datatables
            ->eloquent($this->query())
            ->addColumn('action', 'path.to.action.view');
    }

    public function query()
    {
        $query = User::query();

        return $this->applyScopes($query);
    }

    public function html()
    {
        return $this->builder()
                    ->columns($this->getColumns())
                    ->ajax('')
                    ->addAction(['width' => '80px'])
                    ->layout([
                        'topStart' => 'buttons',
                        'topEnd' => 'search',
                    ]);
    }

    protected function getColumns(): array
    {
        return [
            Column::make('created_at'),
            Column::make('updated_at'),
            Column::make('id'),
        ];
    }

    protected function filename(): string
    {
        return 'posts_' . time();
    }
}
```

---

## Model Option

Pass a `--model` option to set the model to be used by our DataTable:

```bash
php artisan datatables:make Posts --model
```

This will generate an `App\DataTables\PostsDataTable` class that uses `App\Post` as the base model. The exported filename will also be set to `posts_(timestamp)`.

---

## DataTable Class With Model

```php
<?php
// app/DataTables/PostsDataTable.php

namespace App\DataTables;

use App\Models\Post;
use Yajra\DataTables\Html\Column;
use Yajra\DataTables\Services\DataTable;

class PostsDataTable extends DataTable
{
    public function dataTable($query)
    {
        return $this->datatables
            ->eloquent($this->query())
            ->addColumn('action', 'path.to.action.view');
    }

    public function query()
    {
        $query = Post::query();

        return $this->applyScopes($query);
    }

    public function html()
    {
        return $this->builder()
                    ->columns($this->getColumns())
                    ->ajax('')
                    ->addAction(['width' => '80px'])
                    ->layout([
                        'topStart' => 'buttons',
                        'topEnd' => 'search',
                    ]);
    }

    protected function getColumns(): array
    {
        return [
            Column::make('created_at'),
            Column::make('updated_at'),
            Column::make('id'),
        ];
    }

    protected function filename(): string
    {
        return 'posts_' . time();
    }
}
```

---

## Model Namespace Option

```bash
php artisan datatables:make Posts --model-namespace="Models\Client"
```

This implicitly activates the `--model` option and allows you to use a non-standard namespace.

---

## Action Option

```bash
php artisan datatables:make Posts --action="client.action"
```

Sets a custom path for the action column view.

---

## Columns Option

```bash
php artisan datatables:make Posts --columns="id,title,author"
```

Sets the columns to be used by our DataTable.

---

<a name="creating-a-datatable-scope"></a>
## Creating a DataTable Scope

DataTable scope is a class that we can use to limit our database search results based on the defined query scopes:

```bash
php artisan datatables:scope ActiveUser
```

This will create an `ActiveUser` class in `app/DataTables/Scopes` directory:

```php
<?php
// app/DataTables/Scopes/ActiveUser.php

namespace App\DataTables\Scopes;

use Yajra\DataTables\Contracts\DataTableScopeContract;

class ActiveUser implements DataTableScopeContract
{
    /**
     * Apply a query scope.
     */
    public function apply($query)
    {
        return $query->where('active', true);
    }
}
```

---

<a name="using-scopes"></a>
## Using Scopes in DataTable

Register the scope in your DataTable class using the `addScope()` method:

```php
<?php
// app/DataTables/UsersDataTable.php

namespace App\DataTables;

use App\DataTables\Scopes\ActiveUser;
use Yajra\DataTables\Services\DataTable;

class UsersDataTable extends DataTable
{
    public function __construct()
    {
        parent::__construct();
        $this->addScope(new ActiveUser);
    }
}
```

You can also add multiple scopes using `addScopes()`:

```php
use App\DataTables\Scopes\ActiveUser;
use App\DataTables\Scopes\VerifiedUser;

$this->addScopes([
    new ActiveUser,
    new VerifiedUser,
]);
```

---

## See Also

- [Buttons Starter](/docs/{{package}}/{{version}}/buttons-starter) - Quick start guide
- [Buttons Export](/docs/{{package}}/{{version}}/buttons-export) - Export button options
- [Buttons Custom](/docs/{{package}}/{{version}}/buttons-custom) - Custom button actions
