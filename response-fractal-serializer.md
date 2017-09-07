# Fractal Transformer Serializer

You can set the serializer to be used by DataTables using `setSerializer` api. Serializer should be used with `setTransformer` api.

```php
use DataTables;

Route::get('user-data', function() {
	$model = App\User::query();

	return DataTables::eloquent($model)
					->setTransformer(new App\Transformers\UserTransformer)
					->setSerializer(new App\Serializers\CustomSerializer)
					->make(true);
});
```
