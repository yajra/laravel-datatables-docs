---
title: Contributing Guide
description: Contribute to Laravel DataTables
---

# Contributing

Contributions are **welcome** and will be fully **credited**.

We accept contributions via Pull Requests on [GitHub](https://github.com/yajra/laravel-datatables).

---

<a name="pull-requests"></a>
## Pull Requests

- **Run static analysis and tests** - Ensure all checks pass before submitting (see Coding Standards below)
- **Document any change in behaviour** - Make sure the `README.md` and any other relevant documentation are kept up-to-date
- **Consider our release cycle** - We try to follow [SemVer v2.0.0](http://semver.org/). Randomly breaking public APIs is not an option
- **Send coherent history** - Make sure each individual commit in your pull request is meaningful. If you had to make multiple intermediate commits while developing, please squash them before submitting

---

<a name="coding-standards"></a>
## Coding Standards

This project uses Laravel Pint for code style, PHPStan for static analysis, and Rector for automated code modernization.

### Laravel Pint (Code Style)

```bash
# Fix code style issues
./vendor/bin/pint

# Run without modifying files (preview only)
./vendor/bin/pint --test
```

### PHPStan (Static Analysis)

```bash
# Run static analysis
./vendor/bin/phpstan analyse

# Run with specific memory limit
./vendor/bin/phpstan analyse --memory-limit=512M
```

### Rector (Automated Modernization)

```bash
# Preview code changes
./vendor/bin/rector process --dry-run

# Apply code changes
./vendor/bin/rector process
```

### Running All Checks

```bash
# Run tests with coverage
composer test

# Or run all checks manually
./vendor/bin/pint && ./vendor/bin/phpstan analyse && composer test
```

---

<a name="security"></a>
## Security

If you discover any security related issues, please email [aqangeles@gmail.com](mailto:aqangeles@gmail.com) instead of using the issue tracker.

---

<a name="license"></a>
## License

Laravel DataTables is open-sourced software licensed under the [MIT license](LICENSE).
