---
title: Installation
description: Install and configure Laravel DataTables in your Laravel application
---

# Installation

This guide covers how to install Laravel DataTables packages using Composer.

---

## Requirements

| Requirement | Version | Notes |
|-------------|---------|-------|
| PHP | 8.3+ | Required |
| Laravel | 13.x | Latest stable |
| DataTables.js | 1.x or 2.x | [datatables.net](http://datatables.net/) |

---

## Installation Methods

### Option 1: Core Package Only

Install the core package when you need basic DataTables functionality:

```bash
composer require yajra/laravel-datatables-oracle:"^13.0"
```

### Option 2: All-in-One Package

Use the all-in-one installer when you need most DataTables plugins (Buttons, HTML Builder):

```bash
composer require yajra/laravel-datatables:"^13.0"
```

> [!TIP]
> The all-in-one package includes HTML Builder, Buttons, and other commonly used plugins.

---

## Quick Install All-in-One

For a complete setup with all plugins:

```bash
# Install the all-in-one package
composer require yajra/laravel-datatables:"^13.0"

# Install DataTables assets (using Vite)
npm i laravel-datatables-vite --save-dev
```

---

## Configuration

Publishing the configuration file is optional but recommended for customization:

```bash
php artisan vendor:publish --tag=datatables
```

This creates `config/datatables.php` with default settings:

```php
<?php

return [
    /*
     * Smart search adds wildcards automatically
     */
    'smart' => true,

    /*
     * Case insensitive searching
     */
    'case_insensitive' => true,

    /*
     * Wild card character for partial matching
     */
    'use_wildcards' => false,

    /*
     * DataTables internal index column name
     */
    'index_column' => 'DT_RowIndex',

    /*
     * Error message configuration
     */
    'error' => env('DATATABLES_ERROR', null),

    /*
     * NULLS LAST SQL pattern for PostgreSQL & Oracle
     */
    'nulls_last_sql' => '%s %s NULLS LAST',
];
```

---

## Package Structure

Laravel DataTables is split into multiple packages:

```
laravel-datatables/
├── Core Package (required)
│   └── yajra/laravel-datatables-oracle
│
├── HTML Builder (optional)
│   └── yajra/laravel-datatables-html
│
├── Buttons (optional)
│   └── yajra/laravel-datatables-buttons
│
├── Editor (optional - premium license required)
│   └── yajra/laravel-datatables-editor
│
├── Export (optional)
│   └── yajra/laravel-datatables-export
│
└── Fractal (optional)
    └── yajra/laravel-datatables-fractal
```

---

## Next Steps

<div class="grid md:grid-cols-2 gap-4">

**[Quick Starter →](/docs/{{package}}/{{version}}/quick-starter)**
Get up and running in 15 minutes with a complete example.

**[HTML Builder →](/docs/{{package}}/{{version}}/html-builder)**
Learn about the HTML Builder plugin for generating tables.

**[Buttons Plugin →](/docs/{{package}}/{{version}}/buttons-installation)**
Add export functionality (Excel, CSV, PDF).

</div>
