---
title: Advanced Button Configuration
description: Advanced DataTables Button options for Editor integration
---

# Advanced Button Configuration

DataTables Buttons provide advanced configuration options, especially for Editor integration. This guide covers features beyond basic button setup.

---

<a name="editor-buttons"></a>
## Editor Buttons

When using DataTables Editor, buttons can be configured with additional options:

```php
use Yajra\DataTables\Html\Button;
use Yajra\DataTables\Html\Editor\Editor;

// Basic editor buttons
->buttons([
    Button::make('create')->editor('editor'),
    Button::make('edit')->editor('editor'),
    Button::make('remove')->editor('editor'),
])
```

---

<a name="refresh"></a>
## Refresh Data on Edit

Request data refresh from the server when starting an edit operation:

```php
Button::make('edit')
    ->editor('editor')
    ->refresh()
```

This ensures the form displays the latest data from the server before editing.

---

<a name="form-buttons"></a>
## Custom Form Buttons

Customize the buttons displayed in the Editor form:

```php
Button::make('create')
    ->editor('editor')
    ->formButtons([
        [
            'text' => 'Create',
            'className' => 'btn btn-primary',
        ],
        [
            'text' => 'Cancel',
            'className' => 'btn btn-secondary',
        ],
    ])
```

---

<a name="form-message"></a>
## Custom Form Message

Set a custom message to display in the Editor form:

```php
Button::make('create')
    ->editor('editor')
    ->formMessage('Are you sure you want to create this record?')

Button::make('remove')
    ->editor('editor')
    ->formMessage('This action cannot be undone.')
```

---

<a name="form-title"></a>
## Custom Form Title

Set a custom title for the Editor form:

```php
Button::make('create')
    ->editor('editor')
    ->formTitle('Create New User')

Button::make('edit')
    ->editor('editor')
    ->formTitle('Edit User Details')

Button::make('remove')
    ->editor('editor')
    ->formTitle('Confirm Deletion')
```

---

<a name="action-handler"></a>
## Action Handler

Set the editor class action handler:

```php
Button::make('create')
    ->editor('editor')
    ->actionHandler('App\\Actions\\CreateUserAction')
```

---

<a name="button-fluent-api"></a>
## Button Fluent API

The `Button` class provides a fluent API for configuration:

### Basic Options

```php
Button::make('create')
    ->text('Add New User')
    ->className('btn btn-success')
    ->name('createUser')
    ->tag('button')
    ->titleAttr('Create a new user')
```

### Availability

Control when a button is available:

```php
Button::make('remove')
    ->editor('editor')
    ->available('function(dt, node, config) {
        return dt.rows({selected: true}).count() > 0;
    }')
```

### Enabled State

Control button enabled state:

```php
Button::make('edit')
    ->editor('editor')
    ->enabled(false)
```

### Keyboard Shortcut

Set keyboard shortcuts:

```php
Button::make('create')
    ->editor('editor')
    ->key('ctrl+n')

Button::make('edit')
    ->editor('editor')
    ->key('ctrl+e')
```

### Button Attributes

Set HTML attributes:

```php
Button::make('create')
    ->editor('editor')
    ->attr([
        'data-bs-toggle' => 'modal',
        'data-bs-target' => '#confirmModal',
    ])
```

### Init Callback

Run JavaScript when the button is initialized:

```php
Button::make('create')
    ->editor('editor')
    ->init('function(dt, node, config) {
        console.log("Button initialized");
    }')
```

### Customize Callback

Customize button after creation:

```php
Button::make('export')
    ->customize('function(dt, node, config) {
        $(node).addClass("btn-lg");
    }')
```

---

<a name="editor-actions"></a>
## Editor Action Handlers

### Action Submit

Submit the form:

```php
Button::make('save')
    ->actionSubmit()
```

### Action Close

Close the form:

```php
Button::make('cancel')
    ->actionClose()
```

### Custom Action Handler

Set a custom action handler:

```php
Button::make('archive')
    ->editor('editor')
    ->actionHandler('archiveUser')
```

---

<a name="nested-buttons"></a>
## Nested Buttons

Create button groups with nested buttons:

```php
Button::make('collection')
    ->text('<i class="fa fa-plus"></i> Actions')
    ->buttons([
        Button::make('create')
            ->editor('editor')
            ->text('Add User'),
        Button::make('edit')
            ->editor('editor')
            ->text('Edit User'),
        Button::make('remove')
            ->editor('editor')
            ->text('Delete User'),
    ])
```

---

<a name="alignment"></a>
## Button Alignment

Control button alignment:

```php
Button::make('create')
    ->align('button-left')  // Default
    ->align('button-right')
```

---

<a name="namespace"></a>
## Button Namespace

Set a namespace for the button:

```php
Button::make('custom')
    ->namespace('myNamespace')
```

---

<a name="complete-example"></a>
## Complete Example

```php
<?php

namespace App\DataTables;

use App\Models\User;
use Yajra\DataTables\Html\Builder;
use Yajra\DataTables\Html\Button;
use Yajra\DataTables\Html\Column;
use Yajra\DataTables\Html\Editor;
use Yajra\DataTables\Html\Editor\Fields;
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
                        // Create button with custom form
                        Button::make('create')
                            ->editor('editor')
                            ->text('<i class="fa fa-plus"></i> Add User')
                            ->formTitle('Create New User')
                            ->formMessage('Please fill in all required fields.')
                            ->className('btn btn-success'),

                        // Edit button with refresh
                        Button::make('edit')
                            ->editor('editor')
                            ->text('<i class="fa fa-edit"></i> Edit')
                            ->formTitle('Edit User')
                            ->refresh()
                            ->key('ctrl+e'),

                        // Remove button with confirmation
                        Button::make('remove')
                            ->editor('editor')
                            ->text('<i class="fa fa-trash"></i> Delete')
                            ->formTitle('Confirm Deletion')
                            ->formMessage('This action cannot be undone. Are you sure?')
                            ->formButtons([
                                [
                                    'text' => 'Delete',
                                    'className' => 'btn btn-danger',
                                ],
                                [
                                    'text' => 'Cancel',
                                    'className' => 'btn btn-secondary',
                                ],
                            ]),

                        // Export buttons group
                        Button::make('collection')
                            ->text('<i class="fa fa-download"></i> Export')
                            ->buttons([
                                Button::make('excel')->text('Excel'),
                                Button::make('csv')->text('CSV'),
                                Button::make('pdf')->text('PDF'),
                            ]),

                        // Utility buttons
                        Button::make('print'),
                        Button::make('reset'),
                        Button::make('reload'),
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
            Fields\Text::make('name')->required(),
            Fields\Text::make('email')->required(),
            Fields\Select::make('status')
                ->options([
                    'active' => 'Active',
                    'inactive' => 'Inactive',
                ]),
        ];
    }
}
```

---

## See Also

- [Buttons Export](/docs/{{package}}/{{version}}/buttons-export) - Export button options
- [Editor Usage](/docs/{{package}}/{{version}}/editor-usage) - Editor integration guide
- [Editor Form Options](/docs/{{package}}/{{version}}/editor-form-options) - Form configuration
