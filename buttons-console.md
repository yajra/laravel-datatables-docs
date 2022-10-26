# DataTable Console Commands

## Introduction

Artisan is the command-line interface included with Laravel.
It provides a number of helpful commands that can assist you while you build your application.
To view a list of all available Artisan commands, you may use the list command:

```
php artisan list
```

## Creating a DataTable service class

In this example, we will create a DataTable service class.

```
php artisan datatables:make Posts
```

This will create a `PostsDataTable` class in the `app\DataTables` directory.

```php
namespace App\DataTables;

use App\User;
use Yajra\DataTables\Services\DataTable;

class PostsDataTable extends DataTable
{
    /**
     * Build DataTable class.
     *
     * @return \Yajra\DataTables\DataTableAbstract
     */
    public function dataTable()
    {
        return $this->datatables
            ->eloquent($this->query())
            ->addColumn('action', 'path.to.action.view');
    }

    /**
     * Get the query object to be processed by dataTables.
     *
     * @return \Illuminate\Database\Eloquent\Builder|\Illuminate\Database\Query\Builder|\Illuminate\Support\Collection
     */
    public function query()
    {
        $query = User::query();

        return $this->applyScopes($query);
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
                    ->ajax('')
                    ->addAction(['width' => '80px'])
                    ->parameters($this->getBuilderParameters());
    }

    /**
     * Get columns.
     *
     * @return array
     */
    protected function getColumns()
    {
        return [
            'id',
            // add your columns
            'created_at',
            'updated_at',
        ];
    }

    /**
     * Get filename for export.
     *
     * @return string
     */
    protected function filename()
    {
        return 'posts_' . time();
    }
}
```

### Model Option

In this example, we will pass a `--model` option to set the model to be used by our DataTable.

```
php artisan datatables:make Posts --model
```

This will generate an `App\DataTables\PostsDataTable` class that uses `App\Post` as the base model for our query. 
The exported filename will also be set to `posts_(timestamp)`.

```php
<?php

namespace App\DataTables;

use App\Post;
use Yajra\DataTables\Services\DataTable;

class PostsDataTable extends DataTable
{
    /**
     * Build DataTable class.
     *
     * @return \Yajra\DataTables\DataTableAbstract
     */
    public function dataTable()
    {
        return $this->datatables
            ->eloquent($this->query())
            ->addColumn('action', 'path.to.action.view');
    }

    /**
     * Get the query object to be processed by dataTables.
     *
     * @return \Illuminate\Database\Eloquent\Builder|\Illuminate\Database\Query\Builder|\Illuminate\Support\Collection
     */
    public function query()
    {
        $query = Post::query();

        return $this->applyScopes($query);
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
                    ->ajax('')
                    ->addAction(['width' => '80px'])
                    ->parameters($this->getBuilderParameters());
    }

    /**
     * Get columns.
     *
     * @return array
     */
    protected function getColumns()
    {
        return [
            'id',
            // add your columns
            'created_at',
            'updated_at',
        ];
    }

    /**
     * Get filename for export.
     *
     * @return string
     */
    protected function filename()
    {
        return 'posts_' . time();
    }
}
```


### Model Namespace Option

In this example, we will pass a `--model-namespace` option to set the model namespace to be used by our DataTable.

```
php artisan datatables:make Posts --model-namespace="Models\Client"
```
It will implicitly activate `--model` option and override the `model` parameter in `datatables-buttons` config file.
This will allow to use a non-standard namespace if front-end and back-end models are in separate namespace for example. 



### Action Option

In this example, we will use the `--action` option to set a custom path for the action column view.

```
php artisan datatables:make Posts --action="client.action"
```
If no path is provided, a default path will be used. It will need to be changed thereafter.

### Columns Option

In this example, we will pass a `--columns` option to set the columns to be used by our DataTable.

```
php artisan datatables:make Posts --columns="id,title,author"
```
If not provided, a default set of columns will be used. It will need to be manually changed thereafter.



## Creating a DataTable Scope service class

DataTable scope is a class that we can use to limit our database search results based on the defined query scopes.

```
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
