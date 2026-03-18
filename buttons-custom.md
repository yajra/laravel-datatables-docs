# Custom Actions

You can enable custom actions on your buttons by overloading the `buttons` method in your DataTable class.

<a name="creating-custom-action"></a>
## Creating a Custom Action

```php
namespace App\DataTables;

use App\User;
use Yajra\DataTables\Html\Button;
use Yajra\DataTables\Services\DataTable;

class UsersDataTable extends DataTable
{
    protected array $actions = ['print', 'excel', 'myCustomAction'];

    public function html()
    {
        return $this->builder()
                    ->columns($this->getColumns())
                    ->layout([
                        'topStart' => 'buttons',
                        'topEnd' => 'search',
                        'bottomStart' => 'info',
                        'bottomEnd' => 'paging',
                    ])
                    ->buttons([
                        Button::make('print'),
                        Button::make('excel'),
                        Button::make('myCustomAction'),
                    ]);
    }

    public function myCustomAction()
    {
        //...your code here.
    }
}
```

Take a look at `Yajra\DataTables\Services\DataTable` to see how to fetch and manipulate data (see `excel`, `csv`, `pdf` methods).

<a name="see-also"></a>
## See Also

- [Buttons Console](buttons-console.md) - Artisan commands for generating DataTables
- [Buttons Export](buttons-export.md) - Export button options
- [Buttons Starter](buttons-starter.md) - Quick start guide
