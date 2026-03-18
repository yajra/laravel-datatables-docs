---
title: Contributing Guide
description: Contribute to Laravel DataTables
---


# Contributing

Contributions are **welcome** and will be fully **credited**.

We accept contributions via Pull Requests on [GitHub](https://github.com/yajra/laravel-datatables).

---

## Pull Requests

- **[PSR-2 Coding Standard](https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-2-coding-style-guide.md)** - The easiest way to apply the conventions is to install [PHP Code Sniffer](http://pear.php.net/package/PHP_CodeSniffer):

  ```bash
  composer require --dev squizlabs/php_codesniffer
  ```

- **Document any change in behaviour** - Make sure the `README.md` and any other relevant documentation are kept up-to-date.

- **Consider our release cycle** - We try to follow [SemVer v2.0.0](http://semver.org/). Randomly breaking public APIs is not an option.

- **Send coherent history** - Make sure each individual commit in your pull request is meaningful. If you had to make multiple intermediate commits while developing, please squash them before submitting.

---

## Coding Standards

### Running PHP Code Sniffer

```bash
# Check coding standards
./vendor/bin/phpcs

# Auto-fix coding standards
./vendor/bin/phpcbf
```

### Running Tests

```bash
composer test
```

---

## Security

If you discover any security related issues, please email [aqangeles@gmail.com](mailto:aqangeles@gmail.com) instead of using the issue tracker.

---

## License

Laravel DataTables is open-sourced software licensed under the [MIT license](LICENSE).
