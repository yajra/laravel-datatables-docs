# DataTable Service Quick Starter

## Create Users DataTable

```
php artisan datatables:make Users
```

## Update UsersDataTable

Update `UsersDataTable` class and set the columns and parameters needed to render our dataTable.


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
                        'dom'          => 'Bfrtip',
                        'buttons'      => ['export', 'print', 'reset', 'reload'],
                    ]);
    }

    protected function getColumns()
    {
        return [
            'id',
            'name',
            'email',
            'created_at',
            'updated_at',
        ];
    }
}
```

## Example Route:

```php
use App\DataTables\UsersDataTable;

Route::get('users', function getUsers(UsersDataTable $dataTable)
{
    return $dataTable->render('users.index');
});
```


## Example View:

Our `users.index` view located at `resources/views/users/index.blade.php`.

```php
@extends('app')

@section('content')
{!! $dataTable->table() !!}
@endsection

@push('scripts')
<link rel="stylesheet" href="https://cdn.datatables.net/buttons/1.0.3/css/buttons.dataTables.min.css">
<script src="https://cdn.datatables.net/buttons/1.0.3/js/dataTables.buttons.min.js"></script>
<script src="/vendor/datatables/buttons.server-side.js"></script>
{!! $dataTable->scripts() !!}
@endpush
```