---
title: Row Options
description: Configure DataTables row attributes like ID, class, and data
---

# Row Options

Row options allow you to customize HTML attributes on table rows (`<tr>` elements). This is useful for:

- **Styling rows dynamically** based on data (e.g., highlight active vs inactive users)
- **Enabling JavaScript interactions** via `data-*` attributes
- **Facilitating row selection** by setting unique IDs

---

## Value Types

All row methods accept three value types:

| Type | Example | Description |
|------|---------|-------------|
| **String** | `'my-class'` | A static value |
| **Column name** | `'id'` | Uses the column's value from the row |
| **Closure** | `function($row) { }` | Custom logic returning a value |

---

<a name="row-id"></a>
## Setting the Row ID (`setRowId`) {#row-id}

Use `setRowId` to set the `id` attribute on each `<tr>` element. Each row ID should be unique.

### Using a Column Name

```php
$dataTable->setRowId('id');
```

**Result:**
```html
<tr id="1">...</tr>
```

### Using a Closure

```php
$dataTable->setRowId(function ($row) {
    return 'user_' . $row->id;
});
```

**Result:**
```html
<tr id="user_1">...</tr>
```

### Using Blade String

```php
$dataTable->setRowId('{{$id}}');
```

---

<a name="row-class"></a>
## Setting the Row Class (`setRowClass`) {#row-class}

Use `setRowClass` to add CSS classes to each row. Great for conditional styling based on row data.

### Static Class

```php
$dataTable->setRowClass('my-custom-class');
```

### Dynamic Class with Closure

```php
$dataTable->setRowClass(function ($row) {
    return match($row->status) {
        'active'   => 'table-success',
        'inactive' => 'table-secondary',
        'banned'   => 'table-danger',
        default    => '',
    };
});
```

**Result:**
```html
<tr class="table-success">...</tr>
```

### Using Blade String

```php
$dataTable->setRowClass('{{ $id % 2 == 0 ? "alert-success" : "alert-warning" }}');
```

---

<a name="row-data"></a>
## Setting Row Data Attributes (`setRowData`) {#row-data}

Use `setRowData` to set multiple `data-*` attributes for each row. These are useful for passing data to JavaScript.

### Multiple Data Attributes

```php
$dataTable->setRowData([
    'user-id'    => 'id',
    'user-name'  => 'name',
    'user-email' => 'email',
]);
```

**Result:**
```html
<tr data-user-id="1" data-user-name="John" data-user-email="john@example.com">...</tr>
```

### Using Closures for Dynamic Values

```php
$dataTable->setRowData([
    'user-id' => 'id',
    'role'    => function($row) { return $row->role->name ?? 'guest'; },
]);
```

### Adding Single Data Attributes

```php
$dataTable->addRowData('foo', 'bar');
$dataTable->addRowData('dynamic', function($row) { return $row->some_value; });
```

---

<a name="row-attributes"></a>
## Setting Row Attributes (`setRowAttr`) {#row-attributes}

Use `setRowAttr` to set custom HTML attributes (not starting with `data-`).

### Multiple Attributes

```php
$dataTable->setRowAttr([
    'color'      => 'blue',
    'data-custom' => function($row) { return $row->custom_value; },
]);
```

### Adding Single Attributes

```php
$dataTable->addRowAttr('foo', 'bar');
$dataTable->addRowAttr('dynamic', function($row) { return $row->some_value; });
```

---

## Complete Example

Here's a DataTable class combining multiple row options:

```php
use App\Models\User;
use Yajra\DataTables\Html\Builder as HtmlBuilder;
use Yajra\DataTables\Services\DataTable;

class UsersDataTable extends DataTable
{
    public function html(): HtmlBuilder
    {
        return $this->builder()
            ->setTableId('users-table')
            ->columns($this->getColumns())
            ->setRowId('id')
            ->setRowClass(function ($row) {
                return $row->is_active ? 'table-success' : 'table-secondary';
            })
            ->setRowData([
                'user-id'   => 'id',
                'user-name' => 'name',
                'role'      => fn($row) => $row->role?->name ?? 'guest',
            ])
            ->setRowAttr([
                'color' => fn($row) => $row->preference_color,
            ]);
    }
}
```

---

## Using Data Attributes in JavaScript

Once you've set `data-*` attributes, you can access them in JavaScript:

```javascript
$('#users-table tbody').on('click', 'tr', function() {
    const userId = $(this).data('user-id');
    const userName = $(this).data('user-name');
    const role = $(this).data('role');
    
    console.log(`Clicked user: ${userName} (ID: ${userId}, Role: ${role})`);
});
```

---

## How It Works

These methods set templates that are compiled for each row. You can use column names or closures for dynamic values.

**Example Result:**

```html
<tr id="1" class="table-success" data-user-id="1" data-user-name="John" data-role="admin" color="blue">
    <td>1</td>
    <td>John</td>
    <td>john@example.com</td>
</tr>
```

---

## Summary Table

| Method | Purpose | Example |
|--------|---------|---------|
| `setRowId` | Set `<tr id="...">` | `$dt->setRowId('id')` |
| `setRowClass` | Set `<tr class="...">` | `$dt->setRowClass('active')` |
| `setRowData` | Set `data-*` attributes | `$dt->setRowData(['key' => 'value'])` |
| `addRowData` | Add single `data-*` | `$dt->addRowData('key', 'value')` |
| `setRowAttr` | Set custom attributes | `$dt->setRowAttr(['attr' => 'value'])` |
| `addRowAttr` | Add single attribute | `$dt->addRowAttr('attr', 'value')` |

---

## See Also

- [HTML Builder](/docs/{{package}}/{{version}}/html-builder) - Table configuration
- [HTML Builder Column](/docs/{{package}}/{{version}}/html-builder-column) - Column options
