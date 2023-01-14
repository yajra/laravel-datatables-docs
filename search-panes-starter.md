# Getting Started

[SearchPanes](https://datatables.net/extensions/searchpanes/) ([example](https://datatables.net/extensions/searchpanes/examples/initialisation/simple.html))
allow the user to quickly filter the datatable after predefined filters.

> {note} To use datatables you need to make sure that the npm packages `datatables.net-select-bs4` and `datatables.net-searchpanes-bs4` are installed and added to your `app.js`/`app.css` files.

## Adding SearchPanes to the frontend

To be able to see SearchPanes you need to either add them fixed in the dom (displayed at all time) or add a button which
opens them as popup.

<a name="dom"></a>
### Adding SearchPanes fixed in the dom 

SearchPanes can be added to a table via the dom string, in it, they are marked with a `P` if you for example
are using `Bfrtip` as dom you can use `PBfrtip` to display the SearchPanes at the top of the datatable, or `BfrtipP`
to display them at the very bottom.

Setting the dom String with the `\Yajra\DataTables\Html\Builder`:

```php
public function html() : \Yajra\DataTables\Html\Builder
{
    // Setting the dom string directly
    return $this->builder()
        ->searchPanes(SearchPane::make())
        ->dom('PBfrtip');
    
    // Alternatively set the dom with parameters
    return $this->builder()
        ->searchPanes(SearchPane::make())
        ->parameters([
            'dom' => 'PBfrtip'
        ]);
}
```

<a name="button"></a>
### Adding SearchPanes with a button

To add a button which opens the SearchPanes you need to make one extending `searchPanes`:

```php
public function html() : \Yajra\DataTables\Html\Builder
{
    // Adding via class
    return $this->builder()
        ->searchPanes(SearchPane::make())
        ->buttons([
            \Yajra\DataTables\Html\Button::make('searchPanes')
            // other buttons...
        ]);
    
    // Alternatively set the buttons with options
    return $this->builder()
        ->searchPanes(SearchPane::make())
        ->parameters([
            'buttons' => ['searchPanes', /*other buttons...*/]
        ]);
}
```

<a name="backend"></a>
## Adding SearchPanes to the backend

The SearchPanes can be filled in the datatables declaration via the `searchPane()` method. The method takes the column
for which the SearchPane is, as well as the options of the SearchPane. It also allows you to set custom processing for
the options.


```php
public function dataTable($query) : Yajra\DataTables\DataTableAbstract
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
             * Here we define the options for our SearchPane. This should be either a collection or an array with the
             * form:
             * [
             *     [
             *          'value' => 1,
             *          'label' => 'display value',
             *          'total' => 5, // optional
             *          'count' => 3 // optional
             *     ],
             *     [
             *          'value' => 2,
             *          'label' => 'display value 2',
             *          'total' => 6, // optional
             *          'count' => 5 // optional
             *     ],
             * ]
             */
            fn() => User::query()->select('id as value', 'name as label')->get(),
            
            /*
             * This is the filter that gets executed when the user selects one or more values on the SearchPane. The
             * values are always given in an array even if just one is selected
             */
            function (\Illuminate\Database\Eloquent\Builder $query, array $values) {
                return $query
                    ->whereIn(
                        'id',
                        $values);
            }
        );
}
```
