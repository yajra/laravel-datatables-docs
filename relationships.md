# Eager Loading Relationships

`DataTables` support searching and sorting of eager loaded relationships when using `Eloquent`.
In this example, I will show you how to setup a eager loading search using `EloquentEngine`.

To enable search, we need to eager load the relationship we intend to use using Laravel's `User::with('posts')` api.

```php
use DataTables;

Route::get('user-data', function() {
	$model = App\User::with('posts');

	return DataTables::eloquent($model)
				->addColumn('posts', function (User $user) {
                    return $user->posts->map(function($post) {
                        return str_limit($post->title, 30, '...');
                    })->implode('<br>');
                })
				->toJson();
});
```

To trigger search on `posts` relationship, we need to specify the `relation.column_name` as the `name` attribute in our `javascript` appropriately.

```php
<script>
	$(document).ready(function() {
		$('#users-table').DataTable({
	        processing: true,
	        serverSide: true,
	        ajax: '{{ url("collection/basic-object-data") }}',
	        columns: [
	            {data: 'id', name: 'id'},
	            {data: 'name', name: 'name'},
	            {data: 'email', name: 'email'},
	            {data: 'posts', name: 'posts.title'},
	            {data: 'created_at', name: 'created_at'},
	            {data: 'updated_at', name: 'updated_at'}
	        ]
		});
	});
</script>
```

Looking at `{data: 'posts', name: 'posts.title'},`:
- `data: posts` represents the data key (`data.posts`) that we are going to display on our table.
- `name: posts.title` represents the `User` model relationship (`posts`) and the column we are going to perform our search (`title`).

It is advised that you include select('table.') on query to avoid weird issues where id from related model replaces the id of the main model.
```php
$posts = Post::with('user')->select('posts.*');
```
## Nested Relationships
Same strategy goes for nested relationships but do **NOTE** that ordering is not yet fully tested on nested relationships.

