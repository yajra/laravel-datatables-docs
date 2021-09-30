# Fast Excel Integration

[Fast-Excel](https://github.com/rap2hpoutre/fast-excel) is recommended when exporting bulk records. 

## LIMITATIONS! 

FastExcel integration uses cursor behind the scene thus eager loaded columns will not work on export. You must used join statements if you want to export related columns. Also, column value formatting like converting boolean to Yes or No should be done on SQL LEVEL.

## Usage

1. Install `fast-excel` using `composer require rap2hpoutre/fast-excel`.
2. Create a dataTable class `php artisan datatables:make Users`
3. Adjust `UsersDataTable` as needed.
4. Set property `$fastExcel = true`.

```php
class UsersDataTable extends DataTable
{
    protected $fastExcel = true;

    ...
}
```

5. DataTables will now export csv & excel using `fast-excel` package.


## Faster export by disabling the fast-excel callback

1. Just set property `$fastExcelCallback = false`. This is enabled by default for a better formatted output of exported file.

```php
class UsersDataTable extends DataTable
{
    protected $fastExcel = true;
    protected $fastExcelCallback = false;

```

2. Exported file will now be based on how your query was structured. No header formatting and all selected columns in sql will be included in the output.

## Using custom callback

Just override the `fastExcelCallback` method:

```php
class UsersDataTable extends DataTable
{
    protected $fastExcel = true;

    public function fastExcelCallback()
    {
        return function ($row) {
            return [
                'Name' => $row['name'],
                'Email' => $row['email'],
            ];
        };
    }

...
```