---
title: Row Options
description: Configure DataTables row attributes like ID, class, and data
---

# Row Options

You can configure various row attributes in your DataTables including row ID, class, data, and custom attributes.

---

## Row Id

### Setting Row ID via Column Name

```php
->setRowId('id')
```

### Setting Row ID via Closure

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    return DataTables::eloquent(User::query())
        ->setRowId(function ($user) {
            return $user->id;
        })
        ->toJson();
});
```

### Setting Row ID via Blade String

```php
->setRowId('{{$id}}')
```

---

## Row Class

### Setting Row Class via Closure

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    return DataTables::eloquent(User::query())
        ->setRowClass(function ($user) {
            return $user->id % 2 == 0 ? 'alert-success' : 'alert-warning';
        })
        ->toJson();
});
```

### Setting Row Class via Blade String

```php
->setRowClass('{{ $id % 2 == 0 ? "alert-success" : "alert-warning" }}')
```

---

## Row Data

### Setting Row Data via Closure

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    return DataTables::eloquent(User::query())
        ->setRowData([
            'data-id' => function($user) {
                return 'row-' . $user->id;
            },
            'data-name' => function($user) {
                return 'row-' . $user->name;
            },
        ])
        ->toJson();
});
```

### Setting Row Data via Blade String

```php
->setRowData([
    'data-id' => 'row-{{$id}}',
    'data-name' => 'row-{{$name}}',
])
```

---

## Row Attributes

### Setting Row Attribute via Closure

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    return DataTables::eloquent(User::query())
        ->setRowAttr([
            'color' => function($user) {
                return $user->color;
            },
        ])
        ->toJson();
});
```

### Setting Row Attribute via Blade String

```php
->setRowAttr([
    'color' => '{{$color}}',
])
```

---

## See Also

- [Row Id](/docs/{{package}}/{{version}}/row-id) - Setting row IDs
- [Row Class](/docs/{{package}}/{{version}}/row-class) - Setting row classes
