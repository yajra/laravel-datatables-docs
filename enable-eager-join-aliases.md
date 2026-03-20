---
title: Enable Eager Join Aliases
description: Enable unique table aliases for eager loaded relationship joins
---

# Enable Eager Join Aliases

The `enableEagerJoinAliases()` API enables the generation of unique table aliases when joining eagerly loaded relationships. This solves the "Not unique table/alias" error when performing searches or ordering on columns from relationships.

---

<a name="basic"></a>
## Basic Usage

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\Post;

Route::get('post-data', function() {
    return DataTables::eloquent(
        Post::with(['author', 'comments', 'tags'])
    )
        ->enableEagerJoinAliases()
        ->toJson();
});
```

---

<a name="problem"></a>
## The Problem

When you have nested eager-loaded relationships and try to search/sort on relationship columns, you may encounter:

```
SQLSTATE[42000]: Syntax error or access violation: 1066 Not unique table/alias: 'users'
```

This happens because multiple joins reference the same table without unique aliases.

---

<a name="solution"></a>
## The Solution

`enableEagerJoinAliases()` generates unique aliases for each joined table:

```php
// Without aliases - causes conflicts
users
users AS users_author
users AS users_comments_pivot
tags AS tags_posts_pivot
posts AS posts_author
```

---

<a name="supported-relations"></a>
## Supported Relations

This feature works with:

- `HasOne`
- `HasMany`
- `BelongsTo`
- `BelongsToMany`
- `HasOneThrough`

---

<a name="example"></a>
## Example with Multiple Relations

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    // Eager load all relationships
    $model = User::with([
        'posts.comments',      // Nested: posts -> comments
        'roles',               // Direct: roles
        'profile',             // HasOne: profile
        'teams.members',        // BelongsToMany: teams -> members
    ]);

    return DataTables::eloquent($model)
        ->enableEagerJoinAliases()
        ->toJson();
});
```

---

## See Also

- [Search Eloquent Relationships](/docs/{{package}}/{{version}}/relationships) - Basic relationship searching
- [Order Column](/docs/{{package}}/{{version}}/order-column) - Custom column ordering
