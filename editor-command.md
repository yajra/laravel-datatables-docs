---
title: DataTables Editor Command
description: Artisan command to generate DataTables Editor classes
---

# DataTables Editor Command

> [!NOTE]
> The `datatables:editor` artisan command generates a complete Editor class with all required methods.

---

## Overview

The Editor command creates a new `DataTablesEditor` class with:
- Pre-configured model property
- Validation rules for create, edit, and remove
- Ready for customization

---

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

## Command Options

| Option | Description |
|--------|-------------|
| `--model` | Use singular form as the model name |
| `--model-namespace="namespace"` | Set custom model namespace |

---

## Examples

### Example 1: Basic Editor

```bash
php artisan datatables:editor Posts
```

Creates `app/DataTables/PostsDataTableEditor.php`:

```php
<?php
// app/DataTables/PostsDataTableEditor.php

namespace App\DataTables;

use App\Models\User;
use Illuminate\Database\Eloquent\Model;
use Illuminate\Validation\Rule;
use Yajra\DataTables\DataTablesEditor;

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

> [!TIP]
> The `--model` option uses the singular form (`Post`) as the model class and includes generics.

Creates `app/DataTables/PostsDataTableEditor.php` with `App\Post` model:

```php
<?php
// app/DataTables/PostsDataTableEditor.php

namespace App\DataTables;

use App\Models\Post;
use Illuminate\Database\Eloquent\Model;
use Illuminate\Validation\Rule;
use Yajra\DataTables\DataTablesEditor;

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

> [!NOTE]
> This option implicitly activates `--model` and uses `App\Entities\Post` as the model.

```php
<?php
// app/DataTables/PostsDataTableEditor.php

namespace App\DataTables;

use App\Entities\Post;
use Illuminate\Database\Eloquent\Model;
use Illuminate\Validation\Rule;
use Yajra\DataTables\DataTablesEditor;

/** @extends DataTablesEditor<Post> **/
class PostsDataTableEditor extends DataTablesEditor
{
    protected $model = Post::class;

    // ... rules methods
}
```

---

## Generated File Location

| Input | Generated File |
|-------|---------------|
| `datatables:editor Users` | `app/DataTables/UsersDataTableEditor.php` |
| `datatables:editor Posts --model` | `app/DataTables/PostsDataTableEditor.php` |

---

## Next Steps

After generating your Editor class:

1. **[Editor Model](/docs/{{package}}/{{version}}/editor-model)** - Configure your model's fillable properties
2. **[Editor Rules](/docs/{{package}}/{{version}}/editor-rules)** - Update validation rules for your fields
3. **[Editor Events](/docs/{{package}}/{{version}}/editor-events)** - Add business logic with event hooks
4. **[Editor Usage](/docs/{{package}}/{{version}}/editor-usage)** - Set up routes and frontend
