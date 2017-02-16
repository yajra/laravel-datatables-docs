# Extended DataTable

We can now extend and reuse our DataTable class inside our controller by using `before` and `response` callback.

## Quick Example:
```php
Route::get('datatable', function(RolesDataTable $dataTable){
   return $dataTable->before(function (Yajra\Datatables\Engines\BaseEngine $dataTable) {
       return $dataTable->addColumn('test', 'added inside controller');
   })->response(function (Illuminate\Support\Collection $response) {
       $response['test'] = 'Append Data';

       return $response;
   })->render('path.to.view');
});
```

