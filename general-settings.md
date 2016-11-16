# General Settings

## Introduction
You can change the package behavior by updating the configuration file.

The configuration file can be found at `config/datatables.php`.

## Smart Search
Smart search will enclose search keyword with wildcard string `"%keyword%"`.
The sql generated will be like `column LIKE "%keyword%"` when set to `true`.

```php
'smart' => true,
```

## Case Sensitive Search
Case insensitive will search the keyword in lower case format.
The sql generated will be like `LOWER(column) LIKE LOWER(keyword)` when set to `true`.

```php
'case_insensitive' => true,
```

## Wild Card Search
Wild card will add `%` in between every characters of the keyword.
The sql generated will be like `column LIKE "%k%e%y%w%o%r%d%"` when set to `true`.

```php
'use_wildcards' => false,
```

## Engines
A list of available engines.
This is where you can register your custom datatables engine.

```php
'engines' => [
        'eloquent'   => Yajra\Datatables\Engines\EloquentEngine::class,
        'query'      => Yajra\Datatables\Engines\QueryBuilderEngine::class,
        'collection' => Yajra\Datatables\Engines\CollectionEngine::class,
        // add your custom engine
    ],
```

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

## Fractal Includes
Request key name to parse includes on fractal.

```php
'includes' => 'include',
```

## Fractal Serializer
Default fractal serializer to be used when serializing the response.

```php
'serializer' => League\Fractal\Serializer\DataArraySerializer::class,
```

