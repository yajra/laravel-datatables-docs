---
title: Black List Columns
description: Disable sorting and searching on specific columns
---


# Black Listing Columns

The blacklist feature allows you to explicitly disable sorting and searching on specific columns. Columns defined in the blacklist will be excluded from sorting and searching operations.

```php
use DataTables;

Route::get('user-data', function() {
	$model = App\User::query();

	return DataTables::eloquent($model)
				->blacklist(['password', 'name'])
				->toJson();
});
```
