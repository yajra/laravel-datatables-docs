# Further SearchPanes options

```php
public function html() : \Yajra\DataTables\Html\Builder
{
    return $this->builder()
        ->searchPanes(
            SearchPane::make()
                ->hideCount() // Hides the count next to the options, the amount of records with the filters
                ->hideTotal() // Hides the total how many are available without the filters
                ->clear(false) // Hides the clear button, used to clear the user selection
                ->controls(false) // disable controls (search for filters)
                ->layout('columns-2') // can be used to set the layout (amount of search panes in one row)
                ->order(['user_id', 'age']) // Sets the order of the search panes, uses the name of the search pane
                ->orderable(false) // If the order icons should be displayed in the interface
                ->panes( // Can be used to add additional search panes which do not belong to any existing column
                    [
                        // SearchPane::make()...
                    ]
                )
                ->collapse(false)
                ->dtOpts( // Sets the options for the datatables inside the searchpanes
                    [
                        'paging' => true, 
                        'pagingType' => 'numbers'
                    ]
                )
                    
            );
}
```
