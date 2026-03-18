---
title: White List Columns
description: Enable sorting and searching only on specific columns
---


# White Listing Columns

The whitelist feature allows you to explicitly define which columns can be used for sorting and searching. Only columns defined in the whitelist will be searchable and sortable.

```php
use DataTables;

Route::get('user-data', function() {
	$model = App\User::query();

	return DataTables::eloquent($model)
				->whitelist(['name', 'email'])
				->toJson();
});
```
