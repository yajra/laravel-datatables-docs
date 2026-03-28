---
title: DataTables Editor Event Hooks
description: Intercept and modify data during CRUD operations with event hooks
---

# DataTables Editor Event Hooks

> [!NOTE]
> Event hooks allow you to intercept and modify data during CRUD operations.

> [!TIP]
> The type hint `Model` is used, but the actual model type is inferred from the generics (`/** @extends DataTablesEditor<User> **/`) for static analysis.

---

## Overview

In addition to Laravel's model events, DataTables Editor provides pre and post event hooks for all CRUD actions:

```
┌─────────────────────────────────────────────────────────────────┐
│                     Event Hook Flow                              │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Create:    creating → saving → [DB SAVE] → saved → created     │
│  Edit:      updating → saving → [DB SAVE] → saved → updated     │
│  Remove:    deleting → [DB DELETE] → deleted                     │
│  Force Delete: deleting → [DB FORCE DELETE] → deleted            │
│  Restore:     updating → saving → [DB RESTORE] → saved → updated │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

<a name="create-events"></a>
## Create Events

### `creating` - Before Record Creation

Fired **before** a new record is saved to the database:

```php
/**
 * @param Model $model Empty model instance
 * @param array $data  Attribute values from Editor
 * @return array Modified data array
 */
public function creating(Model $model, array $data): array
{
    // Hash password before saving
    $data['password'] = bcrypt($data['password']);

    // Set default values
    $data['created_by'] = auth()->id();

    return $data;
}
```

### `created` - After Record Creation

Fired **after** a new record is successfully created:

```php
/**
 * @param Model $model The newly created model
 * @param array $data  Processed data array
 * @return Model The modified model
 */
public function created(Model $model, array $data): Model
{
    // Log the creation
    activity()->log('User created: ' . $model->name);

    // Send welcome email
    Mail::to($model)->send(new WelcomeEmail($model));

    return $model;
}
```

---

<a name="edit-events"></a>
## Edit Events

### `updating` - Before Record Update

Fired **before** changes are saved to the database:

```php
/**
 * @param Model $model Model instance from database
 * @param array $data  Attribute values from Editor
 * @return array Modified data array
 */
public function updating(Model $model, array $data): array
{
    // Track who made changes
    $data['updated_by'] = auth()->id();

    // Log old vs new values
    Log::info('Updating user', [
        'old' => $model->toArray(),
        'new' => $data,
    ]);

    return $data;
}
```

### `updated` - After Record Update

Fired **after** a record is successfully updated:

```php
/**
 * @param Model $model Updated model instance
 * @param array $data  Processed data array
 * @return Model The modified model
 */
public function updated(Model $model, array $data): Model
{
    // Clear caches
    Cache::forget("user_{$model->id}");

    return $model;
}
```

---

<a name="save-events"></a>
## Save Events

### `saving` - Before Database Save

Fired **after** `creating`/`updating` but **before** the database save:

> [!TIP]
> Use this hook to modify data for both create and edit operations.

```php
/**
 * @param Model $model Empty (creating) or original (updating)
 * @param array $data  Data from creating/updating hooks
 * @return array Modified data array
 */
public function saving(Model $model, array $data): array
{
    // Sanitize input
    $data['name'] = strip_tags($data['name']);

    return $data;
}
```

### `saved` - After Database Save

Fired **after** `created`/`updated` for both create and edit operations:

```php
/**
 * @param Model $model New (creating) or updated model
 * @param array $data  Processed data array
 * @return Model The modified model
 */
public function saved(Model $model, array $data): Model
{
    // Actions for both create and edit
    Cache::tags(['users'])->flush();

    return $model;
}
```

---

<a name="remove-events"></a>
## Remove Events

### `deleting` - Before Record Deletion

Fired **before** the record is deleted from the database:

> [!WARNING]
> The record still exists in the database at this point.

```php
/**
 * @param Model $model Original model from database
 * @param array $data  Attribute values from Editor
 */
public function deleting(Model $model, array $data): void
{
    // Delete related files
    Storage::delete($model->avatar_path);

    // Delete child records
    $model->posts()->delete();

    // Log the deletion
    activity()->log('User deleted: ' . $model->name);
}
```

### `deleted` - After Record Deletion

Fired **after** the record is deleted from the database:

> [!TIP]
> The `$model` instance still contains the original data.

```php
/**
 * @param Model $model Original model (no longer in DB)
 * @param array $data  Attribute values from Editor
 */
public function deleted(Model $model, array $data): void
{
    // Clean up related resources
    Notification::where('user_id', $model->id)->delete();
}
```

---

## Upload Events

### `uploaded` - After File Upload

Fired **after** a file is successfully uploaded:

```php
/**
 * @param string $id Auto-generated file ID from filesystem
 * @return string The file ID to return to Editor
 */
public function uploaded(string $id): string
{
    // Generate thumbnail
    Image::make($id)->resize(100, 100)->save();

    return $id;
}
```

---

## Complete Example

```php
<?php
// app/DataTables/UsersDataTableEditor.php

namespace App\DataTables;

use App\Models\User;
use Illuminate\Database\Eloquent\Model;
use Illuminate\Validation\Rule;
use Yajra\DataTables\DataTablesEditor;

/** @extends DataTablesEditor<User> **/
class UsersDataTableEditor extends DataTablesEditor
{
    protected $model = User::class;

    public function creating(Model $model, array $data): array
    {
        $data['password'] = bcrypt($data['password']);
        $data['created_by'] = auth()->id();
        return $data;
    }

    public function created(Model $model, array $data): Model
    {
        Mail::to($model)->send(new WelcomeEmail($model));
        return $model;
    }

    public function updating(Model $model, array $data): array
    {
        $data['updated_by'] = auth()->id();
        return $data;
    }

    public function updated(Model $model, array $data): Model
    {
        Cache::forget("user_{$model->id}");
        return $model;
    }

    public function deleting(Model $model, array $data): void
    {
        // Delete related files before removing user
        Storage::delete($model->avatar_path);
    }

    public function forceDeleting(Model $model, array $data): void
    {
        // Additional cleanup for force delete
        $model->posts()->forceDelete();
    }

    public function restore(Model $model, array $data): array
    {
        // Prepare data before restoring
        $data['restored_by'] = auth()->id();
        return $data;
    }

    public function restored(Model $model, array $data): Model
    {
        // Actions after restoring
        Notification::send($model, new AccountRestored($model));
        return $model;
    }

    public function createRules(): array
    {
        return [
            'name' => 'required',
            'email' => 'required|email|unique:users',
            'password' => 'required|min:8',
        ];
    }

    public function editRules(Model $model): array
    {
        return [
            'email' => [
                'sometimes',
                'required',
                'email',
                Rule::unique('users')->ignore($model->getKey()),
            ],
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

## See Also

- [Editor Rules](/docs/{{package}}/{{version}}/editor-rules) - Validation rules
- [Editor Model](/docs/{{package}}/{{version}}/editor-model) - Model configuration
- [Laravel Model Events](https://laravel.com/docs/eloquent#events) - Built-in model events
