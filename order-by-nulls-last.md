---
title: Order By Nulls Last
description: Order null values at the end of results
---

# Order By Nulls Last

This API will set DataTables to perform ordering with `NULLS LAST` option.

---

## Basic Usage

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    $model = User::query();

    return DataTables::eloquent($model)
        ->orderByNullsLast()
        ->toJson();
});
```

---

## Configuration

> [!IMPORTANT]
> You must update the [nulls_last_sql](/docs/{{package}}/{{version}}/general-settings#nulls-last-sql) config to use a pattern that matches your database driver.

### MySQL Configuration

MySQL treats NULL values as smaller than non-NULL values, so NULLS LAST is the default behavior.

```php
// config/datatables.php
'nulls_last_sql' => '%s %s',
```

### PostgreSQL Configuration

```php
// config/datatables.php
'nulls_last_sql' => '%s %s NULLS LAST',
```

### Oracle Configuration

```php
// config/datatables.php
'nulls_last_sql' => '%s %s NULLS LAST',
```

---

## See Also

- [Order Column](/docs/{{package}}/{{version}}/order-column) - Single column ordering
- [General Settings](/docs/{{package}}/{{version}}/general-settings) - Configuration reference
