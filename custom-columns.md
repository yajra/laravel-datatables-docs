---
title: Custom Column Classes
description: Create reusable column classes with predefined styling, rendering, and export configurations
---

# Custom Column Classes

Create reusable column classes to standardize column formatting across your application. Custom columns encapsulate styling, JavaScript rendering, and export configurations into a single, reusable component.

---

<a name="when-to-create"></a>
## When to Create Custom Column Classes

Consider creating custom column classes when you:

- Have **repeated column configurations** across multiple DataTables
- Want to **standardize formatting** for specific data types (money, dates, percentages)
- Need **consistent styling** and rendering across different views
- Want to **centralize export formatting** logic

---

<a name="basic-structure"></a>
## Basic Structure

Custom column classes extend `BaseColumn` and configure the column in the constructor:

```php
<?php
declare(strict_types=1);

namespace App\DataTables\Columns;

use Yajra\DataTables\Html\Editor\Columns\BaseColumn;

final class MoneyColumn extends BaseColumn
{
    public function __construct(array $attributes = [])
    {
        parent::__construct($attributes);

        // Configure the column here
    }
}
```

---

<a name="available-methods"></a>
## Available Methods

### addClass()

Add CSS class names to the column:

```php
$this->addClass('text-right');
```

### renderJs()

Set a JavaScript rendering function for the DataTables display:

```php
$this->renderJs('number(",", ".", 2, "", "")');
```

The function formats numbers with thousand separators and decimal places using DataTables' `number()` helper.

### exportFormat()

Set the Excel number format for exports:

```php
$this->exportFormat('#,##0.00');
```

Common formats:
- `#,##0.00` - Number with 2 decimals
- `$#,##0.00` - Currency
- `0.00%` - Percentage
- `mm/dd/yyyy` - Date

### exportRender()

Define a callback to transform values during export:

```php
$this->exportRender(fn (mixed $row, mixed $value): float => round((float) ($value ?? 0), 2));
```

The callback receives:
- `$row` - The model/row instance
- `$value` - The raw column value

Returns the formatted value for export.

---

<a name="money-column-example"></a>
## Example: MoneyColumn

This `MoneyColumn` class configures right-aligned display, JavaScript number formatting, and Excel-compatible export:

```php
<?php
declare(strict_types=1);

namespace App\Editor\Columns;

final class MoneyColumn extends BaseColumn
{
    public function __construct(array $attributes = [])
    {
        parent::__construct($attributes);

        $this->addClass('text-right')
            ->renderJs('number(",", ".", 2, "", "")')
            ->exportFormat('#,##0.00')
            ->exportRender(fn (mixed $row, mixed $value): float => round((float) ($value ?? 0), 2));
    }
}
```

**Usage in DataTable:**

```php
use App\DataTables\Columns\MoneyColumn;

protected function getColumns(): array
{
    return [
        // ...
        MoneyColumn::make('price', 'orders.price'),
        MoneyColumn::make('total', 'orders.total'),
        // ...
    ];
}
```

---

<a name="other-use-cases"></a>
## Other Use Cases

### PercentageColumn

```php
<?php
declare(strict_types=1);

namespace App\DataTables\Columns;

final class PercentageColumn extends BaseColumn
{
    public function __construct(array $attributes = [])
    {
        parent::__construct($attributes);

        $this->addClass('text-center')
            ->renderJs('function(data) { return (data * 100).toFixed(2) + "%"; }')
            ->exportFormat('0.00%')
            ->exportRender(fn (mixed $row, mixed $value): float => round((float) ($value ?? 0) * 100, 2));
    }
}
```

### PhoneColumn

```php
<?php
declare(strict_types=1);

namespace App\DataTables\Columns;

final class PhoneColumn extends BaseColumn
{
    public function __construct(array $attributes = [])
    {
        parent::__construct($attributes);

        $this->addClass('text-center')
            ->exportFormat('@') // Text format in Excel
            ->exportRender(fn (mixed $row, mixed $value): string => preg_replace('/[^0-9+]/', '', $value ?? ''));
    }
}
```

---

<a name="namespace-organization"></a>
## Namespace Organization

Place custom column classes in a dedicated namespace:

```
app/
└── DataTables/
    └── Columns/
        ├── MoneyColumn.php
        ├── PercentageColumn.php
        └── PhoneColumn.php
```

Or for Editor-specific columns:

```
app/
└── Editor/
    └── Columns/
        └── MoneyColumn.php
```

---

## See Also

- [Add Column](/docs/{{package}}/{{version}}/add-column) - Add computed columns
- [Edit Column](/docs/{{package}}/{{version}}/edit-column) - Modify existing columns
- [Export Column](/docs/{{package}}/{{version}}/export-column) - Export configuration
- [Export Options](/docs/{{package}}/{{version}}/exports-options) - Export format options
