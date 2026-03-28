---
title: Buttons Configurations
description: Configure the Buttons plugin for DataTables export functionality
---


# Buttons Configurations

---

<a name="console"></a>
## Artisan Console Configurations

Namespace configuration is used by the datatables command generator:

```php
<?php

return [
    'namespace' => [
        'base'  => 'DataTables',
        'model' => '',
    ],
];
```

### DataTable Base Namespace/Directory

This is the base namespace/directory to be created when a new DataTable is called. This directory is appended to the default Laravel namespace.

**Usage:**

```bash
php artisan datatables:make User
```

**Output:** `App\DataTables\UserDataTable`

**Export filename:** `users_(timestamp)`

### Model Option

This is the base namespace/directory where your models are located:

**Usage:**

```bash
php artisan datatables:make Post --model
```

**Output:** `App\DataTables\PostDataTable`

**With Model:** `App\Post`

**Export filename:** `posts_(timestamp)`

---

<a name="pdf-generator"></a>
## PDF Generator

Set the PDF generator to be used when converting your DataTable to PDF:

```php
'pdf_generator' => 'excel', // or 'snappy'
```

Available generators are: `excel`, `snappy`

### Excel Generator

When `excel` is used as the generator, the package will use [`maatwebsite/excel`](http://www.maatwebsite.nl/laravel-excel/docs) to generate the PDF.

> [!NOTE]
> To export files to PDF, you will have to include `dompdf/dompdf`, `mpdf/mpdf`, or `tecnick.com/tcpdf` in your `composer.json` and change the `export.pdf.driver` config setting accordingly.

### Snappy Generator

When `snappy` is used as the generator, you need to install [`barryvdh/laravel-snappy`](https://github.com/barryvdh/laravel-snappy):

```bash
composer require barryvdh/laravel-snappy
```

### Snappy PDF Options

These are the options passed to `laravel-snappy` when exporting the PDF file:

```php
<?php

return [
    'snappy' => [
        'options' => [
            'no-outline'    => true,
            'margin-left'   => '0',
            'margin-right'  => '0',
            'margin-top'    => '10mm',
            'margin-bottom' => '10mm',
        ],
        'orientation' => 'landscape',
    ],

    /*
     * Default html builder parameters.
     */
    'parameters' => [
        'dom' => 'Bfrtip',
        'order' => [[0, 'desc']],
        'buttons' => [
            'excel',
            'csv',
            'pdf',
            'print',
            'reset',
            'reload',
        ],
    ],

    /*
     * Generator command default options.
     */
    'generator' => [
        'columns' => 'id,add your columns,created_at,updated_at',
        'buttons' => 'excel,csv,pdf,print,reset,reload',
        'dom' => 'Bfrtip',
    ],
];
```

---

<a name="parameters-config"></a>
## Default Builder Parameters

The `parameters` config sets default DataTables HTML builder options:

```php
'parameters' => [
    'dom' => 'Bfrtip',
    'order' => [[0, 'desc']],
    'buttons' => [
        'excel',
        'csv',
        'pdf',
        'print',
        'reset',
        'reload',
    ],
],
```

- `dom`: DataTables layout structure
- `order`: Default column ordering
- `buttons`: Default buttons to display

<a name="generator-config"></a>
## Generator Options

The `generator` config sets default options for the `datatables:make` command:

```php
'generator' => [
    'columns' => 'id,add your columns,created_at,updated_at',
    'buttons' => 'excel,csv,pdf,print,reset,reload',
    'dom' => 'Bfrtip',
],
```

---

## See Also

- [Buttons Installation](/docs/{{package}}/{{version}}/buttons-installation) - Setup the buttons package
- [Buttons Export](/docs/{{package}}/{{version}}/buttons-export) - Export button options
- [Buttons Custom](/docs/{{package}}/{{version}}/buttons-custom) - Custom button actions
