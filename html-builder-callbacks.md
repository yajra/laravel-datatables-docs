# HTML Builder Event Callbacks

Callbacks are JavaScript functions that DataTables executes at specific events during the table's lifecycle. They allow you to hook into DataTables events and execute custom code.

## Available Callbacks

| Callback | Description |
|----------|-------------|
| `createdRow` | Called when a TR element is created for the table's body |
| `drawCallback` | Called every time DataTables performs a draw |
| `footerCallback` | Footer display callback function |
| `formatNumber` | Number formatting callback function |
| `headerCallback` | Header display callback function |
| `infoCallback` | Table summary information display callback |
| `initComplete` | Initialisation complete callback |
| `preDrawCallback` | Pre-draw callback |
| `rowCallback` | Row draw callback |
| `stateLoadCallback` | Callback that defines where and how a saved state should be loaded |
| `stateLoaded` | State loaded callback |
| `stateLoadParams` | State loaded - data manipulation callback |
| `stateSaveCallback` | Callback that defines how the table state is stored and where |
| `stateSaveParams` | State save - data manipulation callback |

## Basic Usage

### Using parameters()

```php
$html = $builder->parameters([
    'drawCallback' => 'function() { alert("Table Drawn"); }',
    'createdRow' => 'function(row, data) { ... }',
]);
```

### Using Fluent Methods

```php
$html = $builder
    ->drawCallback('function() { alert("Table Drawn"); }')
    ->createdRow('function(row, data) { ... }');
```

## Complete Examples

### drawCallback

Called every time DataTables performs a draw:

```php
$html = $builder->drawCallback('function() {
    console.log("Table redrawn");
    // Add custom styling after draw
    $(".dataTables_wrapper .btn").addClass("btn-sm");
}');
```

### createdRow

Called when a TR element is created:

```php
$html = $builder->createdRow('function(row, data, dataIndex) {
    // Add custom class to row
    $(row).addClass("user-row");
    
    // Set data attributes
    $(row).attr("data-id", data.id);
}');
```

### footerCallback

Called for footer display:

```php
$html = $builder->footerCallback('function(tfoot, data, start, end, display) {
    var api = this.api();
    var intVal = function(i) {
        return typeof i === "string" ? i.replace(/[\$,]/g, "") * 1 : i;
    };
    
    var total = api.column(3).data().reduce(function(a, b) {
        return intVal(a) + intVal(b);
    }, 0);
    
    $(tfoot).find("td").eq(3).html("$" + total.toFixed(2));
}');
```

### formatNumber

For number formatting:

```php
$html = $builder->formatNumber('function(number, type, scale) {
    return number.toLocaleString();
}');
```

### headerCallback

For header display:

```php
$html = $builder->headerCallback('function(thead, data, start, end, display) {
    $(thead).find("th").addClass("header-cell");
}');
```

### infoCallback

For table summary information:

```php
$html = $builder->infoCallback('function(settings, start, end, max, total, pre) {
    return "Showing " + total + " records";
}');
```

### initComplete

Called after initialisation:

```php
$html = $builder->initComplete('function(settings, json) {
    console.log("DataTables initialized");
    // Add custom controls
    $(".dataTables_filter").append("<button id=\"custom-btn\">Custom</button>");
}');
```

### preDrawCallback

Called before draw:

```php
$html = $builder->preDrawCallback('function(settings) {
    // Return false to cancel draw
    return true;
}');
```

### rowCallback

Called for each row:

```php
$html = $builder->rowCallback('function(row, data) {
    // Modify row after creation
    if (data.status === "inactive") {
        $(row).addClass("text-muted");
    }
}');
```

### stateLoadCallback

For loading saved state:

```php
$html = $builder->stateLoadCallback('function(settings) {
    return localStorage.getItem('DataTables_' + settings.sInstance);
}');
```

### stateLoaded

Called after state is loaded:

```php
$html = $builder->stateLoaded('function(settings, data) {
    console.log("State loaded:", data);
}');
```

### stateLoadParams

For manipulating loaded state:

```php
$html = $builder->stateLoadParams('function(settings, data) {
    // Remove search from saved state
    data.search.search = "";
}');
```

### stateSaveCallback

For saving state:

```php
$html = $builder->stateSaveCallback('function(settings, data) {
    localStorage.setItem('DataTables_' + settings.sInstance, JSON.stringify(data));
}');
```

### stateSaveParams

For manipulating saved state:

```php
$html = $builder->stateSaveParams('function(settings, data) {
    // Remove search from saved state
    data.search.search = "";
}');
```

## Livewire Integration

For Livewire integration, use `drawCallbackWithLivewire()`:

```php
$html = $builder->drawCallbackWithLivewire();
```

Or with custom script:

```php
$html = $builder->drawCallbackWithLivewire('console.log("Livewire updated");');
```

## See Also

- [Html Builder](/docs/{{package}}/{{version}}/html-builder)
- [Html Builder Parameters](/docs/{{package}}/{{version}}/html-builder-parameters)
