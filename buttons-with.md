---
title: Buttons With Parameters
description: Pass parameters to DataTable class from controller
---

# Sending Parameter to DataTable Class

You can send a parameter from controller to DataTable class using the `with` API.

---

<a name="basic"></a>
## Basic Usage

```php
<?php
// routes/web.php

use App\DataTables\UsersDataTable;
use Illuminate\Support\Facades\Route;

Route::get('datatable/{id}', function(UsersDataTable $dataTable, $id) {
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
<?php
// app/DataTables/UsersDataTable.php

namespace App\DataTables;

use App\Models\User;
use Yajra\DataTables\Services\DataTable;

class UsersDataTable extends DataTable
{
    public function query()
    {
        return User::where('id', $this->id);
    }
}
```

---

## Common Use Cases

### Filter by Category

```php
Route::get('posts/{category}', function(PostsDataTable $dataTable, $category) {
    return $dataTable->with('category', $category)
        ->render('posts.index');
});
```

```php
public function query()
{
    return Post::where('category', $this->category);
}
```

### Filter by Status

```php
Route::get('users/{status}', function(UsersDataTable $dataTable, $status) {
    return $dataTable->with('status', $status)
        ->render('users.index');
});
```

---

## See Also

- [Extended DataTable](/docs/{{package}}/{{version}}/buttons-extended) - More advanced usage
- [Buttons Installation](/docs/{{package}}/{{version}}/buttons-installation) - Install buttons plugin
