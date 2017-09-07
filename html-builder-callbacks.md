# Html Builder Event Callbacks

You can use a `js` string for each valid callback as documented on [`datatables.net`](https://datatables.net/reference/option/) callback options list.

## DataTables - Callbacks
| Callback | Description |
| --- | --- |
|`createdRow` | Callback for whenever a TR element is created for the table's body. |
|`drawCallback` | Function that is called every time DataTables performs a draw. |
|`footerCallback` | Footer display callback function. |
|`formatNumber` | Number formatting callback function. |
|`headerCallback` | Header display callback function. |
|`infoCallback` | Table summary information display callback. |
|`initComplete` | Initialisation complete callback. |
|`preDrawCallback` | Pre-draw callback. |
|`rowCallback` | Row draw callback. |
|`stateLoadCallback` | Callback that defines where and how a saved state should be loaded. |
|`stateLoaded` | State loaded callback. |
|`stateLoadParams` | State loaded - data manipulation callback |
|`stateSaveCallback` | Callback that defines how the table state is stored and where. |
|`stateSaveParams` | State save - data manipulation callback |

## Example
In this example, we will hook on the the `drawCallback` of dataTable.

```php
$builder->parameters([
	'drawCallback' => 'function() { alert("Table Draw Callback") }',
]);
```