# Fractal Transformer Serializer

You can set the serializer to be used by DataTables using `setSerializer` api. Serializer should be used with `setTransformer` api.

```php
use DataTables;

Route::get('user-data', function () {
    $model = App\User::query();

    return DataTables::eloquent($model)
        ->setTransformer(new App\Transformers\UserTransformer)
        ->setSerializer(new App\Serializers\CustomSerializer)
        ->toJson();
});
```

### Available Serializers

The Fractal package provides several built-in serializers:

- **DataArraySerializer** - Returns data as a plain array (default)
- **ArraySerializer** - Returns data wrapped in a data key
- **JsonApiSerializer** - Returns data in JSON API format

### Creating a Custom Serializer

```php
namespace App\Serializers;

use League\Fractal\Serializer\ArraySerializer;

class CustomSerializer extends ArraySerializer
{
    // Override methods as needed
}
```

## See Also

- [Fractal Plugin Installation](/docs/fractal-installation) - Get started with the Fractal plugin
- [Response using Transformer](/docs/response-fractal) - Learn how to transform DataTables responses using Fractal transformers
