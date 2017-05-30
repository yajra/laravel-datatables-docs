# Sending parameter to DataTable class
You can send a parameter from controller to dataTable class using `with` api.


## Example:

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

You can then get the variable as a local property of the class.

```php
class UsersDataTable {
    public function query() {
        return User::where('id', $this->id);
    }
}
```
