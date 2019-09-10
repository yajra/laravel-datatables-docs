# DataTables Editor Rules

DataTables Editor requires three (3) rules for create, edit and remove action respectively.

<a name="create-rules"></a>
## Create Rules

This are the rules that will be used when validating a create action.

```php
public function createRules() {
    return [
        'email' => 'required|email|unique:users',
        'name'  => 'required',
    ];
}
```

<a name="edit-rules"></a>
## Edit Rules

This are the rules that will be used when validating an edit action.

```php
public function editRules(Model $model) {
    return [
        'email'      => 'sometimes|required|email|' . Rule::unique($model->getTable())->ignore($model->getKey()),
        'first_name' => 'sometimes|required',
    ];
}
```

<a name="remove-rules"></a>
## Remove Rules

This are the rules that will be used when validating a remove action.

```php
public function removeRules(Model $model) {
    return [];
}
```

<a name="upload-rules"></a>
## Upload Rules

This are the rules that will be used when validating an upload action.

```php
public function uploadRules() {
    return [
        'avatar' => 'required|image',
        'resume' => 'required|mimes:pdf',
    ];
}
```
