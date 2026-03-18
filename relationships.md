---
title: Eager Loading Relationships
description: Search and sort eager loaded relationships in DataTables
---

# Eager Loading Relationships

DataTables supports searching and sorting of eager loaded relationships when using `Eloquent`. This example shows how to setup eager loading search using the `EloquentEngine`.

---

## Basic Setup

To enable search on relationships, we need to eager load the relationship using Laravel's `with()` API:

```php
use Yajra\DataTables\Facades\DataTables;
use Illuminate\Support\Str;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::with('posts');

    return DataTables::eloquent($model)
        ->addColumn('posts', function (User $user) {
            return $user->posts->map(function($post) {
                return Str::limit($post->title, 30, '...');
            })->implode('<br>');
        })
        ->toJson();
});
```

---

## Searching Relationship Columns

To trigger search on `posts` relationship, specify the `relation.column_name` as the `name` attribute in your JavaScript:

```javascript
$(document).ready(function() {
    $('#users-table').DataTable({
        processing: true,
        serverSide: true,
        ajax: '{{ url("collection/basic-object-data") }}',
        columns: [
            {data: 'id', name: 'id'},
            {data: 'name', name: 'name'},
            {data: 'email', name: 'email'},
            {data: 'posts', name: 'posts.title'},
            {data: 'created_at', name: 'created_at'},
            {data: 'updated_at', name: 'updated_at'}
        ]
    });
});
```

> [!NOTE]
> When using `{data: 'posts', name: 'posts.title'}`:
> - `data: posts` represents the data key (`data.posts`) that will be displayed on the table.
> - `name: posts.title` represents the `User` model relationship (`posts`) and the column to perform the search (`title`).

---

## Using Table Aliases

> [!WARNING]
> It is advised that you include `select('table.*')` on your query to avoid issues where the id from a related model replaces the id of the main model.

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;
use App\Models\Post;

Route::get('user-data', function() {
    // Use table alias to prevent id conflicts
    $model = Post::with('user')->select('posts.*');

    return DataTables::eloquent($model)->toJson();
});
```

---

## Nested Relationships

> [!NOTE]
> Same strategy goes for nested relationships, but ordering is not yet fully tested on nested relationships.

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::with(['posts.comments', 'roles']);

    return DataTables::eloquent($model)->toJson();
});
```

---

## See Also

- [Eloquent Data Source](/docs/{{package}}/{{version}}/engine-eloquent) - Basic Eloquent usage
- [Filter Column](/docs/{{package}}/{{version}}/filter-column) - Custom column filtering
- [Add Column](/docs/{{package}}/{{version}}/add-column) - Adding custom columns
