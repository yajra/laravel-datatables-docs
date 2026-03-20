---
title: Minimum Search Length
description: Set a minimum character requirement for DataTables search
---

# Minimum Search Length

The `minSearchLength()` API enforces a minimum number of characters before search is executed, preventing excessive queries from short search terms.

---

<a name="basic"></a>
## Basic Usage

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    return DataTables::eloquent(User::query())
        ->minSearchLength(3)
        ->toJson();
});
```

---

<a name="how-it-works"></a>
## How It Works

When a user enters fewer characters than the minimum:

1. `recordsTotal` and `recordsFiltered` are set to `0`
2. An exception is thrown with a localized error message
3. The response is handled by the [Error Handler](/docs/{{package}}/{{version}}/error-handler) configuration

---

<a name="custom-message"></a>
## Custom Error Message

The error message uses Laravel's localization system. Publish the language files:

```bash
php artisan vendor:publish --tag=datatables-lang
```

Then customize the message in `resources/lang/{locale}/datatables.php`:

```php
return [
    'min_search_length' => 'Please enter at least :length characters to search.',
];
```

---

<a name="example"></a>
## Example

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\Post;

Route::get('post-data', function() {
    return DataTables::eloquent(Post::query())
        ->minSearchLength(4)  // Require at least 4 characters
        ->toJson();
});
```

If a user searches for "php" (3 characters), the DataTable will show an empty result with the configured error message.

---

## See Also

- [Manual Search](/docs/{{package}}/{{version}}/manual-search) - Custom search logic
- [Smart Search](/docs/{{package}}/{{version}}/smart-search) - Wildcard search configuration
- [Error Handler](/docs/{{package}}/{{version}}/error-handler) - Error handling configuration
