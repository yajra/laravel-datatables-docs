---
title: Release Notes
description: Changelog and release information for Laravel DataTables
---

# Release Notes

This document contains historical release notes. For the latest changes, please visit the [GitHub Releases](https://github.com/yajra/laravel-datatables-oracle/releases) page.

---

## v13.x

Laravel DataTables v13.x is the latest major version with Laravel 13 support.

### Breaking Changes

- Requires PHP 8.3+
- Requires Laravel 13.x
- Namespace updated from `Yajra\Datatables` to `Yajra\DataTables`

---

## v7.0

Laravel DataTables 7.0 splits Laravel DataTables 6.x into a main package and plugins packages for more flexible and pluggable design.

### Buttons Plugin

On Laravel DataTables 7.0, service classes and files are extracted into a separate package to reduce its complexity and dependencies on other packages by default.

This idea comes from [Issue #832](https://github.com/yajra/{{package}}/issues/832) which makes sense since not all users are using the export functionality.

### DomPDF

`DomPDF` dependency is now optional on Laravel DataTables 7.0 and was transferred to Buttons plugin. The `Buttons` plugin will now give you a choice to install it or not. This was as a `suggest` since we now have an option to use [`snappy`](https://github.com/barryvdh/laravel-snappy) as our PDF generator.

### Other Changes

#### Request Property

`DataTables` `request` property is now set as `protected`. To access the request instance, use the getter method `getRequest()`:

```php
$dataTable = DataTables::of(User::query());
$request = $dataTable->getRequest();
```

---

## See Also

- [Upgrade Guide](/docs/{{package}}/{{version}}/upgrade) - Migration instructions
- [Installation](/docs/{{package}}/{{version}}/installation) - Getting started
