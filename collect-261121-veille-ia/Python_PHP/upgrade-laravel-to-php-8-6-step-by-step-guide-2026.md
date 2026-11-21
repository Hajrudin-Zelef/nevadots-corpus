---
id: collect-261121-veille-ia/python-php/upgrade-laravel-to-php-8-6-step-by-step-guide-2026
title: "Upgrade Laravel to PHP 8.6: Step-by-Step Guide (2026)"
domain: python-php
role: reference
task: article
actors: []
dates: ["2026-11-19"]
keywords: ["laravel", "function", "upgrade", "check", "guide", "version", "release", "composer"]
source: https://blog.shakiltech.com/upgrade-laravel-php-8-6/
source_anchor: ""
source_lines: [1, 191]
section: "Python_PHP"
---
PHP 8.6 reaches general availability on **November 19, 2026**, bringing partial function application, `clamp()`, `Time\Duration` and more. This guide takes a Laravel app from 8.5 to 8.6 in the order that keeps production safe, and covers the deprecations you’ll actually meet along the way.

The short version: **test early, upgrade production late.** PHP minor upgrades are usually smooth, but a single unsupported package or extension can block you.

## Step 0: Check compatibility first

### Laravel itself

Laravel’s support table lists **PHP 8.3 to 8.5 for Laravel 13**. Laravel usually adds a new PHP version in a point release shortly after GA, so check the release notes for a Laravel 13.x release that adds PHP 8.6 support and update to it first. If you’re on Laravel 12 or older, upgrade the framework before PHP.

### Your packages

Composer can tell you which installed packages would block PHP 8.6:

```
composer why-not php 8.6
```
Anything listed has a `php` constraint that excludes 8.6. Check each one’s changelog for a newer release. The usual suspects are packages with tight constraints like `^8.2 <8.6`, or old versions of testing tools.

### Your hosting

Forge, Laravel Cloud and most managed hosts add new PHP versions within days to weeks of GA. Check that your host offers 8.6 before you plan the production switch.

## Step 1: Install PHP 8.6 locally

- **Laravel Herd:** pick PHP 8.6 in Herd’s PHP settings once it’s offered, then switch your site’s version.
- **Homebrew:**`brew install php@8.6` (or`brew upgrade php` once 8.6 becomes the default formula).
- **Ubuntu:** the`ondrej/php` PPA usually publishes new versions quickly:`sudo apt install php8.6 php8.6-{cli,fpm,mbstring,xml,curl,mysql,zip,bcmath,intl,redis}` .
- **Sail / Docker:** change the PHP version in your runtime and rebuild. The official`php:8.6` images no longer ship`pecl` or build in mbstring; my Docker and PIE guide covers the fix.

Check it:

```
php -v
php -m   # confirm the extensions your app needs are loaded
```
Extensions from outside php-src (Redis, Imagick, Swoole and similar) need builds for 8.6. A late security fix changed PHP 8.6’s ABI during the release candidates, so extensions built against early RCs must be rebuilt. If one isn’t ready, that’s your blocker.

## Step 2: Update Composer and run the tests

In `composer.json`, widen the PHP constraint if you pin it:

```
"require": {
    "php": "^8.3"
}
```
`^8.3` already allows 8.6. If you use `config.platform.php` to pin a version for dependency resolution, change it to `8.6` only once production is on 8.6.

```
composer update
php artisan test
```
## Step 3: Fix the deprecations

PHP 8.6 adds **31 deprecations**, all accepted in one mass-deprecation vote in August. **Nothing here breaks your app today**: a deprecation raises an `E_DEPRECATED` notice and the feature keeps working. Removal is targeted at PHP 9. But notices fill logs, can fail test suites, and are your to-do list for PHP 9. These are the ones most likely to show up in a Laravel codebase.

### New reserved names

These matter most, because they affect names that are legal today:

| Name | Deprecated use | 
|---|---|
| `is` | as a function, class, constant or other identifier | 
| `let` | as an identifier | 
| `readonly` | as a function name | 
| `namespace` | as a class constant name | 
| `_` | as a constant or compile-time alias | 

**Check `is` first.** The Hamcrest matcher library, which Mockery depends on, uses an `is()` function, and an early warning about `is` as a class name showed up in `guzzlehttp/promises`. Don’t patch vendor code; keep packages updated. In your own code, rename:

```
// Before: deprecated in 8.6
function is(mixed $value): Matcher { /* ... */ }
class Status { const namespace = 'orders'; }
// After
function isEqualTo(mixed $value): Matcher { /* ... */ }
class Status { const NAMESPACE_NAME = 'orders'; }
```
### `return` inside `finally`

This one is a real behaviour fix. A `return` in `finally` silently swallows any exception thrown in the `try` block:

```
// Before: the exception is lost, the function returns null
function charge(Order $order) {
    try {
        return $this->gateway->charge($order);
    } finally {
        Log::info('Charge attempted');
        return null;   // deprecated in 8.6
    }
}
// After: finally only cleans up
function charge(Order $order) {
    try {
        return $this->gateway->charge($order);
    } finally {
        Log::info('Charge attempted');
    }
}
```
If you find one, check whether the swallowed exception was hiding a real failure.

### Type-check aliases

| Deprecated | Use instead | 
|---|---|
| `is_integer()` ,`is_long()` | `is_int()` | 
| `is_double()` | `is_float()` | 
| `doubleval()` | `floatval()` | 

A find-and-replace job, and Rector can automate it.

### Others worth grepping for

- **`strcoll()` and `SORT_LOCALE_STRING`:** use the`intl` extension’s`Collator` .
- **`spl_object_hash()`:** use`spl_object_id()` .
- **`SplFileObject` CSV methods:** consider`league/csv` .
- **`is_a()` / `is_subclass_of()` with a string** when`$allow_string` is`false` .
- **Passing objects where arrays are expected** in functions like`array_walk()` and`http_build_query()` .

**Not deprecated after all:** `list()` survived (the vote tied 23 to 23), as did `_()` as the gettext alias. Reserving `in`, `out` and `inout` also failed.

### How to find them

1. **Route them to their own log.** Set`LOG_DEPRECATIONS_CHANNEL=deprecations` in`.env` and add a`deprecations` channel (a daily file works well) in`config/logging.php` .
2. **Make tests fail on them** in`phpunit.xml` :

```
<phpunit failOnDeprecation="true" displayDetailsOnTestsThatTriggerDeprecations="true">
```
1. **Grep your own code:**

```
grep -rnE "\b(is_integer|is_long|is_double|doubleval|strcoll|spl_object_hash)\(" app/ config/ database/ routes/
grep -rnE "function (is|let|readonly)\(" app/
```
1. **Run PHPStan or Larastan** configured for PHP 8.6; it catches`return` in`finally` more reliably than grep.

Notices from `vendor/` aren’t yours to fix: update the package or check its issue tracker, and focus on `app/`, `config/`, `database/` and `routes/`.

## Step 4: Add PHP 8.6 to CI

Run both versions side by side until production moves. In GitHub Actions:

```
strategy:
  matrix:
    php: ['8.5', '8.6']
steps:
  - uses: shivammathur/setup-php@v2
    with:
      php-version: ${{ matrix.php }}
```
## Step 5: Roll out to production

1. **Staging first.** Switch staging to 8.6, deploy, and run through your critical flows: login, checkout, queues, scheduled tasks.
2. **Restart long-running processes.** Queue workers, Horizon, Octane and Reverb keep the old PHP binary in memory.
3. **Watch your logs** for the first day, especially the deprecations channel and extension errors.
4. **Keep a rollback path.** On Forge you can switch the site back to 8.5 in a click; know the equivalent for your host before you start.

## Troubleshooting

| Problem | Fix | 
|---|---|
| `composer update` fails on a PHP constraint | A package doesn’t allow 8.6 yet. Check for a newer release. `--ignore-platform-req=php` is for local testing only, never deploys. | 
| “Class not found” or fatal errors after switching | An extension isn’t installed for 8.6. Compare `php -m` between versions. | 
| Queue jobs behave like the old version | Workers weren’t restarted: `php artisan queue:restart` or restart Horizon / Supervisor. | 
| Stale code after the switch | Restart PHP-FPM to clear OPcache. | 
| Logs flooded with deprecations | Send them to the separate channel while you fix them, rather than silencing them. | 

## FAQ

**Does Laravel 13 support PHP 8.6?**

Laravel 13 launched supporting PHP 8.3 to 8.5. Support for 8.6 typically arrives in a 13.x point release shortly after PHP’s GA, so update Laravel first and check its release notes.

**Will PHP 8.6 deprecations break my app?**

No. Deprecations only raise notices in 8.6. The features are removed in PHP 9, so fix them now while it’s cheap.

**Which PHP 8.6 deprecation affects Laravel apps most?**

The newly reserved name `is`, mainly through dependencies like Hamcrest. Keep Mockery and other testing packages up to date.

## Conclusion

Check Laravel, packages and hosting; install 8.6 locally; run tests with deprecations visible and fix the ones in your own code; add 8.6 to CI; then move staging, then production. For most Laravel apps it’s an afternoon’s work.

When are you planning to move production to 8.6? Tell me in the comments.
