# Fractal Transformer Serializer

You can set the serializer to be used by Datatables using `setSerializer` api. Serializer should be used with `setTransformer` api.

```php
use Datatables;

Route::get('user-data', function() {
	$model = App\User::query();

	return Datatables::eloquent($model)
					->setTransformer(new App\Transformers\UserTransformer)
					->setSerializer(new App\Serializers\CustomSerializer)
					->make(true);
});
```