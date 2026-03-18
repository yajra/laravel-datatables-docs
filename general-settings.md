---
title: General Settings
description: Configure Laravel DataTables behavior with configuration options
---

# General Settings

You can change the package behavior by updating the configuration file. The configuration file can be found at `config/datatables.php`.

---

<a name="smart"></a>
## Smart Search

Smart search will enclose search keyword with wildcard string `"%keyword%"`. The SQL generated will be like `column LIKE "%keyword%"` when set to `true`.

```php
'smart' => true,
```

---

## Case Sensitive Search

Case insensitive will search the keyword in lower case format. The SQL generated will be like `LOWER(column) LIKE LOWER(keyword)` when set to `true`.

```php
'case_insensitive' => true,
```

---

## Wild Card Search

Wild card will add `%` in between every characters of the keyword. The SQL generated will be like `column LIKE "%k%e%y%w%o%r%d%"` when set to `true`.

```php
'use_wildcards' => false,
```

---

## Index Column

DataTables internal index id response column name:

```php
'index_column' => 'DT_RowIndex',
```

---

## Engines

A list of available engines. This is where you can register your custom DataTables engine:

```php
'engines' => [
    'eloquent'   => Yajra\DataTables\Engines\EloquentEngine::class,
    'query'      => Yajra\DataTables\Engines\QueryBuilderEngine::class,
    'collection' => Yajra\DataTables\Engines\CollectionEngine::class,
    // add your custom engine
],
```

---

## Builders

A list of accepted data source / builders of DataTables with their corresponding engine handler:

```php
'builders' => [
    Illuminate\Database\Eloquent\Relations\HasMany::class => 'eloquent',
    Illuminate\Database\Eloquent\Builder::class           => 'eloquent',
    Illuminate\Database\Query\Builder::class              => 'query',
    Illuminate\Support\Collection::class                  => 'collection',
    // add your data source to custom engine handler
],
```

---

## Fractal Includes

Request key name to parse includes on fractal:

```php
'includes' => 'include',
```

---

## Fractal Serializer

Default fractal serializer to be used when serializing the response:

```php
'serializer' => League\Fractal\Serializer\DataArraySerializer::class,
```

---

## Script Template

DataTables HTML builder script blade template. If published, the file will be installed on `resources/views/vendor/datatables/script.blade.php`:

```php
'script_template' => 'datatables::script',
```

---

## NULLS LAST SQL

Nulls last SQL pattern for PostgreSQL & Oracle:

> [!TIP]
> For MySQL, use `'-%s %s'` as the SQL pattern.

```php
'nulls_last_sql' => '%s %s NULLS LAST',
```

---

## See Also

- [Installation](/docs/{{package}}/{{version}}/installation) - Install and configure the package
- [Eloquent Data Source](/docs/{{package}}/{{version}}/engine-eloquent) - Using Eloquent engine
