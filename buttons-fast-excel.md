---
title: Fast Excel Export
description: Export DataTables using Fast Excel
---

# Fast Excel Integration

[Fast-Excel](https://github.com/rap2hpoutre/fast-excel) is recommended when exporting bulk records.

> [!WARNING]
> FastExcel integration uses cursor behind the scenes thus eager loaded columns will not work on export. You MUST use join statements if you want to export related columns.

---

## Usage

1. Install `fast-excel` using `composer require rap2hpoutre/fast-excel`
2. Create a DataTable class `php artisan datatables:make Users`
3. Adjust `UsersDataTable` as needed
4. Set property `$fastExcel = true`

```php
<?php
// app/DataTables/UsersDataTable.php

namespace App\DataTables;

use Yajra\DataTables\Services\DataTable;

class UsersDataTable extends DataTable
{
    protected bool $fastExcel = true;
}
```

---

## Disabling Callback

Set property `$fastExcelCallback = false` to disable header formatting. The exported file will be based on your query's structure:

```php
protected bool $fastExcel = true;
protected bool $fastExcelCallback = false;
```

---

## Custom Callback

Override the `fastExcelCallback` method:

```php
<?php
// app/DataTables/UsersDataTable.php

namespace App\DataTables;

use Yajra\DataTables\Services\DataTable;

class UsersDataTable extends DataTable
{
    protected bool $fastExcel = true;

    public function fastExcelCallback(): \Closure
    {
        return function ($row) {
            return [
                'Name' => $row['name'],
                'Email' => $row['email'],
            ];
        };
    }
}
```

---

## See Also

- [Fast Excel Docs](https://github.com/rap2hpoutre/fast-excel) - Official Fast Excel documentation
- [Laravel Excel](/docs/{{package}}/{{version}}/buttons-laravel-excel) - Alternative Excel export method
- [Export Options](/docs/{{package}}/{{version}}/exports-options) - Customize export formatting
