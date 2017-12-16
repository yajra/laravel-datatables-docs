# DataTables Editor Model

DataTables Editor requires a `Eloquent Model` that will be used for our CRUD operations.

> All CRUD operations of Editor uses database transaction.

<a name="setup"></a>
## Setup Model

Just set the `$model` property of your editor class to your model's FQCN.

```php
namespace App\DataTables\Editors;

use App\User;
use Yajra\DataTables\DataTablesEditor;

class UsersDataTablesEditor extends DataTablesEditor
{
    protected $model = User::class;
}
