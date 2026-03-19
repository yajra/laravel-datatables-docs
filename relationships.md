---
title: Search Eloquent Relationships
description: Enable search and sort on eager loaded relationships in DataTables
---

# Search Eloquent Relationships

DataTables supports searching and sorting on eager loaded Eloquent relationships using dot notation.

---

## Setup and Searching

### Backend: Eager Load Relationships

Use Laravel's `with()` to eager load relationships:

```php
Route::get('user-data', function () {
    return DataTables::eloquent(User::with('posts'))
        ->addColumn('posts', fn (User $user) =>
            $user->posts
                ->map(fn($post) => Str::limit($post->title, 30, '...'))
                ->implode('<br>')
        )
        ->toJson();
});
```

### Frontend: Use Dot Notation for Searching

Use `relation.column` syntax in the `name` attribute:

```javascript
$('#users-table').DataTable({
    processing: true,
    serverSide: true,
    ajax: '/user-data',
    columns: [
        { data: 'id', name: 'id' },
        { data: 'name', name: 'name' },
        { data: 'email', name: 'email' },
        { data: 'posts', name: 'posts.title' },
        { data: 'created_at', name: 'created_at' }
    ]
});
```

> [!NOTE]
> - `data: 'posts'` - display key shown in the table
> - `name: 'posts.title'` - relationship.column for searching/sorting

---

## Table Aliases

> [!WARNING]
> Include `select('table.*')` in your query to prevent id column conflicts from related models.

```php
Route::get('user-data', function () {
    // Use table alias to prevent id conflicts
    $model = Post::with('user')->select('posts.*');

    return DataTables::eloquent($model)->toJson();
});
```

---

## Nested Relationships

Search through deeply nested relationships:

```php
Route::get('user-data', function () {
    $model = User::with(['posts.comments', 'roles']);

    return DataTables::eloquent($model)->toJson();
});
```

In JavaScript, reference nested columns with dot notation:

```javascript
{ data: 'comments', name: 'posts.comments.content' }
```

> [!WARNING]
> Nested relationship **sorting** may have limitations. Searching is fully supported.

---

## See Also

- [Eloquent Engine](/docs/{{package}}/{{version}}/engine-eloquent) - Basic Eloquent usage
- [Filter Column](/docs/{{package}}/{{version}}/filter-column) - Custom column filtering
- [Add Column](/docs/{{package}}/{{version}}/add-column) - Adding custom columns
