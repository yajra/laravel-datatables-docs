---
title: Fractal Transformer Serializer
description: Configure custom serializers for Fractal transformers
---

# Fractal Transformer Serializer

You can set the serializer to be used by DataTables using the `setSerializer` API. Serializer should be used with `setTransformer` API.

---

<a name="basic"></a>
## Basic Usage

```php
use Yajra\DataTables\Facades\DataTables;

Route::get('user-data', function () {
    $model = App\Models\User::query();

    return DataTables::eloquent($model)
        ->setTransformer(new App\Transformers\UserTransformer)
        ->setSerializer(new App\Serializers\CustomSerializer)
        ->toJson();
});
```

---

## Available Serializers

The Fractal package provides several built-in serializers:

| Serializer | Description |
|------------|-------------|
| `DataArraySerializer` | Returns data as a plain array (default) |
| `ArraySerializer` | Returns data wrapped in a data key |
| `JsonApiSerializer` | Returns data in JSON API format |

---

## Creating a Custom Serializer

```php
<?php
// app/Serializers/CustomSerializer.php

namespace App\Serializers;

use League\Fractal\Serializer\ArraySerializer;

class CustomSerializer extends ArraySerializer
{
    // Override methods as needed
}
```

---

## Example with JSON API

```php
use Yajra\DataTables\Facades\DataTables;
use App\Transformers\UserTransformer;
use League\Fractal\Serializer\JsonApiSerializer;

Route::get('user-data', function () {
    $model = App\Models\User::query();

    return DataTables::eloquent($model)
        ->setTransformer(new UserTransformer)
        ->setSerializer(new JsonApiSerializer)
        ->toJson();
});
```

---

## See Also

- [Fractal Plugin Installation](/docs/{{package}}/{{version}}/fractal-installation) - Get started with the Fractal plugin
- [Response using Transformer](/docs/{{package}}/{{version}}/response-fractal) - Learn how to transform DataTables responses using Fractal transformers
