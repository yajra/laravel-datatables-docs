---
title: Introduction
description: Learn about Laravel DataTables and its integration with Laravel and jQuery DataTables
---

# Introduction

Laravel DataTables is a powerful package that bridges server-side processing capabilities of [DataTables](https://datatables.net) with Laravel's elegant syntax.

---

<a name="overview"></a>
## Quick Overview

| Component | Description |
|-----------|-------------|
| [Laravel](https://laravel.com) | The PHP Framework For Web Artisans |
| [DataTables](https://datatables.net) | jQuery plugin for interactive HTML tables |
| **Laravel DataTables** | Server-side processing bridge for Laravel |

---

<a name="laravel"></a>
## What is Laravel?

Laravel is a web application framework with expressive, elegant syntax. It attempts to take the pain out of development by easing common tasks used in the majority of web projects, such as:

- **Authentication** - Session-based and token-based auth out of the box
- **Routing** - Expressive, fluent API for defining application routes
- **Database ORM** - Eloquent provides a beautiful ActiveRecord implementation
- **Template Engine** - Blade templates for building dynamic views
- **Testing** - Built-in testing support with PHPUnit

> [!NOTE]
> **Official Documentation**: Complete Laravel documentation is available at [laravel.com/docs](https://laravel.com/docs)

---

<a name="datatables"></a>
## What is DataTables?

DataTables is a plug-in for the [jQuery](https://jquery.com/) JavaScript library. It is a highly flexible tool, based upon the foundations of progressive enhancement, that adds advanced interaction controls to any HTML table.

### Key Features

- **Pagination** - Navigate through large datasets
- **Searching** - Global and column-specific filtering
- **Sorting** - Click-to-sort columns with multi-column support
- **AJAX** - Server-side processing for large datasets
- **Extensions** - Buttons, SearchPanes, Editor, and more

> [!NOTE]
> **Official Documentation**: Full DataTables documentation is available at [datatables.net/manual](https://datatables.net/manual)

---

<a name="package"></a>
## What is Laravel DataTables?

[![License](https://poser.pugx.org/yajra/laravel-datatables-oracle/license)](https://packagist.org/packages/yajra/laravel-datatables-oracle)
[![Latest Stable Version](https://poser.pugx.org/yajra/laravel-datatables-oracle/v/stable)](https://packagist.org/packages/yajra/laravel-datatables-oracle)
[![Total Downloads](https://poser.pugx.org/yajra/laravel-datatables-oracle/downloads)](https://packagist.org/packages/yajra/laravel-datatables-oracle)

Laravel DataTables is a package that handles the [server-side](https://www.datatables.net/manual/server-side) works of [DataTables](http://datatables.net) using [Laravel](http://laravel.com).

### Package Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    Laravel DataTables                       │
├─────────────────────────────────────────────────────────────┤
│  Core Package: yajra/laravel-datatables-oracle             │
│  ├── Eloquent Engine                                        │
│  ├── Query Builder Engine                                   │
│  ├── Collection Engine                                     │
├─────────────────────────────────────────────────────────────┤
│  Plugins (Optional)                                        │
│  ├── HTML Builder: yajra/laravel-datatables-html          │
│  ├── Buttons: yajra/laravel-datatables-buttons            │
│  ├── Editor: yajra/laravel-datatables-editor              │
│  ├── Export: yajra/laravel-datatables-export              │
│  └── Fractal: yajra/laravel-datatables-fractal           │
└─────────────────────────────────────────────────────────────┘
```

---

## Package Comparison

| Package | Purpose | When to Use |
|---------|---------|-------------|
| `laravel-datatables-oracle` | Core functionality | Always required |
| `laravel-datatables-html` | HTML table builder | For generating tables with Blade |
| `laravel-datatables-buttons` | Export buttons | For Excel, CSV, PDF exports |
| `laravel-datatables-editor` | Inline editing | For CRUD operations in DataTables |
| `laravel-datatables-export` | Queue exports | For large dataset exports |
| `laravel-datatables-fractal` | API responses | For transforming API data |
| `laravel-datatables` | All-in-one | Quick start with all features |

---

<a name="see-also"></a>
## See Also

- [Installation](/docs/{{package}}/{{version}}/installation) - Get started with Laravel DataTables
- [Quick Starter](/docs/{{package}}/{{version}}/quick-starter) - Build your first DataTable in 15 minutes
- [HTML Builder](/docs/{{package}}/{{version}}/html-builder) - Generate DataTables HTML markup
