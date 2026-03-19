---
title: Error Handler
description: Configure error handling for DataTables
---

# Error Handler

Laravel DataTables allows you to configure how server-side errors are handled when processing DataTables requests. Instead of exposing raw stack traces, you can customize the error response format.

---

<a name="config"></a>
## Configuration

Error handling is configured in `config/datatables.php` under the `error` key:

```php
'error' => env('DATATABLES_ERROR', null),
```

Alternatively, set the `DATATABLES_ERROR` environment variable in your `.env` file:

```env
# Display actual exception message
DATATABLES_ERROR=null

# Throw an exception for custom handling
DATATABLES_ERROR=throw

# Display a custom message
DATATABLES_ERROR="An error occurred while loading data"
```

---

## Available Options

| Option | Description |
|--------|-------------|
| `null` | Display the actual exception message (default) |
| `'throw'` | Throw a `\Yajra\DataTables\Exceptions\Exception` for custom handling |
| Custom string | Display a custom friendly message |

---

<a name="null"></a>
## NULL Error (Default)

When set to `null`, the actual exception message is returned in the response:

```json
{
    "draw": 24,
    "recordsTotal": 200,
    "recordsFiltered": 0,
    "data": [],
    "error": "Exception Message:\n\nSQLSTATE[42S22]: Column not found: 1054 Unknown column 'xxx' in 'order clause'"
}
```

> [!TIP]
> This is useful during development to quickly identify issues.

---

<a name="throw"></a>
## THROW Error

When set to `'throw'`, the package throws a `\Yajra\DataTables\Exceptions\Exception`. This allows you to catch and handle the error with your own logic.

```php
<?php

use Illuminate\Foundation\Application;
use Illuminate\Foundation\Configuration\Exceptions;
use Illuminate\Http\Request;
use Symfony\Component\HttpKernel\Exception\HttpException;
use Yajra\DataTables\Exceptions\Exception;

return Application::configure(basePath: dirname(__DIR__))
    ->withExceptions(function (Exceptions $exceptions): void {
        $exceptions->render(function (Exception $e, Request $request) {
            if ($request->expectsJson() || $request->is('datatables/*')) {
                return response()->json([
                    'draw'            => 0,
                    'recordsTotal'    => 0,
                    'recordsFiltered' => 0,
                    'data'            => [],
                    'error'           => $e->getMessage(),
                ], 500);
            }
        });
    })->create();
```

### Custom HTTP Exceptions

You can also handle specific HTTP exceptions:

```php
->withExceptions(function (Exceptions $exceptions): void {
    $exceptions->render(function (HttpException $e, Request $request) {
        if ($request->expectsJson() || $request->is('datatables/*')) {
            return response()->json([
                'draw'            => 0,
                'recordsTotal'    => 0,
                'recordsFiltered' => 0,
                'data'            => [],
                'error'           => $e->getMessage(),
            ], $e->getStatusCode());
        }
    });
})
```

---

<a name="custom"></a>
## Custom Message

If set to any custom string, that message will be displayed instead of the exception:

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

- [Debugging Mode](/docs/{{package}}/{{version}}/debugger) - Enable debug mode to troubleshoot issues
- [General Settings](/docs/{{package}}/{{version}}/general-settings) - Configuration options
