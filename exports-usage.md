# Export Usage

This guide explains how to integrate the export functionality into your DataTable class and views.

<a name="prerequisites"></a>
## Prerequisites

Before using exports, ensure you have:

1. Installed the export package (see [Installation](exports-installation.md))
2. Configured your queue driver in `config/queue.php`
3. Running queue worker: `php artisan queue:work`

<a name="basic-setup"></a>
## Basic Setup

<a name="step-1-add-the-export-button-component"></a>
### Step 1: Add the Export Button Component

Add the Livewire export button to your view file where your DataTable is rendered:

```blade
<livewire:export-button :table-id="$dataTable->getTableId()" />
```

<a name="step-2-use-the-trait-in-your-datatable-class"></a>
### Step 2: Use the Trait in Your DataTable Class

Add the `WithExportQueue` trait to your DataTable class:

```php
<?php

namespace App\DataTables;

use App\Models\User;
use Yajra\DataTables\Services\DataTable;
use Yajra\DataTables\WithExportQueue;

class UsersDataTable extends DataTable
{
    use WithExportQueue;

    // ... rest of your DataTable methods
}
```

<a name="step-3-start-the-queue-worker"></a>
### Step 3: Start the Queue Worker

Run your queue worker to process export jobs in the background:

```bash
php artisan queue:work
```

For production, consider using Supervisor to keep the queue worker running:

```ini
[program:laravel-worker]
process_name=%(program_name)s_%(process_num)02d
command=php /path/to/artisan queue:work redis --sleep=3 --tries=3 --max-time=3600
autostart=true
autorestart=true
stopper_as_group=true
numprocs=2
redirect_stderr=true
stdout_logfile=/path/to/worker.log
```

<a name="customization-options"></a>
## Customization Options

<a name="export-file-type"></a>
### Export File Type

Specify the export format (default is `xlsx`):

```blade
{{-- Excel export --}}
<livewire:export-button :table-id="$dataTable->getTableId()" type="xlsx" />

{{-- CSV export --}}
<livewire:export-button :table-id="$dataTable->getTableId()" type="csv" />
```

<a name="custom-button-styling"></a>
### Custom Button Styling

Apply custom CSS classes to the export button:

```blade
<livewire:export-button 
    :table-id="$dataTable->getTableId()" 
    class="btn btn-success btn-lg" 
/>
```

<a name="custom-button-text"></a>
### Custom Button Text

Change the button label:

```blade
<livewire:export-button 
    :table-id="$dataTable->getTableId()" 
    button-name="Download Report" 
/>
```

<a name="custom-filename"></a>
### Custom Filename

Set a custom download filename:

```blade
<livewire:export-button 
    :table-id="$dataTable->getTableId()" 
    filename="users-report.xlsx" 
/>
```

<a name="auto-download"></a>
### Auto Download

Automatically trigger download when export completes:

```blade
<livewire:export-button 
    :table-id="$dataTable->getTableId()" 
    :auto-download="true" 
/>
```

<a name="email-export"></a>
### Email Export

Send the export file via email:

```blade
<livewire:export-button 
    :table-id="$dataTable->getTableId()" 
    email-to="admin@example.com" 
/>
```

<a name="custom-sheet-name"></a>
### Custom Sheet Name

Override the sheet name in your DataTable class:

```php
<?php

namespace App\DataTables;

use App\Models\User;
use Yajra\DataTables\Services\DataTable;
use Yajra\DataTables\WithExportQueue;

class UsersDataTable extends DataTable
{
    use WithExportQueue;

    /**
     * Customize the Excel sheet name.
     * Excel sheet names have a 31 character limit.
     */
    protected function sheetName(): string
    {
        return 'Users Report';
    }

    // ... rest of your DataTable methods
}
```

<a name="how-it-works"></a>
## How It Works

1. User clicks the export button
2. A batch job is created with the current DataTable query parameters
3. The job is dispatched to the queue
4. The Livewire component polls for job completion
5. Once finished, the export file is available for download
6. The file is stored on the configured disk (local, S3, etc.)

<a name="export-process-flow"></a>
## Export Process Flow

```
┌─────────────────┐      ┌─────────────────┐      ┌─────────────────┐
│   User clicks   │ ──▶  │  Batch job is   │ ──▶  │   Queue worker  │
│  Export button  │      │   dispatched    │      │  processes job  │
└─────────────────┘      └─────────────────┘      └─────────────────┘
                                                         │
                                                         ▼
┌─────────────────┐      ┌─────────────────┐      ┌─────────────────┐
│  Download file  │ ◀── │  File saved to  │ ◀──  │  Data fetched   │
│  from disk/S3   │      │    disk/S3      │      │  and processed  │
└─────────────────┘      └─────────────────┘      └─────────────────┘
```

<a name="troubleshooting"></a>
## Troubleshooting

<a name="export-button-not-appearing"></a>
### Export button not appearing

Ensure Livewire 3.x is installed and the `@livewireScripts` directive is included in your layout:

```blade
@livewireStyles
</head>
<body>
    {{ $slot }}
    
    @livewireScripts
</body>
</html>
```

<a name="export-job-not-processing"></a>
### Export job not processing

1. Check your queue worker is running: `php artisan queue:work`
2. Verify the queue connection in `config/queue.php`
3. Check the Laravel logs for job failures

<a name="large-exports-timing-out"></a>
### Large exports timing out

1. Ensure you're using the `lazy` method (default)
2. Increase the chunk size in config: `'chunk' => 2000`
3. Consider using Redis as your queue driver for better performance

<a name="related-documentation"></a>
## Related Documentation

- [Export Options](exports-options.md) - Column formatting and advanced options
- [Export Columns](export-column.md) - Configure export-specific columns
- [Export Purge](exports-purge.md) - Automate cleanup of old export files
