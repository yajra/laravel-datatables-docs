# Regex Searching

Datatables has the ability to perform search using regular expressions.

> Regex search only works and tested on the following Laravel DB drivers: MySQL, SQLite and Oracle.

To enable regex, you need to explicitly set it on your javascript:

```
$('#example').dataTable({
	processing: true,
    serverSide: true,
    ajax: '',
    columns: [
        {data: 'id', name: 'id'},
        {data: 'name', name: 'name'},
        {data: 'email', name: 'email'},
        {data: 'created_at', name: 'created_at'},
        {data: 'updated_at', name: 'updated_at'}
    ],
  	search: {
    	"regex": true
  	}
});
```