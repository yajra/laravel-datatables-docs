# HTML Builder Config

The HTML Builder configuration allows you to set default table attributes that will be applied to all tables rendered using the builder class.

## Installation

To begin, you need to publish the config file:

```bash
php artisan vendor:publish --tag=datatables-html
```

## Configuration File

The published config file is located at `config/datatables-html.php`.

## Configuration Options

### Table Attributes

Set default table attributes:

```php
return [
    'table' => [
        'class' => 'table table-bordered',
        'id' => 'dataTable',
    ]
];
```

### Script Template

Set the default script template:

```php
return [
    'script' => 'datatables::script',
];
```

## Complete Example

```php
return [
    // Default table attributes
    'table' => [
        'class' => 'table table-striped table-bordered',
        'id' => 'dataTable',
        'width' => '100%',
        'cellspacing' => '0',
    ],

    // Default script template
    'script' => 'datatables::script',
];
```

## Using Default Configuration

After setting up the default configuration, all tables will automatically use these attributes:

```php
Route::get('users', function(Builder $builder) {
    if (request()->ajax()) {
        return DataTables::of(User::query())->toJson();
    }

    // The table will automatically have class="table table-striped table-bordered" and id="dataTable"
    $html = $builder->columns([...]);

    return view('users.index', compact('html'));
});
```

## Overriding Defaults

You can still override the defaults when calling `table()`:

```php
// Override the default class
echo $html->table(['class' => 'table table-hover']);

// Override the default id
echo $html->table(['id' => 'custom-table']);
```

## See Also

- [Html Builder](/docs/{{package}}/{{version}}/html-builder)
