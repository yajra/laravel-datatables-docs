---
title: Raw Columns
description: Allow HTML content in DataTables columns
---

# Raw Columns

By default, Laravel DataTables protects you from XSS attacks by escaping all outputs. In cases where you want to render HTML content, use the `rawColumns` API.

---

<a name="basic"></a>
## Basic Usage

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::query();

    return DataTables::eloquent($model)
        ->addColumn('link', '<a href="#">Html Column</a>')
        ->addColumn('action', 'path.to.view')
        ->rawColumns(['link', 'action'])
        ->toJson();
});
```

---

<a name="common-use-cases"></a>
## Common Use Cases

### Action Buttons

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    return DataTables::eloquent(User::query())
        ->addColumn('action', function (User $user) {
            return '<a href="/users/'.$user->id.'/edit" class="btn btn-sm btn-primary">Edit</a>';
        })
        ->rawColumns(['action'])
        ->toJson();
});
```

### Status Badges

```php
Route::get('user-data', function() {
    return DataTables::eloquent(User::query())
        ->addColumn('status', function (User $user) {
            $class = $user->is_active ? 'bg-success' : 'bg-secondary';
            return '<span class="badge '.$class.'">'.$user->status.'</span>';
        })
        ->rawColumns(['status'])
        ->toJson();
});
```

### Links with Data

```php
Route::get('post-data', function() {
    return DataTables::eloquent(Post::query())
        ->addColumn('title_link', function (Post $post) {
            return '<a href="/posts/'.$post->id.'">'.$post->title.'</a>';
        })
        ->rawColumns(['title_link'])
        ->toJson();
});
```

---

## Security Warning

> [!WARNING]
> Only use `rawColumns` with trusted data. Never pass user input directly to HTML output without proper sanitization.

```php
// ⚠️ DANGEROUS - User input is not sanitized
->addColumn('comment', '<div>'. $user->comment .'</div>')

// ✅ SAFE - Use HTML escaping
->addColumn('comment', '<div>'. e($user->comment) .'</div>')
```

---

## See Also

- [Add Column](/docs/{{package}}/{{version}}/add-column) - Adding custom columns
- [Edit Column](/docs/{{package}}/{{version}}/edit-column) - Modifying existing columns
- [Security](/docs/{{package}}/{{version}}/security) - XSS protection best practices
