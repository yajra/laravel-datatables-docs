---
title: HTML Builder Event Callbacks
description: JavaScript callbacks for DataTables HTML Builder
---

# HTML Builder Event Callbacks

Callbacks are JavaScript functions that DataTables executes at specific events during the table's lifecycle.

<a name="available"></a>
## Available Callbacks

| Callback | Description |
|----------|-------------|
| `createdRow` | Called when a TR element is created |
| `drawCallback` | Called every time DataTables performs a draw |
| `drawCallbackWithLivewire` | Draw callback with Livewire integration |
| `footerCallback` | Footer display callback function |
| `formatNumber` | Number formatting callback function |
| `headerCallback` | Header display callback function |
| `infoCallback` | Table summary information display callback |
| `initComplete` | Initialisation complete callback |
| `preDrawCallback` | Pre-draw callback |
| `rowCallback` | Row draw callback |
| `stateLoadCallback` | State load callback |
| `stateLoaded` | State loaded callback |
| `stateLoadParams` | State load params callback |
| `stateSaveCallback` | State save callback |
| `stateSaveParams` | State save params callback |

<a name="basic"></a>
## Basic Usage

```php
$html = $builder
    ->drawCallback('function() { alert("Table Drawn"); }')
    ->createdRow('function(row, data) { ... }');
```

<a name="examples"></a>
## Complete Examples

### drawCallback

```php
$html = $builder->drawCallback('function() {
    console.log("Table redrawn");
    $(".dataTables_wrapper .btn").addClass("btn-sm");
}');
```

### drawCallbackWithLivewire

Automatically rescan Livewire components after each draw:

```php
$html = $builder->drawCallbackWithLivewire();
```

With additional custom script:

```php
$html = $builder->drawCallbackWithLivewire('console.log("Custom script");');
```

### createdRow

```php
$html = $builder->createdRow('function(row, data, dataIndex) {
    $(row).addClass("user-row");
    $(row).attr("data-id", data.id);
}');
```

### footerCallback

```php
$html = $builder->footerCallback('function(tfoot, data, start, end, display) {
    var api = this.api();
    var total = api.column(3).data().reduce(function(a, b) {
        return parseFloat(a) + parseFloat(b);
    }, 0);
    $(tfoot).find("td").eq(3).html("$" + total.toFixed(2));
}');
```

### headerCallback

```php
$html = $builder->headerCallback('function(thead, data, start, end, display) {
    $(thead).find("th").css("font-weight", "bold");
}');
```

### rowCallback

```php
$html = $builder->rowCallback('function(row, data, dataIndex) {
    if (data.status === "inactive") {
        $(row).addClass("text-muted");
    }
}');
```

### initComplete

```php
$html = $builder->initComplete('function(settings, json) {
    console.log("DataTables initialized");
    // Custom initialization code
}');
```

### State Callbacks

```php
// Save state to server
$html = $builder->stateSaveCallback('function(settings, data) {
    localStorage.setItem("DataTables_" + settings.sInstance, JSON.stringify(data));
}');

// Load state from server
$html = $builder->stateLoadCallback('function(settings) {
    return JSON.parse(localStorage.getItem("DataTables_" + settings.sInstance));
}');

// Before state is loaded
$html = $builder->stateLoadParams('function(settings, data) {
    if (data.search) {
        data.search.search = "";
    }
}');

// After state is loaded
$html = $builder->stateLoaded('function(settings, data) {
    console.log("State loaded:", data);
});

// Before state is saved
$html = $builder->stateSaveParams('function(settings, data) {
    delete data.search;
}');
```

## See Also

- [Html Builder](/docs/{{package}}/{{version}}/html-builder)
