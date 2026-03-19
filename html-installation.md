---
title: HTML Builder Installation
description: Install and configure the Laravel DataTables HTML Builder plugin
---


# HTML Builder Installation

The HTML Builder plugin provides a fluent interface for generating DataTables HTML markup and JavaScript configuration.

<a name="overview"></a>
## Overview

The HTML Builder allows you to:

- **Generate Table HTML** - Create table markup with columns configuration
- **Configure DataTables** - Set up pagination, searching, ordering
- **Build JavaScript** - Auto-generate client-side DataTables initialization
- **Add Buttons** - Include export, print, and custom buttons

---

<a name="installation"></a>
## Installation

Install the HTML Builder package:

```bash
composer require yajra/laravel-datatables-html:"^13.0"
```

## Publish Configuration

Publish the configuration and assets:

```bash
php artisan vendor:publish --tag=datatables-html
```

This creates:
- `config/datatables-html.php` - Configuration file
- View templates for customization

---

## Basic Usage

### Defining Columns

```php
use Yajra\DataTables\Facades\DataTables;
use Yajra\DataTables\Html\Builder;
use Yajra\DataTables\Html\Column;
use App\Models\User;

Route::get('users', function(Builder $builder) {
    // Handle AJAX requests
    if (request()->ajax()) {
        return DataTables::of(User::query())->toJson();
    }

    // Build HTML for initial page load
    $html = $builder->columns([
        Column::make('name'),
        Column::make('email'),
        Column::make('created_at'),
        Column::make('id'),
    ]);

    return view('users.index', compact('html'));
});
```

### Blade Template

```blade
@extends('layouts.app')

@section('content')
<div class="container">
    <div class="card">
        <div class="card-header">
            <h3>Users</h3>
        </div>
        <div class="card-body">
            {!! $html->table() !!}
        </div>
    </div>
</div>
@endsection

@push('scripts')
    {!! $html->scripts() !!}
@endpush
```

---

## Configuration Options

Customize the default table attributes in `config/datatables-html.php`:

```php
<?php

return [
    'table' => [
        'class' => 'table table-bordered table-striped',
        'id' => 'dataTable',
        'width' => '100%',
    ],
    'script' => 'datatables::script',
    'scriptNonce' => env('DATA_TABLES_SCRIPT_NONCE'),
];
```

---

## Column Types

| Type | Method | Description |
|------|--------|-------------|
| Standard | `Column::make('name')` | Database column |
| Computed | `Column::computed('action')` | JavaScript-rendered column |
| Checkbox | `Column::checkbox()` | Row selection checkboxes |
| Index | `Column::index()` | Row numbering |
| Action | `Column::action()` | Action buttons |

---

## Common Configuration

### Enable Server-Side Processing

```php
public function html(): HtmlBuilder
{
    return $this->builder()
        ->columns($this->getColumns())
        ->minifiedAjax()
        ->orderBy(0, 'asc')
        ->serverSide(true)
        ->processing(true);
}
```

### Add Buttons

```php
use Yajra\DataTables\Html\Button;

public function html(): HtmlBuilder
{
    return $this->builder()
        ->columns($this->getColumns())
        ->buttons([
            Button::make('excel'),
            Button::make('csv'),
            Button::make('pdf'),
            Button::make('print'),
        ]);
}
```

---

## See Also

- [HTML Builder Overview](/docs/{{package}}/{{version}}/html-builder) - Complete reference
- [Column Configuration](/docs/{{package}}/{{version}}/html-builder-column) - Column options
- [AJAX Setup](/docs/{{package}}/{{version}}/html-builder-ajax) - Server-side processing
