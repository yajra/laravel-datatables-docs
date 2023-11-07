# Additional JavaScript Scripts

## General Usage

Starting with v10.10.0, you can easily add additional JavaScript scripts as blade views in your `html` function like this:

```php
return $this->builder()
    ->addScript('your.view.name');
```

## Built-in Additional Scripts

### Scout Search

**View:** `datatables::scout`

Control visibility of sort icons on the frontend table. Hide sort icons when Scout Search is successful (indicating fixed ordering by search relevance) and show them again when the search keyword is removed.

### Batch Remove Optimization

**View:** `datatables::functions.batch_remove`

Delete all unnecessary information before sending `remove` requests (Editor), keeping only the `DT_RowId`. Otherwise, batch remove requests may fail because of server limits.
