---
title: Row Id
description: Set unique identifiers for DataTables rows
---


# Setting Row ID, Class, and Data in Laravel DataTables

DataTables allows you to customize the HTML attributes of each `<tr>` row using special options: `DT_RowId`, `DT_RowClass`, and `DT_RowData`. The Laravel DataTables package provides convenient methods to set these attributes dynamically.

---

<a name="setting-the-row-id"></a>
## Setting the Row ID (`setRowId`)

Use the `setRowId` method to set the `id` attribute of each table row. You can pass a string, column name, or a closure for dynamic values.

**Example:**

```php
$dataTable->setRowId('id'); // Uses the 'id' column value for each row

$dataTable->setRowId(function($row) {
	return 'user_' . $row->id;
});
```

---

<a name="setting-the-row-class"></a>
## Setting the Row Class (`setRowClass`)

Use the `setRowClass` method to add a CSS class to each row. Accepts a string, column name, or a closure for dynamic classes.

**Example:**

```php
$dataTable->setRowClass('my-custom-class');

$dataTable->setRowClass(function($row) {
	return $row->status === 'active' ? 'table-success' : 'table-danger';
});
```

---

<a name="setting-row-data-attributes"></a>
## Setting Row Data Attributes (`setRowData` and `addRowData`)

Use `setRowData` to set multiple `data-*` attributes for each row, or `addRowData` to add them one by one. Values can be static or generated via closure.

**Example:**

```php
$dataTable->setRowData([
	'user-id' => 'id',
	'role' => function($row) { return $row->role; },
]);

$dataTable->addRowData('foo', 'bar');
$dataTable->addRowData('dynamic', function($row) { return $row->some_value; });
```

---

<a name="how-it-works"></a>
## How It Works

These methods set templates that are compiled for each row. You can use column names or closures for dynamic values. The resulting HTML will look like:

```html
<tr id="user_1" class="table-success" data-user-id="1" data-role="admin">
	...
</tr>
```

---

<a name="summary-table"></a>
## Summary Table

| Method         | Purpose                | Example Usage                                  |
|--------------- |-----------------------|-------------------------------------------------|
| setRowId       | Set `<tr id="...">`   | `$dt->setRowId('id')`                           |
| setRowClass    | Set `<tr class="...">`| `$dt->setRowClass('active')`                    |
| setRowData     | Set `data-*` attrs     | `$dt->setRowData(['foo' => 'bar'])`             |
| addRowData     | Add single `data-*`    | `$dt->addRowData('foo', 'bar')`                 |

---

For more details, see the [core implementation](https://github.com/yajra/laravel-datatables).
