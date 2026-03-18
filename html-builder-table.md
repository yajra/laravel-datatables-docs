---
title: HTML Builder Table
description: Generate HTML table markup with the table() method
---

# HTML Builder Table

The `table()` method generates the HTML table markup for your DataTable.

---

## Basic Usage

### Simple Table

```php
use Yajra\DataTables\Html\Builder;

echo $html->table();
```

### Table with Custom Attributes

```php
use Yajra\DataTables\Html\Builder;

echo $html->table([
    'class' => 'table table-striped table-bordered',
    'id' => 'users-table',
]);
```

### Table with Footer

```php
use Yajra\DataTables\Html\Builder;

echo $html->table(['class' => 'table'], true);
```

### Table with Search Headers

```php
use Yajra\DataTables\Html\Builder;

echo $html->table([], false, true);
```

---

## Method Signature

```php
public function table(array $attributes = [], bool $drawFooter = false, bool $drawSearch = false): HtmlString
```

### Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `$attributes` | array | `[]` | HTML table attributes |
| `$drawFooter` | bool | `false` | Whether to draw `<tfoot>` |
| `$drawSearch` | bool | `false` | Whether to draw search filter row |

---

## Complete Example

```php
use Yajra\DataTables\Facades\DataTables;
use Yajra\DataTables\Html\Builder;
use Yajra\DataTables\Html\Column;
use App\Models\User;

Route::get('users', function(Builder $builder) {
    if (request()->ajax()) {
        return DataTables::of(User::query())->toJson();
    }

    $html = $builder->columns([
        Column::make('id'),
        Column::make('name'),
        Column::make('email'),
    ]);

    return view('users.index', compact('html'));
});
```

In your Blade template:

```blade
@extends('layouts.app')

@section('content')
    {{ $html->table(['class' => 'table table-bordered']) }}
@endsection

@push('scripts')
    {{ $html->scripts() }}
@endpush
```

---

## See Also

- [HTML Builder](/docs/{{package}}/{{version}}/html-builder) - Main HTML Builder documentation
- [HTML Builder Column](/docs/{{package}}/{{version}}/html-builder-column) - Column configuration
