---
title: Error Handler
description: Configure error handling for DataTables
---

# Error Handler

Laravel DataTables allows you to configure how you want to handle server-side errors when processing your request.

---

<a name="config"></a>
## Configuration

Configuration is located at `config/datatables.php` under `error` key. You can also configure via env by setting `DATATABLES_ERROR` key appropriately.

The default configuration is `env('DATATABLES_ERROR', null)`.

---

## Available Options

| Option | Description |
|--------|-------------|
| `null` | Display the actual exception message |
| `'throw'` | Throw a `\Yajra\DataTables\Exception` |
| Custom message | Display a custom friendly message |
| Translation key | Use a translation key for localization |

---

## NULL Error (Default)

If set to `null`, the actual exception message will be used on error response:

```json
{
    "draw": 24,
    "recordsTotal": 200,
    "recordsFiltered": 0,
    "data": [],
    "error": "Exception Message:\n\nSQLSTATE[42S22]: Column not found: 1054 Unknown column 'xxx' in 'order clause'"
}
```

---

## THROW Error

If set to `'throw'`, the package will throw a `\Yajra\DataTables\Exception`. You can then use your custom error handler if needed:

```php
<?php
// app/Exceptions/Handler.php

namespace App\Exceptions;

use Exception;
use Illuminate\Foundation\Exceptions\Handler as ExceptionHandler;

class Handler extends ExceptionHandler
{
    /**
     * Render an exception into an HTTP response.
     */
    public function render($request, Exception $exception)
    {
        if ($exception instanceof \Yajra\DataTables\Exception) {
            return response()->json([
                'draw'            => 0,
                'recordsTotal'    => 0,
                'recordsFiltered' => 0,
                'data'            => [],
                'error'           => 'Laravel Error Handler',
            ]);
        }

        return parent::render($request, $exception);
    }
}
```

---

## Custom Message

If set to `'any custom message'` or `'translation.key'`, this message will be used when an error occurs:

```json
{
    "draw": 24,
    "recordsTotal": 200,
    "recordsFiltered": 0,
    "data": [],
    "error": "any custom message"
}
```

---

## See Also

- [Debugging Mode](/docs/{{package}}/{{version}}/debugger) - Enable debug mode
- [General Settings](/docs/{{package}}/{{version}}/general-settings) - Configuration options
