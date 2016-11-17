# Release Notes

- [Laravel Datatables 7.0](#7.0)

<a name="7.0"></a>
## Laravel Datatables 7.0

Laravel Datatable 7.0 splits Laravel Datatable 6.x into a main package and plugins packages for more flexibile and pluggable design.

### Buttons Plugin
On Laravel Datatables 7.0, service classes and files are extracted into a separate package to reduce its complexity and dependencies on other packages by default.
This idea comes up from [Issue #832](https://github.com/yajra/laravel-datatables/issues/832) which actually makes sense since not all users are using the export functionality.

### DomPDF
`DomPDF` dependency is now optional on Laravel Datatables 7.0 and was transferred to Buttons plugin.
And the `Buttons` plugin will now give you a choice to install or not.
This was also set to optional since we now have an option to use [`laravel-snappy`](https://github.com/barryvdh/laravel-snappy) as our pdf generator.
