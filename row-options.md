# Row Editing Options

- [Row Id](#row-id)
- [Row Class](#row-class)
- [Row Data](#row-data)
- [Row Attributes](#row-attributes)

<a name="row-id"></a>
## Row Id

Setting row id via `column` name.

```php
->setRowId('id')
```

Setting row id via `closure`.

```php
->setRowId(function ($user) {
    return $user->id;
})
```

Setting row id via `blade` string.

```php
->setRowId('{{$id}}')
```

<a name="row-class"></a>
## Row Class

Setting row class via `closure`.

```php
->setRowClass(function ($user) {
    return $user->id % 2 == 0 ? 'alert-success' : 'alert-warning';
})
```

Setting row class via `blade` string.

```php
->setRowClass('{{ $id % 2 == 0 ? "alert-success" : "alert-warning" }}')
```


<a name="row-data"></a>
## Row Data

Setting row class via `closure`.

```php
->setRowData([
    'data-id' => function($user) {
    	return 'row-' . $user->id;
    },
    'data-name' => function($user) {
    	return 'row-' . $user->name;
    },
])
```

Setting row class via `blade` string.

```php
->setRowData([
    'data-id' => 'row-{{$id}}',
    'data-name' => 'row-{{$name}}',
])
```

<a name="row-attributes"></a>
## Row Attributes

Setting row class via `closure`.

```php
->setRowAttr([
    'color' => function($user) {
    	return $user->color;
    },
])
```

Setting row class via `blade` string.

```php
->setRowAttr([
    'color' => '{{$color}}',
])
```