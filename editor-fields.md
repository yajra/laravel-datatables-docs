---
title: Editor Fields
description: Configure DataTables Editor fields with various types and options
---

# Editor Fields

DataTables Editor provides a variety of field types for building forms. Each field type has specific configuration options.

---

<a name="field-types"></a>
## Available Field Types

| Type | Class | Description |
|------|-------|-------------|
| Text | `Fields\Text` | Single-line text input |
| TextArea | `Fields\TextArea` | Multi-line text input |
| Password | `Fields\Password` | Password input |
| Select | `Fields\Select` | Dropdown select |
| Select2 | `Fields\Select2` | Enhanced select with search |
| Radio | `Fields\Radio` | Radio button group |
| Checkbox | `Fields\Checkbox` | Checkbox group |
| Boolean | `Fields\Boolean` | True/false toggle |
| Date | `Fields\Date` | Date picker |
| DateTime | `Fields\DateTime` | Date and time picker |
| Time | `Fields\Time` | Time picker |
| Number | `Fields\Number` | Numeric input |
| Hidden | `Fields\Hidden` | Hidden field |
| ReadOnly | `Fields\ReadOnly` | Read-only display |
| File | `Fields\File` | File upload |
| Image | `Fields\Image` | Image upload |
| Tags | `Fields\Tags` | Tags input |
| BelongsTo | `Fields\BelongsTo` | BelongsTo relationship |

---

<a name="basic-configuration"></a>
## Basic Field Configuration

```php
use Yajra\DataTables\Html\Editor\Fields;

// Simple text field
Fields\Text::make('name')

// With label
Fields\Text::make('name', 'Full Name')

// With chained configuration
Fields\Text::make('name')
    ->label('Full Name')
    ->placeholder('Enter your name')
    ->required()
```

---

<a name="common-options"></a>
## Common Field Options

### Label

Set the field label:

```php
Fields\Text::make('name')
    ->label('Full Name')
```

### Placeholder

Set the input placeholder:

```php
Fields\Text::make('name')
    ->placeholder('Enter your name')
```

### Default Value

Set a default value:

```php
Fields\Text::make('status')
    ->default('active')
```

### Required

Add a required indicator to the field label:

```php
Fields\Text::make('name')
    ->required()

// Or with explicit boolean
Fields\Text::make('name')
    ->required(true)
```

### Null Default

Replace null values with a default when editing:

```php
Fields\Text::make('description')
    ->nullDefault()

// With specific default value
Fields\Text::make('description')
    ->nullDefault(true)
    ->default('No description')
```

### CSS Class

Add CSS classes to the field:

```php
Fields\Text::make('name')
    ->className('form-control')

// Or via attribute
Fields\Text::make('name')
    ->attr('class', 'form-control')
```

### Field Info

Add informational text below the field:

```php
Fields\Text::make('email')
    ->fieldInfo('We will never share your email')
```

### Label Info

Add info text next to the label:

```php
Fields\Text::make('name')
    ->labelInfo('(required)')
```

---

<a name="select-fields"></a>
## Select Fields

### Basic Select

```php
Fields\Select::make('status')
    ->options([
        'active' => 'Active',
        'inactive' => 'Inactive',
        'pending' => 'Pending',
    ])
```

### Model Options

Load options from a model:

```php
Fields\Select::make('user_id')
    ->modelOptions(User::class, 'name', 'id')
```

### Table Options

Load options from a database table:

```php
Fields\Select::make('category_id')
    ->tableOptions('categories', 'name', 'id')
```

### Enum Options

Load options from a PHP enum:

```php
use App\Enums\StatusEnum;

Fields\Select::make('status')
    ->enumOptions(StatusEnum::cases())
```

---

<a name="date-fields"></a>
## Date Fields

### Date Picker

```php
Fields\Date::make('birth_date')
    ->format('YYYY-MM-DD')
```

### DateTime Picker

```php
Fields\DateTime::make('created_at')
    ->format('YYYY-MM-DD HH:mm:ss')
```

### DateTime with Wire Format

For Livewire integration, specify a wire format:

```php
Fields\DateTime::make('created_at')
    ->wireFormat('Y-m-d H:i:s')
```

### Display Format

Set a separate display format:

```php
Fields\DateTime::make('created_at')
    ->displayFormat('MMM D, YYYY h:mm A')
```

### Key Input Control

Control whether users can type into the date input:

```php
// Allow typing (default)
Fields\Date::make('birth_date')
    ->keyInput(true)

// Restrict to picker only
Fields\Date::make('birth_date')
    ->keyInput(false)
```

---

<a name="belongs-to"></a>
## BelongsTo Field

The `BelongsTo` field automatically handles belongsTo relationships:

```php
use Yajra\DataTables\Html\Editor\Fields\BelongsTo;

BelongsTo::make('user')
    ->label('Assigned User')
```

This field automatically:
- Loads options from the related model
- Sets the correct value for create/edit operations
- Handles the relationship display

---

<a name="boolean-fields"></a>
## Boolean Fields

```php
Fields\Boolean::make('is_active')
    ->label('Active Status')
```

With custom options:

```php
use Yajra\DataTables\Html\Options;

Fields\Boolean::make('is_active')
    ->options([
        Options::yesNo(),
        Options::trueFalse(),
        Options::from([1 => 'Yes', 0 => 'No']),
    ])
```

---

<a name="field-options-helpers"></a>
## Field Option Helpers

### Yes/No

```php
Options::yesNo()  // ['Yes', 'No']
```

### True/False

```php
Options::trueFalse()  // ['True', 'False']
```

### Custom Options

```php
Options::from(['active' => 'Active', 'inactive' => 'Inactive'])
```

---

<a name="field-events"></a>
## Field Events

### Multi-Editable

Allow editing multiple rows at once:

```php
Fields\Text::make('status')
    ->multiEditable(true)
```

### Submit on Change

Submit the form when the field changes:

```php
Fields\Select::make('status')
    ->submit(true)
```

### Compare Values

Enable value comparison in the form:

```php
Fields\Text::make('name')
    ->compare(true)
```

---

<a name="complete-example"></a>
## Complete Example

```php
<?php

namespace App\DataTables;

use App\Models\User;
use App\Enums\StatusEnum;
use Yajra\DataTables\Html\Builder;
use Yajra\DataTables\Html\Column;
use Yajra\DataTables\Html\Editor;
use Yajra\DataTables\Html\Editor\Fields;
use Yajra\DataTables\Html\Editor\Fields\BelongsTo;
use Yajra\DataTables\Services\DataTable;

class UsersDataTable extends DataTable
{
    public function html(): Builder
    {
        return $this->builder()
                    ->setTableId('users-table')
                    ->columns($this->getColumns())
                    ->minifiedAjax()
                    ->orderBy(1)
                    ->selectStyleSingle()
                    ->editors([
                        Editor::make()
                              ->fields($this->getFields()),
                    ])
                    ->buttons([
                        Button::make('create')->editor('editor'),
                        Button::make('edit')->editor('editor'),
                        Button::make('remove')->editor('editor'),
                    ]);
    }

    protected function getColumns(): array
    {
        return [
            Column::make('id'),
            Column::make('name'),
            Column::make('email'),
            Column::make('status'),
            Column::make('created_at'),
        ];
    }

    protected function getFields(): array
    {
        return [
            Fields\Text::make('name')
                ->label('Full Name')
                ->required(),

            Fields\Text::make('email')
                ->label('Email Address')
                ->required()
                ->fieldInfo('We will never share your email'),

            Fields\Select::make('status')
                ->options([
                    'active' => 'Active',
                    'inactive' => 'Inactive',
                    'pending' => 'Pending',
                ])
                ->default('active'),

            Fields\Select::make('role_id')
                ->label('Role')
                ->tableOptions('roles', 'name', 'id'),

            BelongsTo::make('department')
                ->label('Department'),

            Fields\Date::make('birth_date')
                ->label('Birth Date')
                ->format('YYYY-MM-DD'),

            Fields\DateTime::make('last_login')
                ->label('Last Login')
                ->displayFormat('MMM D, YYYY h:mm A')
                ->nullDefault(),

            Fields\Boolean::make('is_active')
                ->label('Active')
                ->default(true),

            Fields\TextArea::make('bio')
                ->label('Biography')
                ->rows(4),

            Fields\File::make('avatar')
                ->label('Profile Picture')
                ->uploadRules('image|max:2048'),
        ];
    }
}
```

---

## See Also

- [Editor Usage](/docs/{{package}}/{{version}}/editor-usage) - Editor integration guide
- [Editor Rules](/docs/{{package}}/{{version}}/editor-rules) - Validation rules
- [Editor Events](/docs/{{package}}/{{version}}/editor-events) - Event hooks
