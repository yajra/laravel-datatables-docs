# Release Notes

- [Laravel DataTables 10.1](#10.1)

<a name="10.1"></a>
## Laravel DataTables 10.1

Laravel DataTables 10.1 splits Laravel DataTables 6.x into a main package and plugins packages for more flexibile and pluggable design.

### Buttons Plugin
On Laravel DataTables 10.1, service classes and files are extracted into a separate package to reduce its complexity and dependencies on other packages by default.
This idea comes up from [Issue #832](https://github.com/yajra/{{package}}/issues/832) which actually makes sense since not all users are using the export functionality.

### DomPDF
`DomPDF` dependency is now optional on Laravel DataTables 10.1 and was transferred to Buttons plugin.
And the `Buttons` plugin will now give you a choice to install it or not.
This was as a `suggest` since we now have an option to use [`snappy`](https://github.com/barryvdh/laravel-snappy) as our pdf generator.

### Other Changes

#### Request property
`DataTables` `request` property is now set as `protected`. To access the request instance, use the getter method `getRequest()`.

```php
$dataTable = Datatable::of(User::query());
$request = $dataTable->getRequest();
```
