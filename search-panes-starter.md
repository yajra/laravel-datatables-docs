---
title: SearchPanes Getting Started
description: Add SearchPanes for quick filtering of DataTables
---

# SearchPanes Getting Started

[SearchPanes](https://datatables.net/extensions/searchpanes/) allows the user to quickly filter the DataTable after predefined filters.

> [!NOTE]
> To use SearchPanes, you need to ensure that the npm packages `datatables.net-select-bs5` and `datatables.net-searchpanes-bs5` are installed and added to your `app.js`/`app.css` files.

---

<a name="frontend"></a>
## Adding SearchPanes to the Frontend

To see SearchPanes, you can either add them fixed in the DOM (displayed at all times) or add a button which opens them as a popup.

### Adding SearchPanes Fixed in the DOM

SearchPanes can be added to a table via the DOM string, marked with a `P`. For example, if you are using `Bfrtip` as DOM, you can use `PBfrtip` to display SearchPanes at the top, or `BfrtipP` at the bottom.

Setting the layout with the `\Yajra\DataTables\Html\Builder`:

```php
use Yajra\DataTables\Html\SearchPane;

public function html(): \Yajra\DataTables\Html\Builder
{
    return $this->builder()
        ->searchPanes(SearchPane::make())
        ->layout([
            'topStart' => 'buttons',
            'topEnd' => 'search',
            'bottomStart' => 'info',
            'bottomEnd' => 'paging',
        ]);
}
```

### Adding SearchPanes with a Button

To add a button which opens the SearchPanes, make one extending `searchPanes`:

```php
use Yajra\DataTables\Html\Builder;
use Yajra\DataTables\Html\Button;
use Yajra\DataTables\Html\SearchPane;

public function html(): Builder
{
    // Adding via class
    return $this->builder()
        ->searchPanes(SearchPane::make())
        ->buttons([
            Button::make('searchPanes'),
            // other buttons...
        ]);

    // Alternatively set the buttons with options
    return $this->builder()
        ->searchPanes(SearchPane::make())
        ->layout([
            'topStart' => 'buttons',
            'topEnd' => 'search',
            'bottomStart' => 'info',
            'bottomEnd' => 'paging',
        ])
        ->buttons(['searchPanes', /*other buttons...*/]);
}
```

---

<a name="backend"></a>
## Adding SearchPanes to the Backend

The SearchPanes can be filled via the `searchPane()` method. The method takes the column for the SearchPane, options for the SearchPane, and allows custom processing for the options.

```php
use Yajra\DataTables\DataTablesAbstract;
use App\Models\User;

public function dataTable($query): DataTablesAbstract
{
    return datatables()
        ->eloquent($query)
        // Adding the SearchPane
        ->searchPane(
            /*
             * This is the column for which this SearchPane definition is for
             */
            'user_id',

            /*
             * Here we define the options for our SearchPane. This should be either a collection or an array with the form:
             * [
             *     [
             *          'value' => 1,
             *          'label' => 'display value',
             *          'total' => 5, // optional
             *          'count' => 3 // optional
             *     ],
             *     ...
             * ]
             */
            fn() => User::query()->select('id as value', 'name as label')->get(),

            /*
             * This is the filter that gets executed when the user selects one or more values on the SearchPane.
             * The values are always given in an array even if just one is selected.
             */
            function (\Illuminate\Database\Eloquent\Builder $query, array $values) {
                return $query->whereIn('id', $values);
            }
        );
}
```

---

## SearchPanes Options

### Option Structure

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| `value` | mixed | Yes | The value to filter by |
| `label` | string | Yes | The display label |
| `total` | int | No | Total count of records |
| `count` | int | No | Filtered count |

---

## See Also

- [SearchPanes Options](/docs/{{package}}/{{version}}/search-panes-options) - More configuration options
- [SearchPanes Hide Columns](/docs/{{package}}/{{version}}/search-panes-hide-columns) - Control which columns appear in SearchPanes
- [Filter Column](/docs/{{package}}/{{version}}/filter-column) - Custom column filtering
