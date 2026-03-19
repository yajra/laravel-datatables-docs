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

# Use a translation key
DATATABLES_ERROR="datatables.error_message"
```

---

## Available Options

| Option | Description |
|--------|-------------|
| `null` | Display the actual exception message (default) |
| `'throw'` | Throw a `\Yajra\DataTables\Exception` for custom handling |
| Custom string | Display a custom friendly message |
| Translation key | Use a translation key for localization |

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

When set to `'throw'`, the package throws a `\Yajra\DataTables\Exception`. This allows you to catch and handle the error with your own logic.

### Laravel 13 / 12 / 11+

In modern Laravel versions, register an exception handler in `bootstrap/app.php`:

```php
use Illuminate\Foundation\Exceptions\Handler as ExceptionHandler;
use Illuminate\Http\Request;
use Yajra\DataTables\Exception;
use Throwable;

return Application::configure(basePath: dirname(__DIR__))
    ->withExceptions(function (ExceptionHandler $exceptions) {
        $exceptions->render(function (Exception $e, Request $request) {
            if ($request->expectsJson() || $request->is('datatables/*')) {
                return response()->json([
                    'draw'            => 0,
                    'recordsTotal'    => 0,
                    'recordsFiltered' => 0,
                    'data'            => [],
                    'error'           => 'An error occurred while loading the data.',
                ], 500);
            }
        })->stop();
    })->create()->bind('app', Application::class);
```

### Alternative: Separate Handler Class

For cleaner organization, create a dedicated handler class:

**app/Exceptions/DataTableExceptionHandler.php**
```php
<?php

namespace App\Exceptions;

use App\Http\Responses\DataTableErrorResponse;
use Illuminate\Http\Request;
use Yajra\DataTables\Exception;
use Throwable;

class DataTableExceptionHandler
{
    public function handle(Throwable $e, Request $request): ?JsonResponse
    {
        if (! $e instanceof Exception) {
            return null;
        }

        if ($request->expectsJson() || $request->is('datatables/*')) {
            return response()->json([
                'draw'            => 0,
                'recordsTotal'    => 0,
                'recordsFiltered' => 0,
                'data'            => [],
                'error'           => 'An error occurred while loading the data.',
            ], 500);
        }

        return null;
    }
}
```

Then register it in `bootstrap/app.php`:

```php
return Application::configure(basePath: dirname(__DIR__))
    ->withExceptions(function (ExceptionHandler $exceptions) {
        $exceptions->handle(function (Throwable $e, Request $request) {
            $handler = new DataTableExceptionHandler();
            $response = $handler->handle($e, $request);
            
            if ($response !== null) {
                return $response;
            }
        });
    })->create()->bind('app', Application::class);
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

### Using Translation Keys

You can also use translation keys for localization:

```php
// config/datatables.php
'error' => 'datatables.error.generic',
```

```php
// resources/lang/en/datatables.php
return [
    'error' => [
        'generic' => 'An error occurred while loading the data. Please try again.',
        'not_found' => 'The requested data could not be found.',
        'unauthorized' => 'You are not authorized to view this data.',
    ],
];
```

---

## See Also

- [Debugging Mode](/docs/{{package}}/{{version}}/debugger) - Enable debug mode to troubleshoot issues
- [General Settings](/docs/{{package}}/{{version}}/general-settings) - Configuration options
