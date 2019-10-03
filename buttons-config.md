# Buttons Configurations

<a name="console"></a>
## Artisan Console Configurations
Namespace configuration is used by the datatables command generator.

```php
'namespace' => [
    'base'  => 'DataTables',
    'model' => '',
],
```

### DataTable Base Namespace/Directory
This is the base namespace/directory to be created when a new DataTable was called.
This directory is appended on default Laravel namespace.

**Usage:**
```php artisan datatables:make User```

**Output:**
```App\DataTables\UserDataTable```

**Export filename:** ```users_(timestamp)```

### Model Option
This is the base namespace/directory where your model's are located.
This directory is appended on default Laravel namespace.
**Usage:** ```php artisan datatables:make Post --model```
**Output:** ```App\DataTables\PostDataTable```
**With Model:** ```App\Post``
**Export filename:** ```posts_(timestamp)```

<a name="pdf-generator"></a>
## PDF Generator
Set the PDF generator to be used when converting your dataTable to pdf.

Available generators are: `excel`, `snappy`

### Excel Generator
When `excel` is used as the generator, the package will use [`maatwebsite/excel`](http://www.maatwebsite.nl/laravel-excel/docs) to generate the PDF.

> To export files to pdf, you will have to include "dompdf/dompdf": "~0.6.1", "mpdf/mpdf": "~5.7.3" or "tecnick.com/tcpdf": "~6.0.0" in your composer.json and change the export.pdf.driver config setting accordingly.

### Snappy Generator (Default Generator)
When `snappy` is used as the generator, you need to install [`barryvdh/laravel-snappy`](https://github.com/barryvdh/laravel-snappy)

### Snappy PDF Options
These are the options passed to `laravel-snappy` when exporting the pdf file.

```php
'snappy'          => [
    'options'     => [
        'no-outline'    => true,
        'margin-left'   => '0',
        'margin-right'  => '0',
        'margin-top'    => '10mm',
        'margin-bottom' => '10mm',
    ],
    'orientation' => 'landscape',
],
```
