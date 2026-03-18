# HTML Builder Event Callbacks

Callbacks are JavaScript functions that DataTables executes at specific events during the table's lifecycle.

## Available Callbacks

| Callback | Description |
|----------|-------------|
| `createdRow` | Called when a TR element is created |
| `drawCallback` | Called every time DataTables performs a draw |
| `footerCallback` | Footer display callback function |
| `formatNumber` | Number formatting callback function |
| `headerCallback` | Header display callback function |
| `infoCallback` | Table summary information display callback |
| `initComplete` | Initialisation complete callback |
| `preDrawCallback` | Pre-draw callback |
| `rowCallback` | Row draw callback |

## Basic Usage

```php
$html = $builder
    ->drawCallback('function() { alert("Table Drawn"); }')
    ->createdRow('function(row, data) { ... }');
```

## Complete Examples

### drawCallback

```php
$html = $builder->drawCallback('function() {
    console.log("Table redrawn");
    $(".dataTables_wrapper .btn").addClass("btn-sm");
}');
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

## See Also

- [Html Builder](/docs/{{package}}/{{version}}/html-builder)
