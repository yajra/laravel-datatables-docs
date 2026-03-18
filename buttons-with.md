# Sending Parameter to DataTable Class

You can send a parameter from controller to DataTable class using the `with` API.

<a name="example"></a>
## Example

```php
Route::get('datatable/{id}', function(UsersDataTable $dataTable, $id){
   return $dataTable->with('id', $id)
           ->with([
                'key2' => 'value2',
                'key3' => 'value3',
           ])
           ->render('path.to.view');
});
```

You can then access the variable as a class property:

```php
class UsersDataTable extends DataTable
{
    public function query()
    {
        return User::where('id', $this->id);
    }
}
```
