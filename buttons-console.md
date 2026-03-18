# DataTable Console Commands

<a name="introduction"></a>
## Introduction

Artisan is the command-line interface included with Laravel.
It provides a number of helpful commands that can assist you while you build your application.
To view a list of all available Artisan commands, you may use the list command:

```bash
php artisan list
```

## Creating a DataTable service class

In this example, we will create a DataTable service class.

```bash
php artisan datatables:make Posts
```

This will create a `PostsDataTable` class in the `app\DataTables` directory.

<a name="dataTable-class-example"></a>
### DataTable Class Example

```php
namespace App\DataTables;

use App\User;
use Yajra\DataTables\Html\Column;
use Yajra\DataTables\Services\DataTable;

class PostsDataTable extends DataTable
{
    public function dataTable()
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
            Column::make('id'),
            Column::make('created_at'),
            Column::make('updated_at'),
        ];
    }

    protected function filename(): string
    {
        return 'posts_' . time();
    }
}
```

<a name="model-option"></a>
### Model Option

In this example, we will pass a `--model` option to set the model to be used by our DataTable.

```bash
php artisan datatables:make Posts --model
```

This will generate an `App\DataTables\PostsDataTable` class that uses `App\Post` as the base model for our query.
The exported filename will also be set to `posts_(timestamp)`.

<a name="dataTable-class-with-model"></a>
### DataTable Class With Model

```php
namespace App\DataTables;

use App\Post;
use Yajra\DataTables\Html\Column;
use Yajra\DataTables\Services\DataTable;

class PostsDataTable extends DataTable
{
    public function dataTable()
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
            Column::make('id'),
            Column::make('created_at'),
            Column::make('updated_at'),
        ];
    }

    protected function filename(): string
    {
        return 'posts_' . time();
    }
}
```

<a name="model-namespace-option"></a>
### Model Namespace Option

In this example, we will pass a `--model-namespace` option to set the model namespace to be used by our DataTable.

```bash
php artisan datatables:make Posts --model-namespace="Models\Client"
```
It will implicitly activate `--model` option and override the `model` parameter in `datatables-buttons` config file.
This will allow to use a non-standard namespace if front-end and back-end models are in separate namespace for example.

<a name="action-option"></a>
### Action Option

In this example, we will use the `--action` option to set a custom path for the action column view.

```bash
php artisan datatables:make Posts --action="client.action"
```
If no path is provided, a default path will be used. It will need to be changed thereafter.

<a name="columns-option"></a>
### Columns Option

In this example, we will pass a `--columns` option to set the columns to be used by our DataTable.

```bash
php artisan datatables:make Posts --columns="id,title,author"
```
If not provided, a default set of columns will be used. It will need to be manually changed thereafter.

<a name="creating-a-scope"></a>
## Creating a DataTable Scope

DataTable scope is a class that we can use to limit our database search results based on the defined query scopes.

```bash
php artisan datatables:scope ActiveUser
```

This will create an `ActiveUser` class on `app\DataTables\Scopes` directory.

```php
namespace App\DataTables\Scopes;

use Yajra\DataTables\Contracts\DataTableScopeContract;

class ActiveUser implements DataTableScopeContract
{
    /**
     * Apply a query scope.
     *
     * @param \Illuminate\Database\Query\Builder|\Illuminate\Database\Eloquent\Builder $query
     * @return mixed
     */
     public function apply($query)
     {
          return $query->where('active', true);
     }
}
```

<a name="see-also"></a>
## See Also

- [Buttons Starter](buttons-starter.md) - Quick start guide
- [Buttons Export](buttons-export.md) - Export button options
- [Buttons Custom](buttons-custom.md) - Custom button actions
