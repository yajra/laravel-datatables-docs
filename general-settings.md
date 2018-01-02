# General Settings

## Introduction
You can change the package behavior by updating the configuration file.

The configuration file can be found at `config/datatables.php`.

<a name="smart-search"></a>
## Smart Search
Smart search will enclose search keyword with wildcard string `"%keyword%"`.
The sql generated will be like `column LIKE "%keyword%"` when set to `true`.

```php
'smart' => true,
```

<a name="case-sensitivity"></a>
## Case Sensitive Search
Case insensitive will search the keyword in lower case format.
The sql generated will be like `LOWER(column) LIKE LOWER(keyword)` when set to `true`.

```php
'case_insensitive' => true,
```

<a name="wild-card"></a>
## Wild Card Search
Wild card will add `%` in between every characters of the keyword.
The sql generated will be like `column LIKE "%k%e%y%w%o%r%d%"` when set to `true`.

```php
'use_wildcards' => false,
```

<a name="index-column"></a>
## Index Column
DataTables internal index id response column name.

```php
'index_column' => 'DT_Row_Index',
```

<a name="fractal-includes"></a>
## Fractal Includes
Request key name to parse includes on fractal.

```php
'includes' => 'include',
```

<a name="fractal-serializer"></a>
## Fractal Serializer
Default fractal serializer to be used when serializing the response.

```php
'serializer' => League\Fractal\Serializer\DataArraySerializer::class,
```
<a name="script-template"></a>
## Script Template
DataTables html builder script blade template.
If published, the file will installed on `resources/views/vendor/datatables/script.blade.php`.

```php
'script_template' => 'datatables::script',
```

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

<a name="generator"></a>
## PDF Generator
Set the PDF generator to be used when converting your dataTable to pdf.

Available generators are: `excel`, `snappy`

### Excel Generator (Default Generator)
When `excel` is used as the generator, the package will use [`maatwebsite/excel`](http://www.maatwebsite.nl/laravel-excel/docs) to generate the PDF.

> To export files to pdf, you will have to include "dompdf/dompdf": "~0.6.1", "mpdf/mpdf": "~5.7.3" or "tecnick.com/tcpdf": "~6.0.0" in your composer.json and change the export.pdf.driver config setting accordingly.

### Snappy Generator
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

