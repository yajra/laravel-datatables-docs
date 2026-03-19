---
title: General Settings
description: Configure Laravel DataTables behavior with configuration options
---

# General Settings

You can customize Laravel DataTables by updating the configuration file. The configuration file is published to `config/datatables.php` when you run:

```bash
php artisan vendor:publish --tag=datatables
```

---

<a name="smart"></a>
## Smart Search

Smart search automatically encloses the search keyword with wildcards (`"%keyword%"`). When enabled, the generated SQL looks like:

```sql
column LIKE "%keyword%"
```

```php
'smart' => true,
```

---

<a name="case-insensitive"></a>
## Case Insensitive Search

Case insensitive search converts both the column values and search keyword to lowercase before comparison. The generated SQL looks like:

```sql
LOWER(column) LIKE LOWER('%keyword%')
```

```php
'case_insensitive' => true,
```

---

<a name="wildcard"></a>
## Wildcard Search

Wildcard search adds `%` between every character of the keyword. The generated SQL looks like:

```sql
column LIKE "%k%e%y%w%o%r%d%"
```

```php
'use_wildcards' => false,
```

> [!TIP]
> When using wildcards with `smart` search, you only need to add a trailing `%`: `%keyword%`

---

<a name="index-column"></a>
## Index Column

The index column name used for DataTables internal row numbering:

```php
'index_column' => 'DT_RowIndex',
```

---

<a name="error"></a>
## Error Handling

Configure how DataTables handles server-side errors. See the [Error Handler documentation](/docs/{{package}}/{{version}}/error-handler) for full details.

```php
'error' => env('DATATABLES_ERROR', null),
```

**Available options:**
- `null` - Display the actual exception message
- `'throw'` - Throw a `\Yajra\DataTables\Exception`
- Custom string - Display a custom friendly message
- Translation key - Use a translation key for localization

---

<a name="nulls-last"></a>
## NULLS LAST SQL

SQL pattern for ordering NULL values last, used by PostgreSQL & Oracle databases:

```php
'nulls_last_sql' => '%s %s NULLS LAST',
```

> [!TIP]
> For MySQL, use `'-%s %s'` as the SQL pattern to push NULLs to the end.

---

<a name="engines"></a>
## Engines (Advanced)

A list of available DataTables engines. You can register custom engines here:

```php
'engines' => [
    'eloquent'   => Yajra\DataTables\Engines\EloquentEngine::class,
    'query'      => Yajra\DataTables\Engines\QueryBuilderEngine::class,
    'collection' => Yajra\DataTables\Engines\CollectionEngine::class,
],
```

> [!NOTE]
> Most users don't need to modify engine settings. Only change these if you're creating a custom DataTables engine.

---

<a name="builders"></a>
## Builders (Advanced)

Maps data source types to their corresponding engines:

```php
'builders' => [
    Illuminate\Database\Eloquent\Relations\HasMany::class => 'eloquent',
    Illuminate\Database\Eloquent\Builder::class           => 'eloquent',
    Illuminate\Database\Query\Builder::class              => 'query',
    Illuminate\Support\Collection::class                  => 'collection',
],
```

---

## Complete Configuration Reference

Here's a complete example of the `config/datatables.php` file:

```php
<?php

return [
    /*
     * Smart search adds wildcards automatically
     */
    'smart' => true,

    /*
     * Case insensitive searching
     */
    'case_insensitive' => true,

    /*
     * Wild card character for partial matching
     */
    'use_wildcards' => false,

    /*
     * DataTables internal index column name
     */
    'index_column' => 'DT_RowIndex',

    /*
     * Error message configuration
     */
    'error' => env('DATATABLES_ERROR', null),

    /*
     * NULLS LAST SQL pattern for PostgreSQL & Oracle
     */
    'nulls_last_sql' => '%s %s NULLS LAST',
];
```

---

## See Also

- [Installation](/docs/{{package}}/{{version}}/installation) - Install and configure the package
- [Error Handler](/docs/{{package}}/{{version}}/error-handler) - Configure error handling
- [Eloquent Data Source](/docs/{{package}}/{{version}}/engine-eloquent) - Using Eloquent engine
