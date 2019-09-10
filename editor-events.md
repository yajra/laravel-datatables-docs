# DataTables Editor Event Hooks

In addition to Laravel's model events, DataTables Editor offers some pre & post event hooks.

<a name="create-events"></a>
## Create Events

Create action has the following event hooks:

- `creating` event hook that is fired before creating a new record.
- `created` event hook that is fired after the record was created.

To use the event hook, just add the methods on your editor class.

```php
/**
 * Event hook that is fired before creating a new record.
 *
 * @param \Illuminate\Database\Eloquent\Model $model Empty model instance.
 * @param array $data Attribute values array received from Editor.
 * @return array The updated attribute values array.
 */
public function creating(Model $model, array $data)
{
    // Code can change the attribute values array before saving data to the
    // database.
    // Can be used to initialize values on new model.

    // Since arrays are copied when passed by value, the function must return
    // the updated $data array
    return $data;
}

/**
 * Event hook that is fired after a new record is created.
 *
 * @param \Illuminate\Database\Eloquent\Model $model The newly created model.
 * @param array $data Attribute values array received from `creating` or
 *   `saving` hook.
 * @return \Illuminate\Database\Eloquent\Model Since version 1.8.0 it must
 *   return the $model.
 */
public function created(Model $model, array $data)
{
    // Can be used to mutate state of newly created model that is returned to
    // Editor.

    // Prior to version 1.8.0 of Laravel DataTables Editor the hook was not
    // required to return the $model.
    // In version 1.8.0+ the hook must return the $model instance:
    return $model;
}
```

<a name="edit-events"></a>
## Edit Events

Edit action has the following event hooks:

- `updating` event hook that is fired before updating an existing record.
- `updated` event hook that is fired after the record was updated.

To use the event hook, just add the methods on your editor class.

```php
/**
 * Event hook that is fired before updating an existing record.
 *
 * @param \Illuminate\Database\Eloquent\Model $model Model instance retrived
 *  retrived from database.
 * @param array $data Attribute values array received from Editor.
 * @return array The updated attribute values array.
 */
public function updating(Model $model, array $data) {
    // Can be used to modify the attribute values received from Editor before
    // applying changes to model.

    // Since arrays are copied when passed by value, the function must return
    // the updated $data array
    return $data;
}

/**
 * Event hook that is fired after the record was updated.
 *
 * @param \Illuminate\Database\Eloquent\Model $model Updated model instance.
 * @param array $data Attribute values array received from `updating` or
 *   `saving` hook.
 * @return \Illuminate\Database\Eloquent\Model Since version 1.8.0 it must
 *   return the $model.
 */
public function updated(Model $model, array $data) {
    // Can be used to mutate state of updated model that is returned to Editor.

    // Prior to version 1.8.0 of Laravel DataTables Editor the hook was not required
    // to return the $model.
    // In version 1.8.0+ the hook must return the $model instance:
    return $model;
}
```

<a name="save-events"></a>
## Save events

In addition to create and edit events, the following save event hooks are available:

- `saving` event hook that is fired after `creating` and `updating` events, but
    before the model is saved to the database.
- `saved` event hook that is fired after `created` and `updated` events.

To use the event hook, just add the method on your editor class:

```php
/**
 * Event hook that is fired after `creating` and `updating` hooks, but before
 * the model is saved to the database.
 *
 * @param \Illuminate\Database\Eloquent\Model $model Empty model when creating;
 *   Original model when updating.
 * @param array $data Attribute values array received from `creating` or
 *   `updating` event hook.
 * @return array The updated attribute values array.
 */
public function saving(Model $model, array $data)
{
    // The event hook can be used to modify the $data array that is used to
    // create or update the record.

    // Since arrays are copied when passed by value, the function must return
    // the updated $data array
    return $data;
}

/**
 * Event hook that is fired after `created` and `updated` events.
 *
 * @param \Illuminate\Database\Eloquent\Model $model The new model when
 *   creating; the updated model when updating.
 * @param array $data Attribute values array received from `creating`,
 *   `updating`, or `saving`.
 * @return \Illuminate\Database\Eloquent\Model Since version 1.8.0 it must
 *   return the $model.
 */
public function saved(Model $model, array $data)
{
    // Can be used to mutate state of updated model that is returned to Editor.

    // Prior to version 1.8.0 of Laravel DataTables Editor the hook was not required
    // to return the $model.
    // In version 1.8.0+ the hook must return the $model instance:
    return $model;
}
```

<a name="remove-events"></a>
## Remove Events

Remove action has the following event hooks:

- `deleting` event hook that is fired before deleting a record.
- `deleted` event hook that is fired after the record was deleted.

To use the event hook, just add the methods on your editor class.

```php
/**
 * Event hook that is fired before deleting an existing record.
 *
 * @param \Illuminate\Database\Eloquent\Model $model The original model
 *   retrieved from database.
 * @param array $data Attribute values array received from Editor.
 * @return void
 */
public function deleting(Model $model, array $data) {
    // Record still exists in database. Code can be used to delete records from
    // child tables that don't specify cascade deletes on the foreign key
    // definition.
}

/**
 * Event hook that is fired after deleting the record from database.
 *
 * @param \Illuminate\Database\Eloquent\Model $model The original model
 *   retrieved from database.
 * @param array $data Attribute values array received from Editor.
 * @return void
 */
public function deleted(Model $model, array $data) {
    // Record no longer exists in database, but $model instance still contains
    // data as it was before deleting. Any changes to the $model instance will
    // be returned to Editor.
}
```

<a name="upload-events"></a>
## Upload Events

Upload action has the following event hooks:

- `uploaded` event hook that is fired after a file was uploaded.

To use the event hook, just add the methods on your editor class.

```php
/**
 * Event hook that is fired after upload a file.
 *
 * @param string $id The auto-generated file id from filesystem.
 * @return string
 */
public function uploaded($id) {
    // return the file id.
    return $id;
}
```
