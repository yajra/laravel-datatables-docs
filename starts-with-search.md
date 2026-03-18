---
title: Starts With Search
description: Search column values that start with
---


# Starts With Search

Starts with search feature allows the package to search our tables with records that starts with the given keyword.
To enable the feature, just chain `->startsWithSearch()` on your instance.

```php
return datatables()
        ->eloquent($model)
		->startsWithSearch()
		->toJson();
```
