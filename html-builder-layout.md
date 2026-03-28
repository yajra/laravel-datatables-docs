---
title: Layout
description: Define and position table control elements using DataTables layout feature
---

# HTML Builder Layout

The `layout()` method allows customizing the table control elements position using DataTables [layout](https://datatables.net/reference/option/layout) option.

<a name="basic"></a>
## Basic Usage

```php
use Yajra\DataTables\Html\Builder;
use Yajra\DataTables\Html\Layout;

$html = $builder->layout(function (Layout $layout) {
    $layout->topStart('pageLength');
    $layout->topEnd('search');
    $layout->bottomStart('info');
    $layout->bottomEnd('paging');
});
```

---

<a name="positions"></a>
## Available Positions

The position name format is `(top|bottom)[number](Start|End)?`:

| Position | Description |
|----------|-------------|
| `top` | Top row, full width |
| `topStart` | Top left corner |
| `topEnd` | Top right corner |
| `bottom` | Bottom row, full width |
| `bottomStart` | Bottom left corner |
| `bottomEnd` | Bottom right corner |
| `top2`, `top2Start`, `top2End` | Second top row |
| `bottom2`, `bottom2Start`, `bottom2End` | Second bottom row |

```php
$html = $builder->layout(function (Layout $layout) {
    $layout->topStart('pageLength');
    $layout->topEnd('search');
    $layout->bottomStart('info');
    $layout->bottomEnd('paging');
});
```

---

<a name="features"></a>
## Built-in Features

DataTables provides these built-in features:

- `info` - Table information summary
- `pageLength` - Page length control
- `paging` - User input control for paging
- `search` - Search input box
- `div` - A simple placeholder element
- `null` - Show nothing

```php
$html = $builder->layout(function (Layout $layout) {
    // Use built-in features
    $layout->topStart('pageLength');
    $layout->topEnd(['search' => ['placeholder' => 'Search...']]);
    $layout->bottomStart('info');
    $layout->bottomEnd('paging');
    
    // Remove a feature
    $layout->topStart(null);
});
```

---

<a name="multiple"></a>
## Multiple Elements

Pass an array to show multiple items:

```php
$html = $builder->layout(function (Layout $layout) {
    $layout->top(['pageLength', 'search']);
    $layout->bottom(['info', 'paging']);
});
```

---

<a name="ordering"></a>
## Multiple Rows

Use ordering for multiple rows:

```php
$html = $builder->layout(function (Layout $layout) {
    // First top row
    $layout->topStart('pageLength');
    $layout->topEnd('search');
    
    // Second top row
    $layout->top2Start('info');
    $layout->top2End('paging');
});
```

---

<a name="views"></a>
## Custom Elements

Use view methods to render jQuery selectors:

```php
$html = $builder->layout(function (Layout $layout) {
    $layout->topStartView('#custom-toolbar');
    $layout->bottomEndView('#custom-footer');
});
```

---

<a name="livewire"></a>
## Livewire Components

Add Livewire components to the layout:

```php
use Yajra\DataTables\Html\Enums\LayoutPosition;

$html = $builder->layout(function (Layout $layout) {
    $layout->addLivewire(
        \App\Livewire\DataTableFilters::class,
        LayoutPosition::TopStart
    );
});
```

---

<a name="views-add"></a>
## Add Custom View

Render any view or component:

```php
use Yajra\DataTables\Html\Enums\LayoutPosition;

$html = $builder->layout(function (Layout $layout) {
    $layout->addView('components.search-box', LayoutPosition::TopEnd);
});
```

---

<a name="array-config"></a>
## Array Configuration

You can also pass an array directly:

```php
$html = $builder->layout([
    'topStart' => 'pageLength',
    'topEnd' => 'search',
    'bottomStart' => 'info',
    'bottomEnd' => 'paging',
]);
```

Or use `Layout::make()`:

```php
use Yajra\DataTables\Html\Layout;

$layout = Layout::make();
$layout->topStart('pageLength');
$layout->topEnd('search');

$html = $builder->layout($layout);
```

---

## See Also

- [HTML Builder](/docs/{{package}}/{{version}}/html-builder) - Main HTML Builder documentation
- [DataTables Layout](https://datatables.net/reference/option/layout) - Official DataTables layout documentation
