---
id: collect-261121-veille-ia/python-php/php-8-6-is-here-and-it-s-a-good-one-php-architect
title: "PHP 8.6 is here, and it's a good one | PHP Architect"
domain: python-php
role: reference
task: article
actors: []
dates: []
keywords: ["duration", "php", "here", "stream", "time", "value", "file", "context"]
source: https://phparch.com/blog/php-86-is-here-and-its-a-good-one
source_anchor: ""
source_lines: [1, 162]
section: "Python_PHP"
---
*Meta description: PHP 8.6 shipped on November 19, 2026. Here's a quick, friendly tour of the best new features, with short code examples you can try today.*

PHP 8.6 landed on November 19, and it's the kind of release developers love. There's no giant rewrite to survive. Instead you get a pile of small, practical tools that make everyday code shorter and clearer, plus a couple of building blocks that will pay off for years.

Here's a tour of the highlights. Nothing deep, just enough to know what each feature is and how to use it.

## Partial function application

This one is my favorite. You can now call a function with only some of its arguments, put a `?` where the missing ones go, and get back a closure that waits for the rest.

```
$lastThree = array_slice(?, -3);
print_r($lastThree([1, 2, 3, 4, 5]));
// [3, 4, 5]
$loud = strtoupper(?);
print_r(array_map($loud, ['php', 'tek', 'arch']));
// ['PHP', 'TEK', 'ARCH']
```
Before, you'd write a small arrow function by hand. Now the intent is right there in the call. If you want to leave every remaining argument open, use `...` instead of `?`. It works with your own functions and with PHP's built-in ones, and it fits nicely with the pipe operator, which likes callables that take a single argument.

## A proper Duration class

Anyone who has passed "300" around as a timeout and then wondered whether it meant seconds or milliseconds will appreciate `Time\Duration`. It represents a plain stretch of elapsed time with nanosecond precision, like a stopwatch reading. It has no calendar baggage, so a duration always means the same thing.

```
use Time\Duration;
$retryDelay = Duration::fromMinutes(5)->multiplyBy(3);
echo $retryDelay->seconds; // 900
if ($retryDelay > Duration::fromMinutes(10)) {
    echo "That's a long time to wait.";
}
```
You can create durations from seconds, milliseconds, minutes, or an ISO 8601 string, and add, subtract, multiply, and divide them. They're immutable and comparable, so `min()`, `max()`, and plain comparison operators just work. Very small feature, very big readability win.

## clamp()

Keep a number inside a range. That's the whole feature, and you've written it by hand a hundred times.

```
$page = clamp((int) ($_GET['page'] ?? 1), 1, 50);
```
If the value is below the minimum, you get the minimum. If it's above the maximum, you get the maximum. Otherwise you get the value back. It works with anything comparable, including the new `Duration` objects.

## Readonly properties with defaults

Readonly properties can now declare a default value. That sounds minor until you try to satisfy a get-only property from an interface with a fixed value, which used to be a compile-time error.

```
interface Exportable
{
    public string $format { get; }
}
final class PdfExport implements Exportable
{
    public readonly string $format = 'pdf';
}
```
The default counts as the initialization, so nothing can overwrite it later, not even the constructor. It pairs well with the property hooks that arrived in PHP 8.4.

## Friendlier stream errors

File and stream functions have always been a little awkward about failure. You got a warning, or a quiet `false`, and then you had to dig. In 8.6 you can ask streams to throw a `StreamException` instead.

```
$context = stream_context_create([
    'stream' => ['error_mode' => StreamErrorMode::Exception],
]);
try {
    $data = file_get_contents('/missing/file.txt', context: $context);
} catch (StreamException $e) {
    echo 'Could not read the file: ', $e->getMessage();
}
```
The errors are structured objects now, with an enum code, the wrapper name, and the path involved. That makes logging and handling far less of a guessing game. If you'd rather not use exceptions, `stream_last_errors()` gives you the same details after the fact.

## A polling API for the long game

PHP 8.6 adds `Io\Poll`, a low-level way to watch many streams at once using the best mechanism each platform offers (epoll, kqueue, event ports, or WSAPoll). The old `stream_select()` tops out at around a thousand connections, so this removes a real ceiling.

To be clear about what it is: this is not a new async feature, and it doesn't come with an event loop. It's a foundation. Libraries like ReactPHP and AMPHP can build on it, and you'll likely benefit without ever touching it directly. That's the best kind of upgrade.

## A shared SortDirection enum

PHP now ships a built-in `SortDirection` enum with `Ascending` and `Descending` cases. Libraries and frameworks can finally agree on one type instead of each inventing their own.

```
function sortNames(array $names, SortDirection $direction = SortDirection::Ascending): array
{
    if ($direction === SortDirection::Ascending) {
        sort($names);
    } else {
        rsort($names);
    }
    return $names;
}
print_r(sortNames(['Ada', 'Linus', 'Grace'], SortDirection::Descending));
```
Nothing in core accepts it yet, but the groundwork is laid.

## Smaller upgrades worth knowing

There's a long tail of nice touches in this release:

- Parameters can carry their own doc comments, and `ReflectionParameter::getDocComment()` reads them. Static analysis tools should love this.
- `ReflectionProperty` gains`isReadable()` and`isWritable()` , which understand visibility, hooks, and initialization state.
- `#[\Override]` now works on class constants and enum cases, so typos in overridden constants become compile-time errors.
- Enums can define `__debugInfo()` to control`var_dump()` output.
- JSON error messages now point to where in the input the problem is.
- There are new URI builder classes, `mysqli::quote_string()` , endianness modifiers for`pack()` and`unpack()` , and`ZipArchive::openString()` for in-memory archives.
- The Sodium extension adds post-quantum key encapsulation, and TLS streams get session resumption and TLS 1.3 early data.

## Safer session defaults

Three session settings now default to the safer choice:

```
session.use_strict_mode = 1
session.cookie_httponly = 1
session.cookie_samesite = Lax
```
This is a quiet security win for every app that never touched these settings. The one caveat: if your app reads the session cookie from JavaScript, or depends on cross-site POST requests carrying the cookie, test before you deploy.

## Deprecations to watch

A few things now raise deprecation notices. None are dramatic, but a quick scan of your logs will tell you if you're affected.

A `return` inside a `finally` block is deprecated, because it quietly overrides whatever the `try` or `catch` was doing:

```
function loadRows(): array
{
    try {
        return fetchRows();
    } finally {
        return []; // deprecated: this silently replaces the real result
    }
}
```
The old type-check aliases `is_double()`, `is_integer()`, and `is_long()` are deprecated, along with `doubleval()`. Use `is_float()`, `is_int()`, and `floatval()`. Using `let` or `is` as a class or function name is deprecated, as is `spl_object_hash()` in favor of `spl_object_id()`. The mbstring regex functions such as `mb_ereg()` are on the way out too, so move to PCRE with the `u` modifier.

## A little faster, too

The release notes mention several speed-ups: `printf()` and JSON encoding are quicker, stateless closures are cached, and `array_map()` calls with a first-class callable or a partial application get compiled down to a plain loop. The announcement doesn't publish benchmark numbers, so I won't invent any. Measure your own app and see.

## Should you upgrade?

Yes, and this is a good time. PHP 8.6.0 shipped on the same day as security releases for 8.2 through 8.5, so it includes those fixes. A sensible path looks like this:

1. Run your test suite on 8.6 with `error_reporting` set to`E_ALL` , so deprecations show up.
2. Check anything that touches session cookies.
3. Swap the old aliases and `finally` returns as you find them.
4. Roll out to staging, then production.

PHP keeps getting more expressive without losing what makes it easy to love. Partial application, a real duration type, and cleaner error handling are all small on their own, but together they make everyday code nicer to write and nicer to read. Go give 8.6 a spin.

## References

- PHP.net, "PHP 8.6 Release Announcement": https://www.php.net/releases/8.6/en.php
- PHP Manual, "New Features" (PHP 8.6 migration guide): https://www.php.net/manual/en/migration86.new-features.php
- Brent Roose, "What's new in PHP 8.6," stitcher.io: https://stitcher.io/blog/new-in-php-86
- Laravel News, "PHP 8.6 release day": https://laravel-news.com/php-8-6-release-day
- PHP.net news archive, 2026 (security release announcements of November 19, 2026): https://www.php.net/archive/2026.php
