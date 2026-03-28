---
title: Editor Form Options
description: Configure DataTables Editor form display options
---

# Editor Form Options

DataTables Editor provides various form display options to control how forms appear and behave during create, edit, and remove operations.

---

<a name="form-options"></a>
## Form Options

Configure how the Editor form is displayed using the `FormOptions` class:

```php
use Yajra\DataTables\Html\FormOptions;
use Yajra\DataTables\Html\Editor\Editor;
```

---

<a name="main-form"></a>
## Main Form Options

Configure the main (lightbox) form display:

```php
Editor::make()
    ->formOptionsMain(function (FormOptions $options) {
        $options
            ->title('Edit Record')
            ->buttons([
                'btn-primary' => 'Save',
                'btn-secondary' => 'Cancel',
            ])
            ->onComplete('close')
            ->onEsc('close');
    })
```

### Available FormOptions Methods

| Method | Description | Default |
|--------|-------------|---------|
| `title()` | Form title | `true` |
| `buttons()` | Form buttons | `['Create', 'Edit', 'Remove']` |
| `onComplete()` | Action on complete | `'close'` |
| `onEsc()` | Action on escape | `'close'` |
| `onReturn()` | Action on return key | `'submit'` |
| `onBackground()` | Action on background click | `'blur'` |
| `onBlur()` | Action on blur | `'close'` |
| `onFieldError()` | Action on field error | `'focus'` |
| `submit()` | Submit type | `'changed'` |
| `formScope()` | Form scope | `'row'` |
| `focus()` | Field to focus | `null` |
| `nest()` | Enable nesting | `false` |
| `drawType()` | Draw type | `''` |
| `message()` | Form message | `''` |
| `submitTrigger()` | Submit trigger | `null` |
| `submitHtml()` | Submit HTML | `''` |
| `refresh()` | Refresh data on edit | `false` |

---

<a name="bubble-form"></a>
## Bubble Form Options

Configure the bubble editing form:

```php
Editor::make()
    ->formOptionsBubble(function (FormOptions $options) {
        $options
            ->title(false)
            ->onComplete('close')
            ->buttons(false);
    })
```

---

<a name="inline-form"></a>
## Inline Form Options

Configure inline editing form:

```php
Editor::make()
    ->formOptionsInline(function (FormOptions $options) {
        $options
            ->title(false)
            ->buttons(false)
            ->onComplete('close');
    })
```

---

<a name="hidden-fields"></a>
## Hidden Fields

Hide specific fields based on the action being performed:

### Hide on Create

```php
Editor::make()
    ->hiddenOnCreate(['id', 'created_at', 'updated_at'])
```

### Hide on Edit

```php
Editor::make()
    ->hiddenOnEdit(['password', 'created_at'])
```

### Hide on Specific Action

```php
Editor::make()
    ->hiddenOn('create', ['id', 'created_at'])
    ->hiddenOn('edit', ['password'])
    ->hiddenOn('remove', ['name', 'email'])
```

---

<a name="combined-example"></a>
## Combined Example

```php
<?php

namespace App\DataTables;

use App\Models\User;
use Yajra\DataTables\Html\Builder;
use Yajra\DataTables\Html\Button;
use Yajra\DataTables\Html\Column;
use Yajra\DataTables\Html\Editor;
use Yajra\DataTables\Html\Editor\Fields;
use Yajra\DataTables\Html\FormOptions;
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
                              ->fields($this->getFields())
                              // Hide ID and timestamps on create
                              ->hiddenOnCreate(['id', 'created_at', 'updated_at'])
                              // Hide password on edit
                              ->hiddenOnEdit(['password'])
                              // Configure main form
                              ->formOptionsMain(function (FormOptions $options) {
                                  $options
                                      ->title('User Details')
                                      ->buttons([
                                          [
                                              'text' => 'Save',
                                              'className' => 'btn btn-primary',
                                          ],
                                          [
                                              'text' => 'Cancel',
                                              'className' => 'btn btn-secondary',
                                          ],
                                      ])
                                      ->onComplete('close');
                              })
                              // Configure bubble form
                              ->formOptionsBubble(function (FormOptions $options) {
                                  $options
                                      ->title(false)
                                      ->buttons(false);
                              }),
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
            Fields\Text::make('id')->readOnly(),
            Fields\Text::make('name')->required(),
            Fields\Text::make('email')->required(),
            Fields\Password::make('password'),
            Fields\Select::make('status')
                ->options([
                    'active' => 'Active',
                    'inactive' => 'Inactive',
                ]),
            Fields\DateTime::make('created_at')->readOnly(),
            Fields\DateTime::make('updated_at')->readOnly(),
        ];
    }
}
```

---

## See Also

- [Editor Usage](/docs/{{package}}/{{version}}/editor-usage) - Editor integration guide
- [Editor Fields](/docs/{{package}}/{{version}}/editor-fields) - Field types and configuration
- [Editor Events](/docs/{{package}}/{{version}}/editor-events) - Event hooks
