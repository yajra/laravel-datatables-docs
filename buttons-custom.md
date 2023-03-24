# Custom Actions

You can enable custom actions on your buttons as follows:

Update `UsersDataTable` class and overload the `actions` property. Here we are
disabling the `csv` and `pdf` actions (so they cannot be fired by hijacking their
request) and enabling a `myCustomAction`.


```php
namespace App\DataTables;

use App\User;
use Yajra\DataTables\Services\DataTable;

class UsersDataTable extends DataTable
{
    protected array $actions = ['print', 'excel', 'myCustomAction'];

    public function html()
    {
        return $this->builder()
                    ->columns($this->getColumns())
                    ->dom('Bfrtip')
                    ->buttons([
                        'print',
                        'excel',
                        'myCustomAction',
                    ]);
    }

    public function myCustomAction()
    {
        //...your code here.
    }

}
```

Take a look at `Yajra\DataTables\Services\DataTable` to see how to fetch and manipulate the data (functions `excel`, `csv`, `pdf`).
