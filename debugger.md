---
title: Debugging Mode
description: Enable debugging to see queries and inputs used in DataTables
---

# Debugging Mode

Laravel DataTables includes a debugging mode that helps you inspect the SQL queries and request parameters being used when processing DataTables requests.

---

<a name="setup"></a>
## Setup

1. Set `APP_DEBUG=true` in your `.env` file:

```env
APP_DEBUG=true
```

2. Make sure `APP_ENV` is set to `local` or `testing` in development environments.

> [!WARNING]
> Never enable `APP_DEBUG=true` in production. Exposing SQL queries and application internals is a security risk.

---

## How It Works

When `APP_DEBUG=true`, Laravel DataTables automatically intercepts the response and appends debugging information. The debug data is only included when:
- `APP_DEBUG` is `true`
- The request is an AJAX/DataTables request
- The response is in JSON format

---

<a name="example"></a>
## Example Response

When debugging is enabled, your JSON response includes additional `queries` and `input` fields:

```json
{
    "draw": 1,
    "recordsTotal": 10,
    "recordsFiltered": 3,
    "data": [
        {
            "id": 414,
            "name": "Abel Cole",
            "email": "jklein@block.com",
            "created_at": "2016-07-31 23:26:10",
            "updated_at": "2016-07-31 23:26:10"
        }
    ],
    "queries": [
        {
            "query": "select count(*) as aggregate from (select '1' as `row_count` from `users` where `users`.`deleted_at` is null) count_row_table",
            "bindings": [],
            "time": 1.84
        },
        {
            "query": "select * from `users` where `users`.`deleted_at` is null order by `name` asc limit 10 offset 0",
            "bindings": [],
            "time": 1.8
        }
    ],
    "input": {
        "draw": "1",
        "columns": [
            {
                "data": "id",
                "name": "",
                "searchable": "true",
                "orderable": "true",
                "search": {
                    "value": "",
                    "regex": "false"
                }
            }
        ],
        "order": [
            {
                "column": "1",
                "dir": "asc"
            }
        ],
        "start": "0",
        "length": "10",
        "search": {
            "value": "",
            "regex": "false"
        }
    }
}
```

---

<a name="fields"></a>
## Debug Output Fields

| Field | Description |
|-------|-------------|
| `queries` | Array of executed SQL queries with bindings and execution time |
| `input` | The complete DataTables request parameters including columns, order, and search settings |

### Query Object Structure

Each query object contains:

| Property | Type | Description |
|----------|------|-------------|
| `query` | string | The SQL query string with placeholder bindings |
| `bindings` | array | Parameter values bound to the query |
| `time` | float | Execution time in milliseconds |

---

## See Also

- [Error Handler](/docs/{{package}}/{{version}}/error-handler) - Configure error handling
- [General Settings](/docs/{{package}}/{{version}}/general-settings) - Configuration options
