---
id: collect-261121-veille-ia/python-php/php-8-6-release-date-and-new-features-what-changes-and-when-to-upgrade
title: "PHP 8.6 Release Date and New Features: What Changes and When to Upgrade"
domain: python-php
role: reference
task: article
actors: []
dates: []
keywords: ["session", "function", "clamp", "release", "readonly", "duration", "cookie", "upgrade"]
source: https://www.cloudhim.com/php-laravel-wordpress/php-8-6-release-date-new-features
source_anchor: ""
source_lines: [1, 113]
section: "Python_PHP"
---
PHP 8.6 is scheduled for general availability on **Thursday, November 19, 2026**. Release candidates have been out since September (RC4 shipped on October 21), so the feature list is frozen. This article covers what's new, the one default change that can affect existing apps, and when it actually makes sense to upgrade a production server.

## PHP 8.6 at a glance

- **Release date:** November 19, 2026 (planned).
- **Support:** PHP branches get two years of active support and two of security fixes, ending on December 31. For 8.6 that points to active support until the end of 2028 and security fixes until the end of 2030; php.net confirms the exact dates when the release goes out.
- **Biggest changes:** partial function application, a built-in`clamp()` , a`Time\Duration` class, default values for readonly properties, and safer session defaults.

## What's new

### Partial function application

You can call a function with some arguments filled in and a `?` placeholder for the rest. Instead of calling the function, PHP returns a closure that takes the missing arguments.

```
$slugify = str_replace(' ', '-', ?);
echo $slugify('hello world');   // hello-world
$titles = array_map(strtoupper(...), $titles);    // first-class callable, PHP 8.1+
$slugs  = array_map(str_replace(' ', '-', ?), $titles);   // new in 8.6
```
It replaces the small `fn($x) => str_replace(' ', '-', $x)` wrappers that fill a lot of array code. One detail to know: every `?` becomes a required parameter of the closure, even if the original parameter was optional.

### clamp()

A built-in for the `max($min, min($max, $value))` pattern:

```
clamp(101, min: 0, max: 100);   // 100
clamp(-5, min: 0, max: 100);    // 0
clamp(42, min: 0, max: 100);    // 42
```
It throws a `ValueError` if `min` is greater than `max`, which catches a class of bug the `min`/`max` version silently hides. If your codebase defines its own global `clamp()` function, rename it or wrap it in `if (!function_exists('clamp'))` before upgrading, or you'll get a "cannot redeclare" error.

### Time\Duration

A final, read-only class for elapsed time with nanosecond precision. It has factory methods, arithmetic and comparisons, so you can stop passing around integers whose unit you have to guess:

```
use Time\Duration;
$timeout = Duration::fromSeconds(30);
$half    = Duration::fromSeconds(1)->divideBy(2);
```
### Readonly properties can have defaults

Before 8.6, giving a readonly property a default value was a compile-time error. Now it's allowed, which removes boilerplate constructors:

```
final class ImportJob
{
    public readonly int $batchSize = 500;
}
```
Readonly still means readonly: the value can't be changed after it's set.

### Smaller additions

- **`SortDirection` enum** with`Ascending` and`Descending` cases. Nothing in core uses it yet; it gives libraries such as query builders a shared type.
- **`Io\Poll`** , one interface over the operating system's polling mechanisms (epoll, kqueue and others) for event-loop libraries, replacing`stream_select()` for that job.
- Doc comments on function parameters, enum `__debugInfo()` , unified stream error handling and URI builder classes that extend the URI extension added in PHP 8.5.

## The change that can affect existing apps: session defaults

Three `php.ini` session defaults change for new installations:

| Setting | Before | PHP 8.6 | 
|---|---|---|
| `session.use_strict_mode` | 0 | 1 | 
| `session.cookie_httponly` | 0 | 1 | 
| `session.cookie_samesite` | (empty) | Lax | 

These are safer: strict mode rejects session IDs the server didn't create, HttpOnly hides the session cookie from JavaScript, and SameSite=Lax stops the cookie being sent on most cross-site requests.

**Laravel** handles its own session cookies, so Laravel apps aren't affected. **WordPress** core logs users in with its own cookies rather than PHP sessions, but some plugins do use `session_start()`. Code that reads the session cookie from JavaScript, or relies on it being sent during a cross-site POST (some older payment gateway callbacks did), can break. If you have such code, test it, or set the old values explicitly in your pool config while you fix it.

## Should you upgrade on release day?

For production, no. Three reasons to wait:

1. **Your dependencies need to support it.** Laravel 13 currently supports PHP 8.3 to 8.5, and WordPress lists 8.3 to 8.5 for its 7.x releases. Both have added new PHP versions after release before, but check their pages rather than assuming. Composer will tell you:`composer why-not php 8.6` .
2. **Distribution packages lag.** Ubuntu 26.04 ships PHP 8.5. PHP 8.6 on Ubuntu or Debian means a third-party repository such as the ondrej/php PPA, Docker images, or compiling it yourself.
3. **The first point releases fix real bugs.** Waiting for 8.6.1 or 8.6.2, a few weeks after GA, costs you nothing.

Do try it early in CI and on a staging server. Running your test suite against 8.6 now tells you what needs fixing before you need to upgrade.

## The upgrade that matters more right now

If any of your servers still run **PHP 8.2**, that's the urgent one: it gets its last security fixes on December 31, 2026. Moving from 8.2 to 8.5 is a better use of November than moving from 8.5 to 8.6. Our PHP 8.2 end of life guide covers how to do it on a VPS, and the PHP end of life schedule has every branch's dates.

## FAQ

### What is the latest PHP version?

Until November 19, 2026, the latest stable version is PHP 8.5. From that date, assuming the schedule holds, it's PHP 8.6.

### Is PHP 8.6 a big release?

No. It's a normal minor release with a handful of language features. Most code that runs on 8.5 runs on 8.6 unchanged. Check the official migration guide on php.net for the full list of backward-incompatible changes and deprecations when it's published.

### Will there be a PHP 9?

Not announced. PHP 8.6 is the next release.

## Bottom line

PHP 8.6 is due on November 19, 2026, with partial function application, `clamp()`, `Time\Duration`, readonly defaults and safer session defaults. Test it in CI now, upgrade production once your framework supports it and a point release or two is out, and deal with any PHP 8.2 servers first.

*Release date and features checked on 2 November 2026 against the PHP 8.6 release candidate announcements and Laravel News' PHP 8.6 overview. If the release date moves, we'll update this page.*

## Comments 0

Be the first to comment.

## Leave a comment
