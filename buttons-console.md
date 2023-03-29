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

use App\Models\Post;
use Illuminate\Database\Eloquent\Builder as QueryBuilder;
use Yajra\DataTables\EloquentDataTable;
use Yajra\DataTables\Html\Builder as HtmlBuilder;
use Yajra\DataTables\Html\Button;
use Yajra\DataTables\Html\Column;
use Yajra\DataTables\Html\Editor\Editor;
use Yajra\DataTables\Html\Editor\Fields;
use Yajra\DataTables\Services\DataTable;

class PostsDataTable extends DataTable
{
    /**
     * Build the DataTable class.
     *
     * @param QueryBuilder $query Results from query() method.
     */
    public function dataTable(QueryBuilder $query): EloquentDataTable
    {
        return (new EloquentDataTable($query))
            ->addColumn('action', 'posts.action')
            ->setRowId('id');
    }

    /**
     * Get the query source of dataTable.
     */
    public function query(Post $model): QueryBuilder
    {
        return $model->newQuery();
    }

    /**
     * Optional method if you want to use the html builder.
     */
    public function html(): HtmlBuilder
    {
        return $this->builder()
                    ->setTableId('posts-table')
                    ->columns($this->getColumns())
                    ->minifiedAjax()
                    //->dom('Bfrtip')
                    ->orderBy(1)
                    ->selectStyleSingle()
                    ->buttons([
                        Button::make('excel'),
                        Button::make('csv'),
                        Button::make('pdf'),
                        Button::make('print'),
                        Button::make('reset'),
                        Button::make('reload')
                    ]);
    }

    /**
     * Get the dataTable columns definition.
     */
    public function getColumns(): array
    {
        return [
            Column::computed('action')
                  ->exportable(false)
                  ->printable(false)
                  ->width(60)
                  ->addClass('text-center'),
            Column::make('id'),
            Column::make('add your columns'),
            Column::make('created_at'),
            Column::make('updated_at'),
        ];
    }

    /**
     * Get the filename for export.
     */
    protected function filename(): string
    {
        return 'Posts_' . date('YmdHis');
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
