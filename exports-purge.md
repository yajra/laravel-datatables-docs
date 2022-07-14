# Export Usage

## Purging exported files

On `app\Console\Kernel.php`, register the purge command

```php
$schedule->command('datatables:purge-export')->weekly();
```
