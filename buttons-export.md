# DataTable Buttons

<a name="export"></a>
## Export Button Group

To enable export button group, set `export` on the buttons array.
Export button group includes `excel`, `csv` and `pdf` button.

```php
namespace App\DataTables;

use App\User;
use Yajra\Datatables\Services\DataTable;

class UsersDataTable extends DataTable
{
    //...some default stubs deleted for simplicity.

    public function html()
    {
        return $this->builder()
                    ->columns($this->getColumns())
                    ->parameters([
                        'buttons' => ['export'],
                    ]);
    }
...
```

<a name="excel"></a>
## Export as Excel

To enable exporting to excel, set `excel` on the buttons array.

```php
namespace App\DataTables;

use App\User;
use Yajra\Datatables\Services\DataTable;

class UsersDataTable extends DataTable
{
    //...some default stubs deleted for simplicity.

    public function html()
    {
        return $this->builder()
                    ->columns($this->getColumns())
                    ->parameters([
                        'buttons' => ['excel'],
                    ]);
    }
...
```

<a name="csv"></a>
## Export as CSV

To enable exporting to csv, set `csv` on the buttons array.

```php
namespace App\DataTables;

use App\User;
use Yajra\Datatables\Services\DataTable;

class UsersDataTable extends DataTable
{
    //...some default stubs deleted for simplicity.

    public function html()
    {
        return $this->builder()
                    ->columns($this->getColumns())
                    ->parameters([
                        'buttons' => ['csv'],
                    ]);
    }
...
```

<a name="pdf"></a>
## Export as PDF

To enable exporting to pdf, set `pdf` on the buttons array.

```php
namespace App\DataTables;

use App\User;
use Yajra\Datatables\Services\DataTable;

class UsersDataTable extends DataTable
{
    //...some default stubs deleted for simplicity.

    public function html()
    {
        return $this->builder()
                    ->columns($this->getColumns())
                    ->parameters([
                        'buttons' => ['pdf'],
                    ]);
    }
...
```

<a name="print"></a>
## Printable Version

To enable print button, set `print` on the buttons array.

```php
namespace App\DataTables;

use App\User;
use Yajra\Datatables\Services\DataTable;

class UsersDataTable extends DataTable
{
    //...some default stubs deleted for simplicity.

    public function html()
    {
        return $this->builder()
                    ->columns($this->getColumns())
                    ->parameters([
                        'buttons' => ['print'],
                    ]);
    }
...
```