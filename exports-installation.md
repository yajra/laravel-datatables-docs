---
title: Export Plugin Installation
description: Install and configure the Export plugin for queue-based DataTables exports
---


# Export Plugin Installation

> **GitHub**: [yajra/laravel-datatables-export](https://github.com/yajra/laravel-datatables-export)

This package is a plugin for Laravel DataTables that handles server-side exporting using Queue, OpenSpout, and Livewire. It allows you to export large datasets without timing out by processing exports in background jobs.

> [!NOTE]
> **Requirements:**
> - PHP 8.3+
> - Laravel 13.x
> - Livewire 3.x
> - A queue worker (Redis, Database, SQS, etc.)

---

<a name="installation"></a>
## Quick Installation

Install the package via Composer:

```bash
composer require yajra/laravel-datatables-export:"^13.0"
```

---

<a name="queue-setup"></a>
<a name="queue"></a>
## Queue Setup

The export package uses Laravel's job batching feature. You need to set up the queue and batch jobs tables:

```bash
# Create the jobs table (if not already exists)
php artisan queue:table

# Create the job batches table (required for export tracking)
php artisan queue:batches-table

# Run migrations
php artisan migrate
```

---

<a name="configuration"></a>
## Configuration

> [!NOTE]
> This step is optional. The package works with default configuration out of the box.

Publish the configuration file and views to customize the export behavior:

```bash
php artisan vendor:publish --tag=datatables-export --force
```

### Configuration Options

After publishing, you can customize the export behavior in `config/datatables-export.php`:

```php
<?php

return [
    /*
     * Query iteration method: 'lazy' (recommended for large datasets) or 'cursor'
     */
    'method' => 'lazy',

    /*
     * Chunk size when using lazy method
     */
    'chunk' => 1000,

    /*
     * Storage disk for export files
     */
    'disk' => 'local',

    /*
     * Optional S3 disk for final destination
     */
    's3_disk' => '',

    /*
     * Default date format for exports
     */
    'default_date_format' => 'yyyy-mm-dd',

    /*
     * Auto-purge exports older than X days
     */
    'purge' => [
        'days' => 1,
    ],
];
```

---

## Next Steps

- [Export Usage](exports-usage) - Learn how to use the export functionality
- [Export Options](exports-options) - Customize export types, sheet names, and column formatting
- [Export Columns](export-column) - Configure which columns to include in exports
- [Export Purge](exports-purge) - Automate cleanup of old export files
