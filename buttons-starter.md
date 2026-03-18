# Buttons Quick Starter

<a name="create-users-datatable"></a>
## Create Users DataTable

```bash
php artisan datatables:make Users
```

<a name="update-users-datatable"></a>
## Update UsersDataTable

Update `UsersDataTable` class and set the columns and parameters needed to render our dataTable.

```php
namespace App\DataTables;

use App\User;
use Yajra\DataTables\Html\Column;
use Yajra\DataTables\Html\Button;
use Yajra\DataTables\Services\DataTable;

class UsersDataTable extends DataTable
{
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
                        Button::make('export'),
                        Button::make('print'),
                        Button::make('reset'),
                        Button::make('reload'),
                    ]);
    }

    protected function getColumns(): array
    {
        return [
            Column::make('id'),
            Column::make('name'),
            Column::make('email'),
            Column::make('created_at'),
            Column::make('updated_at'),
        ];
    }
}
```

<a name="example-route"></a>
## Example Route

```php
use App\DataTables\UsersDataTable;

Route::get('users', function(UsersDataTable $dataTable) {
    return $dataTable->render('users.index');
});
```

<a name="example-view"></a>
## Example View

Our `users.index` view located at `resources/views/users/index.blade.php`.

```php
@extends('app')

@section('content')
{!! $dataTable->table() !!}
@endsection

@push('scripts')
{!! $dataTable->scripts() !!}
@endpush
```
