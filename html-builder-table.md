# HTML Builder Table

The `table()` method generates the HTML table markup for your DataTable. It provides options for customizing the table structure, attributes, and footer.

## Basic Usage

### Simple Table

```php
echo $html->table();
```

### Table with Custom Attributes

```php
echo $html->table([
    'class' => 'table table-striped table-bordered',
    'id' => 'users-table',
    'width' => '100%',
]);
```

### Table with Footer

```php
echo $html->table(['class' => 'table'], true);
```

### Table with Search Headers

```php
echo $html->table([], false, true);
```

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

## Table Attributes

Common attributes you can set:

| Attribute | Description |
|-----------|-------------|
| `class` | CSS classes for the table |
| `id` | Table ID for JavaScript selection |
| `width` | Table width |
| `cellspacing` | Cell spacing |
| `cellpadding` | Cell padding |
| `border` | Border width |

## Complete Examples

### Basic Table

```php
$html = $builder->columns([
    Column::make('id'),
    Column::make('name'),
    Column::make('email'),
]);

// Generates: <table class="table" id="dataTable"><thead>...</thead></table>
echo $html->table(['class' => 'table']);
```

### Table with Footer

```php
$html = $builder->columns([
    ['data' => 'id', 'footer' => 'Id'],
    ['data' => 'name', 'footer' => 'Name'],
    ['data' => 'email', 'footer' => 'Email'],
    ['data' => 'created_at', 'footer' => 'Created At'],
]);

echo $html->table(['class' => 'table table-bordered'], true);
```

### Table with Search Headers

```php
$html = $builder->columns([
    Column::make('id'),
    Column::make('name'),
    Column::make('email'),
]);

echo $html->table(['class' => 'table'], false, true);
```

### Full Example with All Options

```php
$html = $builder->columns([
    Column::make('id'),
    Column::make('name'),
    Column::make('email'),
    Column::make('created_at'),
]);

echo $html->table(
    ['class' => 'table table-striped table-hover', 'id' => 'users-table'],
    true,  // Draw footer
    true   // Draw search headers
);
```

## Blade Template Usage

```blade
@extends('app')

@section('contents')
    <div class="card">
        <div class="card-body">
            {{ $html->table(['class' => 'table table-bordered']) }}
        </div>
    </div>
@endsection

@push('scripts')
    {{ $html->scripts() }}
@endpush
```

## See Also

- [Html Builder](/docs/{{package}}/{{version}}/html-builder)
- [Html Builder Column](/docs/{{package}}/{{version}}/html-builder-column)
