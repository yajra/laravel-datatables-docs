---
title: Debugging Mode
description: Enable debugging to see queries and inputs used in DataTables
---

# Debugging Mode

To enable debugging mode, set `APP_DEBUG=true` in your environment. The package will then include the queries and inputs used when processing the table.

> [!WARNING]
> Please make sure that `APP_DEBUG` is set to `false` when your app is in production. Exposing query information can be a security risk.

---

## Setup

1. Set `APP_DEBUG=true` in your `.env` file
2. Update the [Error Handler](/docs/{{package}}/{{version}}/error-handler) config appropriately

---

## Example Response

When debugging is enabled, your JSON response will include additional `queries` and `input` fields:

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

## Debug Output Fields

| Field | Description |
|-------|-------------|
| `queries` | Array of executed SQL queries with bindings and execution time |
| `input` | The complete DataTables request input including columns, order, and search parameters |

---

## See Also

- [Error Handler](/docs/{{package}}/{{version}}/error-handler) - Configure error handling
- [General Settings](/docs/{{package}}/{{version}}/general-settings) - Configuration options
