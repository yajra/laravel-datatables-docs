# DataTables Editor Plugin

<a name="editor-installation"></a>

> {tip} Special thanks to [@bellwood](https://github.com/bellwood) and [@DataTables](https://github.com/datatables) for being [generous](https://github.com/yajra/laravel-datatables/issues/1548) for providing a license to support the development of this package.

---

<a name="overview"></a>
## Overview

The DataTables Editor plugin integrates the powerful [DataTables Editor](https://editor.datatables.net/) library with Laravel DataTables. It provides a complete CRUD (Create, Read, Update, Delete) interface for your DataTables, handling all the backend logic including validation, database transactions, and event hooks.

This plugin is particularly useful when you need an **inline editing experience** directly within your DataTables, allowing users to edit table data without navigating to separate forms.

> {warning} A [premium license](https://editor.datatables.net/purchase/index) is required to use the DataTables Editor library. Please purchase a license before using this plugin.

---

<a name="prerequisites"></a>
## Prerequisites

Before installing this plugin, ensure you have:

| Requirement | Version | Notes |
|------------|---------|-------|
| Laravel | 13.x or later | Required |
| PHP | 8.3+ | Required for Laravel 13 |
| yajra/laravel-datatables | 13.x | Core package must be installed first |
| DataTables Editor License | Valid | [Purchase here](https://editor.datatables.net/purchase/index) |
| DataTables Editor Library | Downloaded | From your DataTables account |

---

<a name="installation"></a>
## Installation

### Step 1: Install the Package

```bash
composer require yajra/laravel-datatables-editor:"^13.0"
```

### Step 2: Install Editor Assets

Follow the [Editor Tutorial](/docs/{{package}}/{{version}}/editor-tutorial) for detailed instructions on setting up the frontend assets.

---

<a name="quick-start"></a>
## Quick Start

Once installed, you can quickly create an editor class using the artisan command:

```bash
php artisan datatables:editor Posts --model
```

Then, follow the [Editor Usage](/docs/{{package}}/{{version}}/editor-usage) documentation to set up your routes and frontend.

---

<a name="best-practices"></a>
## Best Practices

> {tip} **Pro Tips for Success**

- **Always use database transactions** - The Editor automatically wraps operations in transactions
- **Set fillable attributes** - Ensure your model has proper `$fillable` property
- **Use validation rules** - Always define validation rules for create, edit, and remove actions
- **Return the model** - In event hooks (v1.8.0+), always return the `$model` instance
- **DT_RowId for remove** - Always validate `DT_RowId` in `removeRules()` as `required|exists:table,id`

---

<a name="related-documentation"></a>
## Related Documentation

| Documentation | Description |
|--------------|-------------|
| [Editor Usage](/docs/{{package}}/{{version}}/editor-usage) | Complete usage guide |
| [Editor Commands](/docs/{{package}}/{{version}}/editor-command) | Artisan command reference |
| [Editor Events](/docs/{{package}}/{{version}}/editor-events) | Event hooks documentation |
| [Editor Model](/docs/{{package}}/{{version}}/editor-model) | Model configuration |
| [Editor Rules](/docs/{{package}}/{{version}}/editor-rules) | Validation rules reference |
| [Editor Tutorial](/docs/{{package}}/{{version}}/editor-tutorial) | Step-by-step tutorial |
