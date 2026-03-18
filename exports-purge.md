---
title: Export Purge
description: Purge old export files
---


# Export Purge

This guide explains how to automatically clean up old export files from your storage disk.

<a name="overview"></a>
## Overview

Export files (XLSX, CSV) can accumulate over time and consume storage space. The export package includes a purge command that removes files older than the configured number of days.

<a name="configuration"></a>
## Configuration

Configure the purge behavior in `config/datatables-export.php`:

```php
'purge' => [
    // Remove export files older than this many days
    'days' => 1,
],
```

<a name="manual-purge"></a>
## Manual Purge

Run the purge command manually whenever needed:

```bash
php artisan datatables:purge-export
```

<a name="options"></a>
### Options

- `--days`: Override the configured days value for this run

```bash
# Purge files older than 7 days
php artisan datatables:purge-export --days=7
```

<a name="automated-purge-recommended"></a>
## Automated Purge (Recommended)

Schedule the purge command to run automatically in `routes/console.php`:

```php
use Illuminate\Console\Scheduling\Schedule;

$schedule->command('datatables:purge-export')->daily();
```

<a name="how-it-works"></a>
## How It Works

The purge command:

1. Reads the configured disk from `config('datatables-export.disk')`
2. Scans all files on the disk
3. Identifies files with `.xlsx` or `.csv` extensions
4. Deletes files modified before the configured cutoff date
5. Outputs progress information

<a name="important-notes"></a>
## Important Notes

- The purge only affects export files (`.xlsx`, `.csv`)
- Files from other applications on the same disk are not affected
- The purge respects the configured storage disk, not the S3 disk
- Running the command multiple times is safe (idempotent)

<a name="related-documentation"></a>
## Related Documentation

- [Installation](exports-installation.md) - Initial setup
- [Export Usage](exports-usage.md) - How exports work
