# Extended DataTable

We can extend and reuse our DataTable class inside our controller by using `before` and `response` callback.

<a name="quick-example"></a>
## Quick Example

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
