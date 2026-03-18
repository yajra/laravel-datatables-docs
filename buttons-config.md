# Buttons Configurations

<a name="artisan-console"></a>
## Artisan Console Configurations
Namespace configuration is used by the datatables command generator.

```php
'namespace' => [
    'base'  => 'DataTables',
    'model' => '',
],
```

<a name="base-namespace"></a>
### DataTable Base Namespace/Directory
This is the base namespace/directory to be created when a new DataTable is called.
This directory is appended to the default Laravel namespace.

**Usage:**
```php artisan datatables:make User```

**Output:**
```App\DataTables\UserDataTable```

**Export filename:** ```users_(timestamp)```

<a name="model-option-config"></a>
### Model Option
This is the base namespace/directory where your models are located.
This directory is appended to the default Laravel namespace.
**Usage:** ```php artisan datatables:make Post --model```
**Output:** `App\DataTables\PostDataTable`
**With Model:** `App\Post`
**Export filename:** `posts_(timestamp)`

<a name="pdf-generator"></a>
## PDF Generator
Set the PDF generator to be used when converting your dataTable to PDF.

Available generators are: `excel`, `snappy`

<a name="excel-generator"></a>
### Excel Generator
When `excel` is used as the generator, the package will use [`maatwebsite/excel`](http://www.maatwebsite.nl/laravel-excel/docs) to generate the PDF.

> To export files to pdf, you will have to include "dompdf/dompdf": "~0.6.1", "mpdf/mpdf": "~5.7.3" or "tecnick.com/tcpdf": "~6.0.0" in your composer.json and change the export.pdf.driver config setting accordingly.

<a name="snappy-generator"></a>
### Snappy Generator
When `snappy` is used as the generator, you need to install [`barryvdh/laravel-snappy`](https://github.com/barryvdh/laravel-snappy)

<a name="snappy-pdf-options"></a>
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

<a name="see-also"></a>
## See Also

- [Buttons Installation](buttons-installation.md) - Setup the buttons package
- [Buttons Export](buttons-export.md) - Export button options
- [Buttons Custom](buttons-custom.md) - Custom button actions
