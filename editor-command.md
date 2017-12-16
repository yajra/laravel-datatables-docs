# DataTables Editor Command

## Introduction

Artisan is the command-line interface included with Laravel.
It provides a number of helpful commands that can assist you while you build your application.
To view a list of all available Artisan commands, you may use the list command:

```bash
php artisan list
```

## Editor Command

```bash
php artisan datatables:editor {name}
```

## Editor Command Options

`--model` : The name given will be used as the model is singular form.
`--model-namespace` : The namespace of the model to be used.

## Creating a DataTables Editor class

In this example, we will create a DataTable Editor class.

```bash
php artisan datatables:editor Posts
```

This will create an `PostsDataTableEditor` class on `app\DataTables` directory.

```php
namespace App\DataTables;

use Illuminate\Database\Eloquent\Model;
use Illuminate\Validation\Rule;
use Yajra\DataTables\DataTablesEditor;
use App\User;

class PostsDataTableEditor extends DataTablesEditor
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
            'email' => 'required|email',
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
}
```

### Model Option

In this example, we will pass a `--model` option to set the model to be used by our DataTable.

```bash
php artisan datatables:editor Posts --model
```

This will generate a `App\DataTables\PostsDataTable` class that uses `App\Post` as the base model for our query.
The exported filename will also be set to `posts_(timestamp)`.

```php
namespace App\DataTables;

use Illuminate\Database\Eloquent\Model;
use Illuminate\Validation\Rule;
use Yajra\DataTables\DataTablesEditor;
use App\Post;

class PostsDataTableEditor extends DataTablesEditor
{
    protected $model = Post::class;

    /**
     * Get create action validation rules.
     *
     * @return array
     */
    public function createRules()
    {
        return [
            'email' => 'required|email',
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
}
```

### Model Namespace Option

In this example, we will pass a `--model-namespace` option to set the model namespace to be used by our DataTable.

```bash
php artisan datatables:editor Posts --model-namespace="Entities"
```

It will implicitly activate `--model` option and override the `model` parameter in `datatables-buttons` config file.
This will allow to use a non-standard namespace if front-end and back-end models are in separate namespace for example.
