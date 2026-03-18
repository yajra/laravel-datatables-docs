# Fast Excel Integration

[Fast-Excel](https://github.com/rap2hpoutre/fast-excel) is recommended when exporting bulk records.

> **Important**: FastExcel integration uses cursor behind the scene thus eager loaded columns will not work on export. You MUST use join statements if you want to export related columns.

<a name="usage"></a>
## Usage

1. Install `fast-excel` using `composer require rap2hpoutre/fast-excel`
2. Create a dataTable class `php artisan datatables:make Users`
3. Adjust `UsersDataTable` as needed
4. Set property `$fastExcel = true`

```php
class UsersDataTable extends DataTable
{
    protected bool $fastExcel = true;
}
```

<a name="disabling-callback"></a>
## Disabling Callback

Set property `$fastExcelCallback = false` to disable header formatting. The exported file will be based on your query's structure.

```php
class UsersDataTable extends DataTable
{
    protected bool $fastExcel = true;
    protected bool $fastExcelCallback = false;
}
```

<a name="custom-callback"></a>
## Custom Callback

Override the `fastExcelCallback` method:

```php
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

<a name="see-also"></a>
## See Also

- [Fast Excel Docs](https://github.com/rap2hpoutre/fast-excel) - Official Fast Excel documentation
- [Laravel Excel](buttons-laravel-excel.md) - Alternative Excel export method
- [Export Options](exports-options.md) - Customize export formatting
