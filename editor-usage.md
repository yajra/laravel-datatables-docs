# Using DataTables Editor

All actions requested by DataTables Editor are being submitted via `POST` ajax request.
This means, that we need to create a `post` request route that will handle all the actions we need.

> {info} This doc assumes that you are already knowlegeable of [DataTables Editor](https://editor.datatables.net/examples/index) library.

## Create your Editor

You can create your editor using [artisan command](/docs/{{package}}/{{version}}/editor-command).

```bash
php artisan datatables:editor Users
```

## Setup Editor Rules

See [editor rules](/docs/{{package}}/{{version}}/editor-rules) docs for ref:

## Register Route Handler

```php
use App\DataTables\UsersDataTablesEditor;

Route::post('editor', function(UsersDataTablesEditor $editor) {
    return $editor->process(request());
});
```

## Setup your content

You can use [DataTables Editor Genetor](https://editor.datatables.net/generator/index) to help you speed-up the process.
Once generated, copy the necessary scripts and html on your blade template.
