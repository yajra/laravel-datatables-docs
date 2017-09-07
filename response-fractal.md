# Response using Transformer

When using tranformer, all response manipulations must be done via transformer.
Thus `addColumn`, `editColumn`, `removeColumn`, `setRowAttr`, `setClassAttr`, etc... should be avoided when using fractal.

```php
use DataTables;
use App\Transformers\UserTransformer;

Route::get('user-data', function() {
	$model = App\User::query();

	return DataTables::eloquent($model)
				->setTransformer(new UserTransformer)
				->toJson();
});
```

Create your transformer. `app\Transformers\UserTransformer.php`

```php
namespace App\Transformers;

use App\User;
use League\Fractal\TransformerAbstract;

class UserTransformer extends TransformerAbstract
{
    protected $availableIncludes = ['posts'];

    /**
     * @param \App\User $user
     * @return array
     */
    public function transform(User $user)
    {
        return [
            'id'         => (int) $user->id,
            'name'       => $user->name,
            'email'      => $user->email,
            'created_at' => (string) $user->created_at,
            'updated_at' => (string) $user->updated_at,
        ];
    }

    /**
     * @param \App\User $user
     * @return \League\Fractal\Resource\Collection
     */
    public function includePosts(User $user)
    {
        $posts =  $user->posts;

        return $this->collection($posts, new PostTransformer);
    }
}
```
