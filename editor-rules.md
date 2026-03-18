---
title: DataTables Editor Rules
description: Validation rules for DataTables Editor CRUD operations
---

# DataTables Editor Rules

> [!NOTE]
> Validation rules define what data is allowed when creating, editing, or removing records.

> [!TIP]
> The type hint `Model` is used, but the actual model type is inferred from the generics (`/** @extends DataTablesEditor<User> **/`) for static analysis.

---

## Overview

DataTables Editor requires validation rules for three main actions:

| Action | Method | Purpose |
|--------|--------|---------|
| Create | `createRules()` | Validate new record data |
| Edit | `editRules()` | Validate updated record data |
| Remove | `removeRules()` | Validate deletion requests |
| Upload | `uploadRules()` | Validate file uploads |

---

## Create Rules

> [!TIP]
> Use these rules when validating data for new records being created.

```php
public function createRules(): array
{
    return [
        'email' => 'required|email|unique:users',
        'name'  => 'required|max:255',
        'password' => 'required|min:8|confirmed',
    ];
}
```

---

## Edit Rules

> [!WARNING]
> When editing, you must ignore the current record in unique validation rules.

```php
use Illuminate\Validation\Rule;

public function editRules(Model $model): array
{
    return [
        'email' => [
            'sometimes',
            'required',
            'email',
            Rule::unique($model->getTable())->ignore($model->getKey()),
        ],
        'name' => 'sometimes|required|max:255',
    ];
}
```

### Alternative Syntax

You can also use string-based unique rules:

```php
public function editRules(Model $model): array
{
    return [
        'email' => 'sometimes|required|email|unique:users,email,' . $model->getKey(),
        'name'  => 'sometimes|required',
    ];
}
```

---

## Remove Rules

> [!NOTE]
> `DT_RowId` is always submitted in the request and contains the row's data DT_RowId value (e.g., `row_1`). It should be validated as required and must exist in the database.

```php
public function removeRules(Model $model): array
{
    return [
        'DT_RowId' => 'required|exists:' . $model->getTable() . ',id',
    ];
}
```

### With Additional Validation

```php
public function removeRules(Model $model): array
{
    return [
        'DT_RowId' => 'required|exists:' . $model->getTable() . ',id',
        // Add additional validation rules here
    ];
}
```

---

## Upload Rules

```php
public function uploadRules(): array
{
    return [
        'avatar' => 'required|image|mimes:jpg,jpeg,png,gif|max:2048',
        'resume' => 'required|mimes:pdf|max:5120',
    ];
}
```

---

## Rule Parameters

The `$model` parameter in `editRules()` and `removeRules()` provides:

| Property | Description |
|----------|-------------|
| `$model->getTable()` | Get the database table name |
| `$model->getKey()` | Get the primary key value |
| `$model->getKeyName()` | Get the primary key column name |

---

## Common Validation Rules

| Rule | Usage | Description |
|------|-------|-------------|
| `required` | Both | Field must be present |
| `sometimes` | Edit | Only validate if field is present |
| `email` | Both | Must be valid email format |
| `unique` | Create | Value must be unique in table |
| `exists` | Both | Value must exist in table |
| `numeric` | Both | Must be a number |
| `date` | Both | Must be a valid date |

---

## Related Documentation

- [Laravel Validation](https://laravel.com/docs/validation) - Full validation documentation
- [Editor Events](/docs/{{package}}/{{version}}/editor-events) - Modify data before/after validation
- [Editor Model](/docs/{{package}}/{{version}}/editor-model) - Model configuration
