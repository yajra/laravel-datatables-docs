---
title: DataTables Editor Model
description: Configure the Eloquent model for DataTables Editor
---

# DataTables Editor Model

> [!NOTE]
> The Editor Model is the Eloquent model that will be used for all CRUD operations.

---

## Overview

Every Editor class requires a model to perform CRUD operations. The model handles:
- Database queries
- Attribute assignment
- Data retrieval and storage

> [!TIP]
> All CRUD operations are automatically wrapped in database transactions.

---

## Setup Model

### Basic Configuration

Set the `$model` property in your Editor class:

```php
<?php
// app/DataTables/UsersDataTableEditor.php

namespace App\DataTables;

use App\Models\User;
use Yajra\DataTables\DataTablesEditor;

/** @extends DataTablesEditor<User> **/
class UsersDataTableEditor extends DataTablesEditor
{
    protected $model = User::class;
}
```

### With Custom Namespace

```php
<?php
// app/DataTables/UsersDataTableEditor.php

namespace App\DataTables;

use App\Entities\User;
use Yajra\DataTables\DataTablesEditor;

/** @extends DataTablesEditor<User> **/
class UsersDataTableEditor extends DataTablesEditor
{
    protected $model = User::class;
}
```

---

## Fillable Property

> [!WARNING]
> The Editor relies on `$fillable` to mass-assign attributes. Always define this property.

### Basic Fillable

```php
<?php
// app/Models/User.php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class User extends Model
{
    protected $fillable = [
        'name',
        'email',
        'password',
    ];
}
```

### With Guarded

```php
<?php
// app/Models/User.php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class User extends Model
{
    protected $guarded = [
        'id',
        'created_at',
        'updated_at',
    ];
}
```

---

## Model Requirements

| Requirement | Description |
|------------|-------------|
| `$fillable` or `$guarded` | Define which attributes can be mass-assigned |
| Timestamps | Recommended for `created_at` and `updated_at` tracking |
| Primary Key | Ensure primary key is named `id` or configured in `$primaryKey` |

---

## Common Patterns

### With Relationships

For advanced operations involving relationships, use [Event Hooks](/docs/{{package}}/{{version}}/editor-events):

```php
<?php
// app/DataTables/PostsDataTableEditor.php

namespace App\DataTables;

use App\Models\Post;
use Illuminate\Database\Eloquent\Model;
use Yajra\DataTables\DataTablesEditor;

/** @extends DataTablesEditor<Post> **/
class PostsDataTableEditor extends DataTablesEditor
{
    protected $model = Post::class;

    public function created(Model $model, array $data): Model
    {
        // Attach tags after post creation
        if (isset($data['tags'])) {
            $model->tags()->sync($data['tags']);
        }

        return $model;
    }
}
```

### With Accessors & Mutators

```php
<?php
// app/Models/User.php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class User extends Model
{
    protected $fillable = [
        'first_name',
        'last_name',
        'email',
    ];

    protected $appends = ['full_name'];

    public function getFullNameAttribute(): string
    {
        return "{$this->first_name} {$this->last_name}";
    }

    public function setPasswordAttribute($value): void
    {
        $this->attributes['password'] = bcrypt($value);
    }
}
```

### Soft Deletes

```php
<?php
// app/Models/User.php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;
use Illuminate\Database\Eloquent\SoftDeletes;

class User extends Model
{
    use SoftDeletes;

    protected $fillable = [
        'name',
        'email',
    ];

    protected $dates = ['deleted_at'];
}
```

---

## Validation with Model

Use the model instance in your validation rules:

```php
public function editRules(Model $model): array
{
    return [
        'email' => [
            'sometimes',
            'required',
            'email',
            Rule::unique($model->getTable())->ignore($model->getKey()),
        ],
    ];
}
```

### Available Model Methods in Rules

| Method | Description |
|--------|-------------|
| `$model->getTable()` | Get the database table name |
| `$model->getKey()` | Get the primary key value |
| `$model->getKeyName()` | Get the primary key column name |
| `$model->getFillable()` | Get the fillable attributes |

---

## Related Documentation

| Documentation | Description |
|---------------|-------------|
| [Editor Rules](/docs/{{package}}/{{version}}/editor-rules) | Validation rules reference |
| [Editor Events](/docs/{{package}}/{{version}}/editor-events) | Event hooks for relationships |
| [Laravel Eloquent](https://laravel.com/docs/eloquent) | Full Eloquent documentation |
| [Mass Assignment](https://laravel.com/docs/eloquent#mass-assignment) | Laravel fillable guide |
