# General Settings

## Introduction
You can change the package behavior by updating the configuration file.

The configuration file can be found at `config/datatables.php`.

<a name="smart-search"></a>
## Smart Search
Smart search will enclose search keyword with wildcard string `"%keyword%"`.
The sql generated will be like `column LIKE "%keyword%"` when set to `true`.

```php
'smart' => true,
```

<a name="case-sensitivity"></a>
## Case Sensitive Search
Case insensitive will search the keyword in lower case format.
The sql generated will be like `LOWER(column) LIKE LOWER(keyword)` when set to `true`.

```php
'case_insensitive' => true,
```

<a name="wild-card"></a>
## Wild Card Search
Wild card will add `%` in between every characters of the keyword.
The sql generated will be like `column LIKE "%k%e%y%w%o%r%d%"` when set to `true`.

```php
'use_wildcards' => false,
```

<a name="index-column"></a>
## Index Column
DataTables internal index id response column name.

```php
'index_column' => 'DT_RowIndex',
```

<a name="engines"></a>
## Engines
A list of available engines.
This is where you can register your custom datatables engine.

```php
'engines' => [
        'eloquent'   => Yajra\DataTables\Engines\EloquentEngine::class,
        'query'      => Yajra\DataTables\Engines\QueryBuilderEngine::class,
        'collection' => Yajra\DataTables\Engines\CollectionEngine::class,
        // add your custom engine
    ],
```

<a name="builders"></a>
## Builders
A list of accepted data source / builders of datatables with their corresponding engine handler.

```php
'builders' => [
        Illuminate\Database\Eloquent\Relations\HasMany::class => 'eloquent',
        Illuminate\Database\Eloquent\Builder::class           => 'eloquent',
        Illuminate\Database\Query\Builder::class              => 'query',
        Illuminate\Support\Collection::class                  => 'collection',
        // add your data source to custom engine handler
    ],
```

<a name="fractal-includes"></a>
## Fractal Includes
Request key name to parse includes on fractal.

```php
'includes' => 'include',
```

<a name="fractal-serializer"></a>
## Fractal Serializer
Default fractal serializer to be used when serializing the response.

```php
'serializer' => League\Fractal\Serializer\DataArraySerializer::class,
```
<a name="script-template"></a>
## Script Template
DataTables html builder script blade template.
If published, the file will installed on `resources/views/vendor/datatables/script.blade.php`.

```php
'script_template' => 'datatables::script',
```

<a name="nulls-last-sql"></a>
## NULLS LAST sql
Nulls last sql pattern for Postgresql & Oracle.

> {tip} For MySQL, use '-%s %s' as the sql pattern.

```php
'nulls_last_sql' => '%s %s NULLS LAST',
```

