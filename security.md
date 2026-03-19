---
title: Security
description: Security best practices for Laravel DataTables
---

# Security

This guide covers essential security practices when working with Laravel DataTables. Understanding these patterns helps protect your application from common vulnerabilities.

> [!NOTE]
> Laravel DataTables provides secure defaults. Most security features work automatically, but you need to be intentional when customizing data output.

---

## Overview

DataTables security involves several key areas:

| Area | Risk | Default Protection |
|------|------|---------------------|
| **XSS** | Malicious scripts in user data | Automatic escaping |
| **CSRF** | Cross-site request forgery | Token handling via `scripts()` |
| **Mass Assignment** | Unintended data modification | `$fillable`/`$guarded` attributes |
| **Authorization** | Unauthorized data access | Manual policy checks |

---

<a name="xss"></a>
## XSS Protection

### How It Works

Laravel DataTables escapes all column data by default, converting special characters to HTML entities. This prevents malicious scripts from executing in the browser.

```php
// Input: <script>alert('hacked')</script>
// Output (escaped): &lt;script&gt;alert(&#039;hacked&#039;)&lt;/script&gt;
```

### Allowing HTML in Specific Columns

When you need to render HTML (buttons, badges, links), use `rawColumns` to mark trusted columns:

```php
use Yajra\DataTables\Facades\DataTables;
use App\Models\User;

Route::get('user-data', function() {
    return DataTables::eloquent(User::query())
        ->addColumn('action', function (User $user) {
            // Generate HTML for action buttons
            return view('users.datatable-actions', ['user' => $user])->render();
        })
        ->rawColumns(['action']) // Mark as safe - contains no user input
        ->toJson();
});
```

> [!WARNING]
> **Never pass unsanitized user input to rawColumns.** The `rawColumns` method trusts the content completely. If user data must be displayed as HTML, sanitize it first using a library like HTML Purifier.

### Safe vs Unsafe Patterns

```php
// ✅ SAFE - Static HTML with no user data
->addColumn('status_badge', '<span class="badge bg-success">Active</span>')
->rawColumns(['status_badge'])

// ✅ SAFE - User data is escaped
->addColumn('name', e($user->name))

// ⚠️ DANGEROUS - User input in raw column
->addColumn('bio', '<p>' . $user->bio . '</p>')
->rawColumns(['bio']) // User's bio could contain malicious scripts!

// ✅ SAFE - Sanitized user data in raw column
use HTMLPurifier;
->addColumn('bio', '<p>' . (new HTMLPurifier())->purify($user->bio) . '</p>')
->rawColumns(['bio'])
```

---

<a name="csrf"></a>
## CSRF Protection

### The Problem

Without CSRF protection, malicious websites could trick users into submitting requests to your DataTables endpoint while they're authenticated.

```
Attacker Site → Hidden Form → Your DataTables Endpoint (with user's session)
```

### Automatic Protection with scripts()

Laravel DataTables handles CSRF automatically when you include the JavaScript properly:

**Step 1: Include scripts in your Blade template**

```blade
@push('scripts')
    {{ $dataTable->scripts() }}
@endpush
```

**Step 2: Ensure CSRF meta tag exists (Laravel default)**

```blade
<head>
    <meta name="csrf-token" content="{{ csrf_token() }}">
</head>
```

**Step 3: Ensure app.blade.php includes the CSRF middleware (default in Laravel)**

```php
// App\Http\Middleware\VerifyCsrfToken.php
protected $except = [
    // Add exceptions only for endpoints that DON'T need CSRF
    // DataTables endpoints should NOT be here
];
```

> [!IMPORTANT]
> The `scripts()` method automatically reads the CSRF token and includes it with all AJAX requests. **Do not** disable this protection.

### What Happens Without scripts()?

If you implement DataTables JavaScript manually without using `scripts()`:

```javascript
// ⚠️ Manual AJAX without CSRF - VULNERABLE
$.ajax({
    url: '/data/users',
    type: 'GET'
    // CSRF token NOT included!
});
```

You must manually add the token to each request:

```javascript
// ✅ Manual implementation WITH CSRF
$.ajaxSetup({
    headers: {
        'X-CSRF-TOKEN': document.querySelector('meta[name="csrf-token"]').content
    }
});
```

---

<a name="mass-assignment"></a>
## Mass Assignment Protection

### The Problem

Without protection, attackers could modify fields they shouldn't access:

```php
// ⚠️ VULNERABLE - User controls all input
User::create($request->all()); // Could include 'is_admin' => true
```

### Using $fillable

Define which fields can be mass-assigned:

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class User extends Model
{
    /**
     * Fields that can be set via mass assignment.
     * Only include fields users should be able to modify.
     */
    protected $fillable = [
        'name',
        'email',
        'password',
    ];

    /**
     * Fields that are NEVER mass-assignable.
     * Security-sensitive fields should ALWAYS be here.
     */
    protected $guarded = [
        'id',
        'is_admin',
        'role',
        'created_at',
        'updated_at',
    ];
}
```

### With DataTables Editor

When using the Editor plugin for inline editing:

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;
use Yajra\DataTables\Editor\Eloquent;
use Yajra\DataTables\Editor\Fields;

class User extends Model
{
    protected $fillable = [
        'name',
        'email',
        'department_id',
    ];

    // Editor integration
    public static function editor()
    {
        return Eloquent::model(new self)
            ->field(Field::text('name')->validator('required|max:255'))
            ->field(Field::text('email')->validator('required|email'))
            ->field(Field::text('department_id')); // User can change department
            // Note: 'is_admin' is intentionally omitted - cannot be changed via editor
    }
}
```

> [!WARNING]
> **Never include security-sensitive fields in `$fillable`.** Fields like `is_admin`, `role`, `password` (when accepting plain text), and `created_at` should always be guarded.

---

<a name="authorization"></a>
## Authorization & Access Control

### Why You Need It

DataTables queries can return sensitive data. Always verify the user has permission to view that data.

### Policy-Based Authorization

```php
use App\Models\User;
use App\Policies\UserPolicy;

Route::get('user-data', function() {
    $query = User::query();

    // Apply policy-based filtering
    if (!auth()->user()->can('viewAny', User::class)) {
        // Return only the current user's data
        $query->where('id', auth()->id());
    }

    return DataTables::eloquent($query)
        ->toJson();
});
```

### Column-Level Authorization

Hide sensitive columns based on permissions:

```php
use Yajra\DataTables\Facades\DataTables;

Route::get('user-data', function() {
    $datatable = DataTables::eloquent(User::query());

    // Only show salary to managers
    if (auth()->user()->isManager()) {
        $datatable->addColumn('salary', fn(User $user) => $user->salary);
    } else {
        $datatable->editColumn('salary', fn(User $user) => '****'); // Masked
    }

    return $datatable->toJson();
});
```

### Route Protection

Ensure endpoints are properly guarded:

```php
// ✅ Protected routes
Route::middleware(['auth', 'verified'])->group(function () {
    Route::get('user-data', [UserController::class, 'data']);
    Route::post('user-data/store', [UserController::class, 'store']);
});

// ⚠️ Public endpoints are dangerous
Route::get('user-data', [UserController::class, 'data']); // No auth!
```

---

<a name="api-security"></a>
## API Security Considerations

### Authentication for API Endpoints

When DataTables serves an API (SPA/mobile):

```php
Route::middleware(['auth:sanctum'])->group(function () {
    Route::get('user-data', [UserController::class, 'data']);
});
```

### Rate Limiting

Protect against abuse:

```php
use Illuminate\Cache\RateLimiting\Limit;
use Illuminate\Support\Facades\RateLimiter;

RateLimiter::for('datatables', function (Request $request) {
    return Limit::perMinute(60)->by($request->user()?->id ?: $request->ip());
});

Route::middleware(['auth:sanctum', 'throttle:datatables'])->group(function () {
    Route::get('user-data', [UserController::class, 'data']);
});
```

---

<a name="best-practices"></a>
## Security Checklist

Use this checklist when implementing DataTables:

- [ ] **XSS**: Only use `rawColumns` with trusted, sanitized content
- [ ] **XSS**: Use `e()` helper when displaying user input
- [ ] **CSRF**: Include `scripts()` method or manually add CSRF token to AJAX
- [ ] **Mass Assignment**: Define `$fillable` with only necessary fields
- [ ] **Mass Assignment**: Keep sensitive fields in `$guarded`
- [ ] **Authorization**: Add policy checks before returning data
- [ ] **Authorization**: Hide sensitive columns based on permissions
- [ ] **API**: Protect endpoints with authentication middleware
- [ ] **API**: Consider rate limiting for expensive queries

---

## See Also

- [XSS Filtering](/docs/{{package}}/{{version}}/xss) - Detailed XSS protection guide
- [Raw Columns](/docs/{{package}}/{{version}}/raw-columns) - Using `rawColumns` safely
- [Editor Installation](/docs/{{package}}/{{version}}/editor-installation) - Secure editor setup

---

**Security Contact**

If you discover a security vulnerability, please email [aqangeles@gmail.com](mailto:aqangeles@gmail.com) directly. Do not report security issues through the public issue tracker.
