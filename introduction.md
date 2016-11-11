# Introduction

## jQuery DataTables API for Laravel
This package is created to handle server-side works of DataTables jQuery Plugin via AJAX option by using the following engines:
- Eloquent ORM
- Fluent Query Builder
- Collection.

```php
use Yajra\Datatables\Facades\Datatables;

// Using Eloquent
return Datatables::eloquent(User::query())->make(true);

// Using Query Builder
return Datatables::queryBuilder(DB::table('users'))->make(true);

// Using Collection
return Datatables::collection(User::all())->make(true);

// Using the Engine Factory
return Datatables::of(User::query())->make(true);
return Datatables::of(DB::table('users'))->make(true);
return Datatables::of(User::all())->make(true);
```