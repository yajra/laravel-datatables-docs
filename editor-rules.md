# DataTables Editor Rules

DataTables Editor requires three (3) rules for create, edit and remove action respectively.

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

## Remove Rules

This are the rules that will be used when validating a remove action.

```php
public function removeRules(Model $model) {
    return [];
}
```
