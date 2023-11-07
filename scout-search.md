# Scout Search

**Note:** This documentation is applicable from version 10.11.0 and upwards, where external search engines can be integrated with Laravel Scout, replacing the built-in search functionality.

Scout Search provides a fallback mechanism, so the built-in search will be used if any issues occur with the external search engines (e.g., server problems or indexing problems).

Supported drivers: Meilisearch, Algolia

<a name="setup"></a>

## Setup

To start using Scout Search, you need to install and configure <a href="https://laravel.com/docs/10.x/scout#installation">Laravel Scout</a> with your preferred driver.

Once Laravel Scout is set up, you can enable Scout Search in your `dataTable` function like this:

```php
return (new EloquentDataTable($query))
    // Enable scout search for eloquent model
    ->enableScoutSearch(Product::class)
    
    // Add filters to scout search
    ->scoutFilter(function (string $keyword) {
        return 'region IN ["Germany", "France"]'; // Meilisearch
        // or
        return 'region:Germany OR region:France'; // Algolia
    })
    
    // Add filters to default search
    ->filter(function (QueryBuilder $query, bool $scout_searched) {
        if (!$scout_searched)
        {
            // Filter already added for scout search
            $query->whereIn('region', ['Germany', 'France']);
        }

        // Stock is not indexed so it has to be filtered after the initial scout search
        $query->where('stock', '>', 50);
    }, true);
```

<a name="additional-js"></a>

## Additional JS script

To control the visibility of sort icons on the frontend table, you'll need an additional JavaScript script. This script hides the sort icons when Scout Search is successful (indicating fixed ordering by search relevance) and shows them again when the search keyword is removed.

If you are using the `laravel-datatables-html` package, you can easily include the script in your `html` function:

```php
return $this->builder()
    ->setTableId('products-table')
    ->addScript('datatables::scout');
```

Alternatively, you can manually fetch and include the JavaScript script into your own code from this file: <a href="https://github.com/yajra/laravel-datatables-html/blob/master/src/resources/views/scout.blade.php">resources/views/scout.blade.php</a>
