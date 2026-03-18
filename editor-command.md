# DataTables Editor Command

<a name="editor-command"></a>

> {info} The `datatables:editor` artisan command generates a complete Editor class with all required methods.

---

<a name="overview"></a>
## Overview

The Editor command creates a new `DataTablesEditor` class with:
- Pre-configured model property
- Validation rules for create, edit, and remove
- Ready for customization

---

<a name="basic-usage"></a>
## Basic Usage

### List All Commands

```bash
php artisan list | grep datatables
```

### Create Editor Class

```bash
php artisan datatables:editor {name}
```

| Argument | Description |
|----------|-------------|
| `name` | The base name for your editor class (e.g., `Users` creates `UsersDataTableEditor`) |

---

<a name="command-options"></a>
## Command Options

| Option | Description |
|--------|-------------|
| `--model` | Use singular form as the model name |
| `--model-namespace="namespace"` | Set custom model namespace |

---

<a name="examples"></a>
## Examples

### Example 1: Basic Editor

```bash
php artisan datatables:editor Posts
```

Creates `app/DataTables/PostsDataTableEditor.php`:

```php
namespace App\DataTables;

use Illuminate\Database\Eloquent\Model;
use Illuminate\Validation\Rule;
use Yajra\DataTables\DataTablesEditor;
use App\User;

/** @extends DataTablesEditor<User> **/
class PostsDataTableEditor extends DataTablesEditor
{
    protected $model = User::class;

    public function createRules(): array
    {
        return [
            'email' => 'required|email',
            'name'  => 'required',
        ];
    }

    public function editRules(Model $model): array
    {
        return [
            'email' => 'sometimes|required|email|' . Rule::unique($model->getTable())->ignore($model->getKey()),
            'name'  => 'sometimes|required',
        ];
    }

    public function removeRules(Model $model): array
    {
        return [
            'DT_RowId' => 'required|exists:' . $model->getTable() . ',id',
        ];
    }
}
```

---

### Example 2: With Model Option

```bash
php artisan datatables:editor Posts --model
```

> {tip} The `--model` option uses the singular form (`Post`) as the model class and includes generics.

Creates `app/DataTables/PostsDataTableEditor.php` with `App\Post` model:

```php
namespace App\DataTables;

use Illuminate\Database\Eloquent\Model;
use Illuminate\Validation\Rule;
use Yajra\DataTables\DataTablesEditor;
use App\Post;

/** @extends DataTablesEditor<Post> **/
class PostsDataTableEditor extends DataTablesEditor
{
    protected $model = Post::class;

    public function createRules(): array
    {
        return [
            'email' => 'required|email',
            'name'  => 'required',
        ];
    }

    public function editRules(Model $model): array
    {
        return [
            'email' => 'sometimes|required|email|' . Rule::unique($model->getTable())->ignore($model->getKey()),
            'name'  => 'sometimes|required',
        ];
    }

    public function removeRules(Model $model): array
    {
        return [
            'DT_RowId' => 'required|exists:' . $model->getTable() . ',id',
        ];
    }
}
```

---

### Example 3: With Custom Model Namespace

```bash
php artisan datatables:editor Posts --model-namespace="Entities"
```

> {info} This option implicitly activates `--model` and uses `App\Entities\Post` as the model.

```php
namespace App\DataTables;

use Illuminate\Database\Eloquent\Model;
use Illuminate\Validation\Rule;
use Yajra\DataTables\DataTablesEditor;
use App\Entities\Post;

/** @extends DataTablesEditor<Post> **/
class PostsDataTableEditor extends DataTablesEditor
{
    protected $model = Post::class;

    // ... rules methods
}
```

---

<a name="generated-file-location"></a>
## Generated File Location

| Input | Generated File |
|-------|---------------|
| `datatables:editor Users` | `app/DataTables/UsersDataTableEditor.php` |
| `datatables:editor Posts --model` | `app/DataTables/PostsDataTableEditor.php` |

---

<a name="next-steps"></a>
## Next Steps

After generating your Editor class:

1. **[Editor Model](/docs/{{package}}/{{version}}/editor-model)** - Configure your model's fillable properties
2. **[Editor Rules](/docs/{{package}}/{{version}}/editor-rules)** - Update validation rules for your fields
3. **[Editor Events](/docs/{{package}}/{{version}}/editor-events)** - Add business logic with event hooks
4. **[Editor Usage](/docs/{{package}}/{{version}}/editor-usage)** - Set up routes and frontend
