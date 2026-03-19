---
title: Print Column
description: Configure columns for print view
---

# Print Column

You can print a column with a customized header if manually set.

---

Configure which columns appear when users print the DataTable.

---

<a name="property"></a>
## Using $printColumns Property

Define which columns to include in print output:

```php
<?php

namespace App\DataTables;

use App\Models\User;
use Yajra\DataTables\Services\DataTable;

class UsersDataTable extends DataTable
{
    protected $printColumns = [
        ['data' => 'name', 'title' => 'Name'],
        ['data' => 'email', 'title' => 'Registered Email'],
    ];
}
```

Or use simple string keys when the data key equals the title:

```php
protected $printColumns = [
    'name',
    'email',
];
```

---

## Using Column Method

You can also configure print columns using the fluent method:

```php
use Yajra\DataTables\Html\Column;

Column::make('name')
    ->printable(true),

Column::make('email')
    ->printable(true)
    ->title('Email Address'),
```

---

## Exclude from Print

```php
use Yajra\DataTables\Html\Column;

Column::make('action')
    ->printable(false),  // Exclude from print
```

---

## See Also

- [Export Columns](/docs/{{package}}/{{version}}/export-column) - Export column configuration
- [HTML Builder Column](/docs/{{package}}/{{version}}/html-builder-column) - Column options
