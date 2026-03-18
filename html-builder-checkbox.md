# HTML Builder Checkbox Column

The `addCheckbox()` method provides a quick way to add a checkbox column to your DataTable. This is commonly used for bulk operations like selecting multiple rows for deletion or other actions.

## Basic Usage

### Using addCheckbox()

```php
$html = $builder->addCheckbox();
```

### With Custom Attributes

```php
$html = $builder->addCheckbox([
    'title' => '<input type="checkbox" id="dataTablesCheckbox"/>',
]);
```

### With Position

```php
// Prepend to beginning
$html = $builder->addCheckbox([], true);

// At specific position
$html = $builder->addCheckbox([], 2);
```

## Default Attributes

The checkbox column has the following default attributes:

```php
[
    'defaultContent' => '<input type="checkbox" />',
    'title' => '<input type="checkbox" id="dataTablesCheckbox"/>',
    'data' => 'checkbox',
    'name' => 'checkbox',
    'orderable' => false,
    'searchable' => false,
    'exportable' => false,
    'printable' => true,
    'width' => '10px',
]
```

## Complete Examples

### Basic Checkbox Column

```php
Route::get('users', function(Builder $builder) {
    if (request()->ajax()) {
        return DataTables::of(User::query())->toJson();
    }

    $html = $builder
        ->addCheckbox()
        ->columns([
            Column::make('id'),
            Column::make('name'),
            Column::make('email'),
        ]);

    return view('users.index', compact('html'));
});
```

### Checkbox with Custom Title

```php
$html = $builder->addCheckbox([
    'title' => '<input type="checkbox" id="selectAll"/>',
]);
```

### Checkbox with Custom Width

```php
$html = $builder->addCheckbox([
    'width' => '40px',
]);
```

### Checkbox with Custom Class

```php
$html = $builder->addCheckbox([
    'className' => 'select-checkbox',
]);
```

### Checkbox with Custom Data

```php
$html = $builder->addCheckbox([
    'data' => 'id',
    'render' => 'function(data, type, row) {
        return "<input type=\"checkbox\" value=\"" + data + "\" class=\"row-checkbox\"/>";
    }',
]);
```

## Using Column::checkbox()

You can also use the `Column::checkbox()` method:

```php
$html = $builder->columns([
    Column::checkbox(),
    Column::make('id'),
    Column::make('name'),
]);
```

## JavaScript Integration

### Select All Checkbox

```blade
@push('scripts')
<script>
$(document).ready(function() {
    $('#selectAll').on('click', function() {
        $('.row-checkbox').prop('checked', this.checked);
    });
    
    $('.row-checkbox').on('click', function() {
        $('#selectAll').prop('checked', $('.row-checkbox').length === $('.row-checkbox:checked').length);
    });
});
</script>
@endpush
```

### Get Selected Rows

```blade
@push('scripts')
<script>
function getSelectedIds() {
    var ids = [];
    $('.row-checkbox:checked').each(function() {
        ids.push($(this).val());
    });
    return ids;
}

$('#bulkDelete').on('click', function() {
    var ids = getSelectedIds();
    if (ids.length > 0) {
        // Perform bulk delete
        $.post('/users/bulk-delete', {ids: ids});
    }
});
</script>
@endpush
```

## See Also

- [Html Builder](/docs/{{package}}/{{version}}/html-builder)
- [Html Builder Column](/docs/{{package}}/{{version}}/html-builder-column)
