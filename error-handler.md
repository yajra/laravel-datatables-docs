# Error Handler

Laravel DataTables allows you to configure how you want to handle server-side errors when processing your request. 
Below are the options available for error handling.

## ERROR CONFIGURATIONS
Configuration is located at `config/datatables.php` under `error` key.


- [NULL](#null-error) : `'error' => null`
- [THROW](#throw-error) : `'error' => 'throw'`
- [CUSTOM MESSAGE](#custom-message) : `'error' => 'Any custom friendly message'`
- [TRANSLATION](#custom-message) : `'error' => 'translation.key'`

<a name="null-error"></a>
## NULL Error
If set to `null`, the actual exception message will be used on error response.

```json
{
	"draw": 24,
	"recordsTotal": 200,
	"recordsFiltered": 0,
	"data": [],
	"error": "Exception Message:\n\nSQLSTATE[42S22]: Column not found: 1054 Unknown column 'xxx' in 'order clause' (SQL: select * from `users` where `users`.`deleted_at` is null order by `xxx` asc limit 10 offset 0)"
}
```

<a name="throw-error"></a>
## THROW Error
If set to `'throw'`, the package will throw a `\Yajra\Datatables\Exception`. 
You can then use your custom error handler if needed.

**Example Error Handler**

Update `app\Exceptions\Handler.php` and register dataTables error exception handler.

```php
    /**
     * Render an exception into an HTTP response.
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  \Exception  $exception
     * @return \Illuminate\Http\Response
     */
    public function render($request, Exception $exception)
    {
        if ($exception instanceof \Yajra\Datatables\Exception) {
            return response([
                'draw'            => 0,
                'recordsTotal'    => 0,
                'recordsFiltered' => 0,
                'data'            => [],
                'error'           => 'Laravel Error Handler',
            ]);
        }

        return parent::render($request, $exception);
    }
```

<a name="custom-message"></a>
## Custom Message
If set to `'any custom message'` or `'translation.key'`, this message will be used when an error occurs when processing the request.

```json
{
	"draw": 24,
	"recordsTotal": 200,
	"recordsFiltered": 0,
	"data": [],
	"error": "any custom message"
}
```


