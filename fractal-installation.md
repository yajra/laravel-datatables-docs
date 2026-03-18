---
title: Fractal Plugin Installation
description: Install and configure the Fractal plugin for API responses
---


# Fractal Plugin Installation

---

<a name="installation"></a>
## Installation

Run the following command in your project to get the latest version of the plugin:

```bash
composer require yajra/laravel-datatables-fractal:"^13.0"
```

---

<a name="configuration"></a>
## Configuration

> [!NOTE]
> This step is optional. The package works with default configuration out of the box.

Publish the configuration file:

```bash
php artisan vendor:publish --tag=datatables-fractal
```

---

## See Also

- [Response using Transformer](/docs/{{package}}/{{version}}/response-fractal) - Learn how to transform DataTables responses using Fractal transformers
- [Fractal Transformer Serializer](/docs/{{package}}/{{version}}/response-fractal-serializer) - Configure custom serializers for Fractal transformers
