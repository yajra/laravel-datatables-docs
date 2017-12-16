# DataTables Editor Event Hooks

In addition to Laravel's model events, DataTables Editor offers some pre & post event hooks.

<a name="create-events"></a>
## Create Events

Create action has the following event hooks:

- `creating` event hook that is fired before creating a new record.
- `created` event hook that is fired after the record was created.

To use the event hook, just add the methods on your editor class.

```php
public function creating(Model $model, array $data) {
    return $data;
}

public function created(Model $model, array $data) {
    return $data;
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
    return $data;
}

public function updated(Model $model, array $data) {
    return $data;
}
```

<a name="remove-events"></a>
## Remove Events

Remove action has the following event hooks:

- `deleting` event hook that is fired before deleting a new record.
- `deleted` event hook that is fired after the record was deleted.

To use the event hook, just add the methods on your editor class.

```php
public function deleting(Model $model, array $data) {
    return $data;
}

public function deleted(Model $model, array $data) {
    return $data;
}
```
