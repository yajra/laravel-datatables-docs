# DataTables Editor Event Hooks

In addition to Laravel's model events, DataTables Editor offers some pre & post event hooks.

<a name="create-events"></a>
## Create Events

Create action has the following event hooks:

- `creating` event hook that is fired before creating a new record.
- `created` event hook that is fired after the record was created.

To use the event hook, just add the methods on your editor class.

```php
public function creating(Model $model, array $data)
{
    // Prior to version 1.8.0 of Laravel DataTables Editor the hook was not required
    // to return the $model.
    // In version 1.8.0+ the hook must return the $model instance:
    return $model;
}

public function created(Model $model, array $data)
{
    // Prior to version 1.8.0 of Laravel DataTables Editor the hook was not required
    // to return the $model.
    // In version 1.8.0+ the hook must return the $model instance:
    return $model;
}
```

<a name="edit-events"></a>
## Edit Events

Edit action has the following event hooks:

- `updating` event hook that is fired before updating a new record.
- `updated` event hook that is fired after the record was updated.

To use the event hook, just add the methods on your editor class.

```php
public function updating(Model $model, array $data) {
    // Prior to version 1.8.0 of Laravel DataTables Editor the hook was not required
    // to return the $model.
    // In version 1.8.0+ the hook must return the $model:
    return $model;
}

public function updated(Model $model, array $data) {
    // Prior to version 1.8.0 of Laravel DataTables Editor the hook was not required
    // to return the $model.
    // In version 1.8.0+ the hook must return the $model instance:
    return $model;
}
```

<a name="saved-event"></a>
## Saved event

In addition to create and edit events, the `saved` event hook is called after `created` and `saved`.

To use the event hook, just add the method on your editor class:

```php
public function saved(Model $model, array $data)
{
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
public function deleting(Model $model, array $data) {
    // Record still exists in database. Code can be used to delete records from
    // child tables that don't specify cascade deletes on the foreign key
    // definition.
}

public function deleted(Model $model, array $data) {
    // Record no longer exists in database, but $model instance still contains
    // data as it was before deleting. Any instance state mutation will be
    // preserved and returned in the 'data' array of the response.
}
```
