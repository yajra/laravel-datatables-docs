---
title: Skip Paging
description: Disable pagination for DataTables
---

# Skip Paging

To skip paging of DataTables, we can use the `skipPaging` API or just set `paging: false` in our JavaScript.

---

## Using PHP

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::query();

    return DataTables::eloquent($model)
        ->skipPaging()
        ->toJson();
});
```

---

## Using JavaScript

```javascript
$(document).ready(function() {
    $('#example').DataTable({
        serverSide: true,
        processing: true,
        ajax: '',
        paging: false
    });
});
```

---

## Use Cases

| Use Case | Recommendation |
|----------|----------------|
| Small datasets | ✅ Use skip paging |
| Export all records | ✅ Use skip paging |
| Large datasets | ❌ Use pagination |
| Performance optimization | ❌ Use pagination |

---

## See Also

- [General Settings](/docs/{{package}}/{{version}}/general-settings) - Configuration options
- [Quick Starter](/docs/{{package}}/{{version}}/quick-starter) - Getting started guide
