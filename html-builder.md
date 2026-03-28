---
title: HTML Builder
description: Build DataTables HTML and JavaScript using a fluent API
---


# HTML Builder

The HTML Builder is a fluent interface for generating DataTables HTML markup and JavaScript configuration. It provides a clean, expressive way to define your table structure, columns, and options.

---

<a name="installation"></a>
## Installation

```bash
composer require yajra/laravel-datatables-html:"^13.0"
```

After installation, publish the configuration:

```bash
php artisan vendor:publish --tag=datatables-html
```

---

<a name="basic"></a>
## Basic Usage

### Via Dependency Injection

```php
use Yajra\DataTables\Html\Builder;
use Yajra\DataTables\Facades\DataTables;
use Yajra\DataTables\Html\Column;
use App\Models\User;

Route::get('users', function(Builder $builder) {
    if (request()->ajax()) {
        return DataTables::of(User::query())->toJson();
    }

    $html = $builder->columns([
        Column::make('name'),
        Column::make('email'),
        Column::make('id'),
    ]);

    return view('users.index', compact('html'));
});
```

### Via IoC Container

```php
use Yajra\DataTables\Facades\DataTables;

Route::get('users', function() {
    $builder = app('datatables.html');
});
```

### From DataTables Instance

```php
use Yajra\DataTables\Facades\DataTables;

Route::get('users', function(DataTables $dataTable) {
    $builder = $dataTable->getHtmlBuilder();
});
```

---

## Configuration

Edit `config/datatables-html.php` to set default table attributes:

```php
<?php

return [
    'table' => [
        'class' => 'table table-bordered',
        'id'    => 'dataTable',
    ],
];
```

---

## Column Types

### Standard Columns

```php
Column::make('name')
    ->title('Name')
    ->data('name')
    ->name('name')
```

### ID Columns

Use `Column::make('id')` for the ID field:

```php
Column::make('id')
    ->title('ID')
```

### Computed Columns

```php
Column::computed('action', 'Action')
    ->orderable(false)
    ->searchable(false)
```

### Checkbox Columns

```php
Column::checkbox()
```

Or use the helper method:

```php
$builder->addCheckbox()
```

### Index Columns

```php
Column::index()
```

Or use the helper method:

```php
$builder->addIndex()
```

### Action Columns

```php
Column::action()
```

Or use the helper method:

```php
$builder->addAction()
```

---

## Column Attributes

| Attribute | Type | Description |
|-----------|------|-------------|
| `name` | string | Column name from data source |
| `data` | string | Key from JSON response |
| `title` | string | Column header text |
| `searchable` | bool | Enable/disable search |
| `orderable` | bool | Enable/disable ordering |
| `render` | string | JavaScript render function |
| `footer` | string | Footer content |
| `exportable` | bool | Include in exports |
| `printable` | bool | Include in print view |
| `className` | string | CSS class name |
| `width` | string | Column width |
| `visible` | bool | Show/hide column |

---

## Table Configuration

### Basic Table

```php
echo $html->table();
```

### Table with Custom Attributes

```php
echo $html->table(['class' => 'table table-striped', 'id' => 'users-table']);
```

### Table with Footer

```php
echo $html->table(['class' => 'table'], true);
```

### Table with Search Headers

```php
echo $html->table([], false, true);
```

---

## AJAX Configuration

### Basic AJAX

```php
$html = $builder->ajax(route('users.data'));
```

### POST AJAX

```php
$html = $builder->postAjax(route('users.data'));
```

### AJAX with Custom Data

```php
$html = $builder->ajax([
    'url' => route('users.data'),
    'type' => 'GET',
    'data' => 'function(d) { d.key = "value"; }'
]);
```

---

## Parameters

Configure DataTables options:

```php
$html = $builder
    ->paging(true)
    ->searching(true)
    ->info(false)
    ->searchDelay(350);
```

### Display Options

```php
$html = $builder
    ->autoWidth(true)           // Enable auto width calculation
    ->deferRender(true)         // Defer rendering for better performance
    ->scrollX(true)             // Enable horizontal scrolling
    ->scrollY('500px')          // Enable vertical scrolling with height
    ->scrollCollapse(true)      // Collapse table when scrolling
    ->pagingType('simple_numbers')  // Pagination style
    ->pageLength(25)            // Default page length
    ->lengthMenu([10, 25, 50, 100])  // Available page lengths
    ->stripeClasses(['odd', 'even'])  // CSS classes for row striping
    ->tabIndex(0)               // Keyboard navigation tab index
    ->renderer('bootstrap')     // Bootstrap renderer
    ->retrieve(false)           // Retrieve existing instance
    ->destroy(true)             // Destroy existing instance before creating new
    ->displayStart(0)          // Initial display start index
    ->deferLoading(100)        // Deferred loading with known total
    ->tabIndex(0);              // Keyboard navigation
```

### Ordering Options

```php
$html = $builder
    ->ordering(true)            // Enable column ordering
    ->orderBy(0, 'desc')        // Default order by column 0 descending
    ->orderByFixed(1, 'asc')    // Fixed order (always applied)
    ->orderMulti(true)          // Enable multi-column sorting
    ->orderCellsTop(true)      // Order only on first cell click
    ->orderClasses(true)        // Highlight ordered column
    ->orders([[0, 'desc'], [1, 'asc']]);  // Multiple default orders
```

### Search Options

```php
$html = $builder
    ->searching(true)           // Enable search
    ->searchDelay(400)         // Search delay in milliseconds
    ->search(['regex' => false, 'search' => '']);  // Initial search settings
```

### State Options

```php
$html = $builder
    ->stateSave(true)          // Save table state
    ->stateDuration(-1)        // State duration (-1 = session, 0 = forever)
    ->rowId('DT_RowId');      // Row ID attribute
```

---

## Plugins

### Select Plugin

```php
$html = $builder
    ->selectStyleSingle()
    ->selectInfo(true)
    ->selectItemsRow();
```

### Buttons Plugin

```php
use Yajra\DataTables\Html\Button;

$html = $builder->buttons(
    Button::make('excel')->text('Export Excel'),
    Button::make('pdf')->text('Export PDF')
);
```

### SearchPanes Plugin

```php
$html = $builder->searchPanes(true);
```

### Responsive Plugin

```php
$html = $builder->responsive(true);
```

---

## Scripts Generation

```php
// With default type
echo $html->scripts();

// With module type (for Vite)
echo $html->scripts([], ['type' => 'module']);
```

### Vite/Webpack Configuration

Set the default script type globally:

```php
// Use module type (for Vite)
Builder::useVite();

// Use text/javascript (for Webpack/Laravel Mix)
Builder::useWebpack();
```

### Script Output Options

```php
// Generate DataTable options as a variable
echo $builder->asOptions();

// Wrap as a function
echo $builder->asFunction();

// Get the template being used
$builder->getTemplate();
```

### Additional Scripts

Add extra scripts to the DataTables initialization:

```php
// Add additional script view
$html = $builder->addScript('datatables::custom-script');

// Conditional script loading
$html = $builder->addScriptIfCan('delete', 'datatables::delete-button');
$html = $builder->addScriptIf($condition, 'datatables::conditional-script');
$html = $builder->addScriptIfCannot('admin', 'datatables::readonly-script');
```

### Template Data

Pass custom data to the script template:

```php
$html = $builder->setTemplateData([
    'customVar' => 'value',
]);

// Or use a closure
$html = $builder->setTemplateData(function ($builder) {
    return ['id' => $builder->getTableId()];
});
```

---

## See Also

- [HTML Builder Column](/docs/{{package}}/{{version}}/html-builder-column) - Column configuration options
- [HTML Builder Parameters](/docs/{{package}}/{{version}}/html-builder-parameters) - All available parameters
- [HTML Builder Callbacks](/docs/{{package}}/{{version}}/html-builder-callbacks) - JavaScript callbacks
- [Buttons Plugin](/docs/{{package}}/{{version}}/buttons-installation) - Export button options
