# Extended DataTable

We can now extend and reuse our DataTable class inside our controller by using `before` and `response` callback.

> IMPORTANT: Extended DataTable is only applicable on `^1.1` and above.


## Upgrading from v1.0 to v1.1
- Upgrade to `laravel-datatables-buttons:^1.1`
- Rename `ajax()` method to `dataTable()`
- Remove `->toJson()` from the method chain.

```php
    public function ajax()
    {
        return $this->datatables
            ->eloquent($this->query())
            ->addColumn('action', 'path.to.action.view')
            ->toJson()
    }
```

TO


```php
    public function dataTable()
    {
        return $this->datatables
            ->eloquent($this->query())
            ->addColumn('action', 'path.to.action.view');
    }
```

## Quick Example:
```php
Route::get('datatable', function(RolesDataTable $dataTable){
   return $dataTable->before(function (\Yajra\DataTables\DataTableAbstract $dataTable) {
       return $dataTable->addColumn('test', 'added inside controller');
   })
   ->response(function (\Illuminate\Support\Collection $response) {
       $response['test'] = 'Append Data';

       return $response;
   })
   ->withHtml(function(\Yajra\DataTables\Html\Builder $builder) {
        $builder->columns(['id', 'name', 'etc...']);
   })
   ->with('key', 'value')
   ->with([
    'key2' => 'value2',
    'key3' => 'value3',
   ])
   ->render('path.to.view');
});
```
