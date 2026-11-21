---
id: collect-261121-veille-ia/python-php/php-8-6-released-new-features-breaking-changes
title: "PHP 8.6 Released: New Features & Breaking Changes"
domain: python-php
role: reference
task: article
actors: []
dates: ["2026-11-19"]
keywords: ["clamp", "time", "readonly", "function", "duration", "features", "class", "released"]
source: https://tecadmin.net/php-8-6-released-whats-new/
source_anchor: ""
source_lines: [1, 146]
section: "Python_PHP"
---
**PHP 8.6.0** is released on **19 November 2026**, one year after PHP 8.5. The headline features are **partial function application**, a new **`clamp()`** function, a **`Time\Duration`** class, a native **polling API** for non-blocking I/O, and **default values for readonly properties**. PHP 8.6 also makes sessions **more secure by default**, which is the change most likely to affect existing applications.

The release was managed by Daniel Scherzer, Matteo Beccati and Joe Ferguson, after three alphas, three betas and release candidates up to RC7.

## PHP 8.6 at a Glance

| Release date | 19 November 2026 | 
|---|---|
| Version | PHP 8.6.0 | 
| Headline features | Partial function application, `clamp()` ,`Time\Duration` ,`Io\Poll` polling API, readonly property defaults | 
| Smaller additions | `#[\Override]` on class constants,`SortDirection` enum,`ReflectionProperty::isReadable()` /`isWritable()` | 
| Behaviour change | Secure session defaults (strict mode, HttpOnly, SameSite=Lax) | 
| Support | 2 years of active support + 2 years of security fixes | 
| Previous release | PHP 8.5 (20 November 2025) | 

## What’s New in PHP 8.6

### Partial function application

You can now create a new callable from a function call by leaving some arguments out. A `?` stands for one missing argument and `...` stands for all remaining ones. The arguments you do pass are evaluated when the callable is created:

```
```
```
$toSlug = str_replace(' ', '-', ?);
echo $toSlug('hello php world');   // hello-php-world
```
It works especially well with the pipe operator (`|>`) that arrived in PHP 8.5, because a pipeline step must be a callable that takes one argument:

```
```
```
$slug = $title
    |> trim(...)
    |> strtolower(...)
    |> str_replace(' ', '-', ?);
```
Before PHP 8.6, the last step needed a closure such as `fn($s) => str_replace(' ', '-', $s)`. Note that `new` can’t be partially applied, but static factory methods can.

### The clamp() function

`clamp()` limits a value to a range, replacing the familiar `max($min, min($max, $value))` pattern:

```
```
```
clamp(42,  min: 0, max: 100);   // 42
clamp(150, min: 0, max: 100);   // 100
clamp(-5,  min: 0, max: 100);   // 0
```
It uses PHP’s normal comparison rules, so it also works with strings and `DateTime` objects. It throws a `ValueError` if `$min` is greater than `$max`, or if either bound is `NAN`.

### Time\Duration

The new `Time\Duration` class is an immutable (`final readonly`) value object for a length of time with nanosecond precision. You create one with named factories such as `fromSeconds()`, `fromMilliseconds()`, `fromMinutes()`, `fromHours()` or `fromIso8601DurationString()`, and do maths with `add()`, `sub()`, `multiplyBy()`, `divideBy()`, `negate()` and `absolute()`:

```
```
```
use Time\Duration;
// exponential back-off: 100 ms, 200 ms, 400 ms ...
$delay = Duration::fromMilliseconds(100)->multiplyBy(2 ** $attempt);
```
No more guessing whether an integer timeout means seconds or milliseconds. Errors are reported with the new `Time\TimeException`.

### A native polling API

The new `Io\Poll` namespace gives PHP one interface for waiting on many streams at once, on top of the best mechanism each platform offers: `epoll` on Linux, `kqueue` on BSD and macOS, event ports on Solaris and `WSAPoll` on Windows. The main classes are `Io\Poll\Context` and `Io\Poll\Watcher`, with `Backend` and `Event` enums. Until now, PHP code had only `stream_select()`, which scales poorly, or extensions such as ev and event. Async libraries and frameworks (ReactPHP, AMPHP, Revolt and similar) are the main audience.

### Readonly properties can have defaults

Readonly properties can now declare a default value directly, including in readonly classes. The default counts as the property’s one initialisation, so it can’t be changed later in the constructor:

```
```
```
final class ApiClient
{
    public readonly int $timeout = 30;
}
```
### #[\Override] on class constants

The `#[\Override]` attribute from PHP 8.3 now also works on class constants. PHP checks that a public or protected constant with the same name exists in a parent class or interface, and reports an error if it doesn’t, for example after the parent constant was renamed:

```
```
```
class Base { protected const LIMIT = 10; }
class Child extends Base
{
    #[\Override]
    public const LIMIT = 50;
}
```
### SortDirection enum and other additions

- A global `SortDirection` enum with the cases`Ascending` and`Descending` , intended for library and framework APIs. No built-in function accepts it yet.
- `ReflectionProperty::isReadable()` and`ReflectionProperty::isWritable()` check whether a property can be read or written from a given scope, taking visibility, readonly and hooks into account.
- Enums can implement `__debugInfo()` , and`ReflectionParameter` can read doc comments on function parameters.
- Error messages can include the function’s arguments (new `error_include_args` INI setting).
- New `grapheme_strrev()` ,`mysqli_quote_string()` ,`Locale::getDisplayKeyword()` and`Locale::getDisplayKeywordValue()` ; TLS session resumption for streams;`trim()` now also strips the form-feed character (`\f` ).

## Before You Upgrade: Breaking Changes

### Secure session defaults

Three session settings change their built-in defaults:

| Setting | PHP 8.5 | PHP 8.6 | 
|---|---|---|
| `session.use_strict_mode` | 0 | 1 | 
| `session.cookie_httponly` | 0 | 1 | 
| `session.cookie_samesite` | (empty) | Lax | 

Most apps are only safer for it, but check three cases: JavaScript that reads the session cookie (it no longer can), cross-site POST flows such as some payment or SSO callbacks (they need `SameSite=None; Secure`), and code that hands over a session ID that PHP never created (strict mode rejects it). Frameworks that set their own cookie options, like Laravel and Symfony, are usually unaffected. To check what a server is using:

```
```
`php -i | grep -E "session.(use_strict_mode|cookie_httponly|cookie_samesite)"`
### Deprecations

- Returning a value from `__construct()` or`__destruct()` is deprecated.
- The `mb_ereg*()` functions and`mb_split()` are deprecated, because the Oniguruma regex library they rely on is no longer maintained. Use the`preg_*()` functions instead.
- The yearly deprecation round targets old aliases and rarely used APIs. Run your test suite with `E_DEPRECATED` enabled and read the official migration guide for the full list.

## Support Timeline

PHP versions get two years of active support (bug and security fixes) followed by two years of security fixes only, with each phase ending on 31 December. Under that policy, PHP 8.6 should be actively supported until **31 December 2028** and receive security fixes until **31 December 2030**. PHP 8.5 stays in active support until 31 December 2027, and **PHP 8.2 reaches end of life on 31 December 2026**, so this is a good time to move off it. See the PHP release history for every version and its dates.

## How to Get PHP 8.6

Source tarballs and Windows builds are available from php.net/downloads on release day. The official Docker images usually follow within hours, and you can test your code right away:

```
```
`docker run --rm php:8.6-cli php -v`
On Ubuntu and Debian, PHP 8.6 isn’t in the distribution repositories yet. Most servers get new PHP versions from the `ondrej/php` PPA (Ubuntu) or the sury.org repository (Debian), which normally add a new release shortly after GA. Once it’s there, the install steps match our PHP 8.5 on Ubuntu 26.04 guide, with `8.5` replaced by `8.6`.

## Should You Upgrade?

For new projects, yes: start on PHP 8.6. For existing applications, test first. Run your test suite on 8.6, watch for deprecation notices, and check the session cookie behaviour above, especially in login, SSO and payment flows. Wait until your framework, CMS and key Composer packages officially support 8.6 before switching production; for most popular projects that takes a few weeks. If you are still on PHP 8.2 or older, plan the upgrade now, because 8.2 stops receiving security fixes at the end of 2026.

Sources: PHP 8.6 release announcement, PHP 8.6 release timetable and the PHP RFC index.

How did it make you feel?
