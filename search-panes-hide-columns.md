# Exclude Columns

Some columns you might not want in your SearchPanes, to hide them you can add `->searchPanes(false)` in your column
definition:

```php
protected function getColumns() : array
{
    return [
        Column::make('id')
            ->searchPanes(false);
    ]
}
```