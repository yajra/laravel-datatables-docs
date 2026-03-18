---
title: Laravel Excel Export
description: Export DataTables using Laravel Excel
---


# Laravel Excel Integration

[Laravel Excel](https://docs.laravel-excel.com) is the default package used when exporting DataTables to Excel and CSV.

<a name="using-export-class"></a>
## Using Export Class

1. Create an export class `php artisan make:export UsersExport`
2. Update the generated export class and extend `DataTablesCollectionExport`

```php
namespace App\Exports;

use Yajra\DataTables\Exports\DataTablesCollectionExport;

class UsersExport extends DataTablesCollectionExport
{
}
```

3. Update your `UsersDataTable` class and set `protected $exportClass = UsersExport::class`

```php
class UsersDataTable extends DataTable
{
    protected string $exportClass = UsersExport::class;
}
```

4. Update your export class as needed. See official package docs: https://docs.laravel-excel.com/3.1/exports/collection.html

<a name="example-export-class"></a>
## Example Export Class

```php
namespace App\Exports;

use Maatwebsite\Excel\Concerns\WithMapping;
use Yajra\DataTables\Exports\DataTablesCollectionExport;

class UsersExport extends DataTablesCollectionExport implements WithMapping
{
    public function headings(): array
    {
        return [
            'Name',
            'Email',
        ];
    }

    public function map($row): array
    {
        return [
            $row['name'],
            $row['email'],
        ];
    }
}
```

<a name="see-also"></a>
## See Also

- [Laravel Excel Docs](https://docs.laravel-excel.com) - Official Laravel Excel documentation
- [Buttons Fast Excel](buttons-fast-excel.md) - Alternative Excel export method
- [Export Options](exports-options.md) - Customize export formatting

