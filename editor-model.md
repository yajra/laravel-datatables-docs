# DataTables Editor Model

DataTables Editor requires a `Eloquent Model` that will be used for our CRUD operations.

> {tip} All CRUD operations of Editor uses database transaction.

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
```

## Fillable Property

Don't forget to set your model's fillable property. The Editor's basic crud operation relies on this.
For advance operations like saving relations, use the [Editors Event Hooks](/docs/{{package}}/{{version}}/editor-events).

```php
class User extends Model {
    protected $fillable = [
        'name',
        'email',
        'password',
    ];
}
```