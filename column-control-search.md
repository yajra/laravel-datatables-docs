---
title: Column Control Search
description: Advanced column search with control logic for DataTables
---

# Column Control Search

The `columnControlSearch()` API provides advanced search functionality with control logic. This allows DataTables to send column control search parameters including logic operators, value lists, and date masks.

---

<a name="overview"></a>
## Overview

Column control search supports multiple search types:

| Type | Logic Options | Description |
|------|--------------|-------------|
| `text` | `equal`, `not`, `contains`, `notContains`, `starts`, `ends` | Text-based searches |
| `num` | `equal`, `not`, `greater`, `less`, `greaterOrEqual`, `lessOrEqual` | Numeric comparisons |
| `date` | `equal`, `notEqual`, `greater`, `less`, `greaterOrEqual`, `lessOrEqual` | Date comparisons |

---

<a name="frontend"></a>
## Frontend Setup

Enable column control search in your JavaScript:

```javascript
$('#users-table').DataTable({
    processing: true,
    serverSide: true,
    ajax: '/user-data',
    columns: [
        { data: 'id', name: 'id' },
        { data: 'name', name: 'name' },
        { data: 'created_at', name: 'created_at' }
    ],
    searchPanes: {
        controls: true
    }
});
```

---

<a name="backend"></a>
## Backend Handling

The backend automatically processes column control search parameters. Use `filterColumn()` for custom handling:

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;
use Illuminate\Database\Eloquent\Builder;

Route::get('user-data', function() {
    return DataTables::eloquent(User::query())
        ->filterColumn('status', function(Builder $query, $keyword) {
            // Handle comma-separated values from list control
            $values = explode(', ', $keyword);
            $query->whereIn('status', $values);
        })
        ->toJson();
});
```

---

<a name="supported-operators"></a>
## Supported Operators

### Text Operators
- `equal` - Exact match
- `not` - Not equal
- `contains` - Contains substring
- `notContains` - Does not contain
- `starts` - Starts with
- `ends` - Ends with
- `empty` - Is empty
- `notEmpty` - Is not empty

### Numeric Operators
- `equal` - Equal to
- `not` - Not equal to
- `greater` - Greater than
- `less` - Less than
- `greaterOrEqual` - Greater than or equal
- `lessOrEqual` - Less than or equal

### Date Operators
- All numeric operators plus:
- `notEqual` - Date not equal (excludes NULL values)

---

## See Also

- [Filter Column](/docs/{{package}}/{{version}}/filter-column) - Custom column filtering
- [SearchPanes Extension](/docs/{{package}}/{{version}}/search-panes-starter) - SearchPanes integration
