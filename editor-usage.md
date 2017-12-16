# Using DataTables Editor

All actions requested by DataTables Editor are being submitted via `POST` ajax request.
This means, that we need to create a `post` request route that will handle all the actions we need.

> This doc assumes that you are already knowlegeable of [DataTables Editor](https://editor.datatables.net/examples/index) library.

<a name="create-editor"></a>
## Create your Editor

You can create your editor using [artisan command](/docs/{{package}}/{{version}}/editor-command).

```bash
php artisan datatables:editor Users
```

<a name="setup-editor-rules"></a>
## Setup Editor Rules

See [editor rules](/docs/{{package}}/{{version}}/editor-rules) docs for ref:

<a name="register-route"></a>
## Register Route Handler

```php
use App\DataTables\UsersDataTablesEditor;

Route::post('editor', function(UsersDataTablesEditor $editor) {
    return $editor->process(request());
});
```

<a name="setup-csrf"></a>
## Setup AJAX csrf-token

Since actions are being sent via `post`, we need to make sure that we setup [csrf-token](https://laravel.com/docs/csrf#csrf-x-csrf-token).
Just add the snippets below before your scripts to avoid csrf errors:

```js
$.ajaxSetup({
    headers: {
        'X-CSRF-TOKEN': $('meta[name="csrf-token"]').attr('content')
    }
});
```

<a name="setup-content"></a>
## Setup your content

You can use [DataTables Editor Generator](https://editor.datatables.net/generator/index) to help you speed-up the process.
Once generated, copy the necessary scripts and html on your blade template.
