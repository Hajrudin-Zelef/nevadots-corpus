---
id: collect-261121-veille-ia/python-php/php-8-6-released-new-features-and-what-they-mean-for-joomla
title: "PHP 8.6 Released: New Features and What They Mean for Joomla"
domain: python-php
role: reference
task: article
actors: []
dates: []
keywords: ["joomla", "function", "value", "duration", "time", "released", "site", "before"]
source: https://www.nextsoftware.dev/news/php-8-6-released-joomla
source_anchor: ""
source_lines: [1, 183]
section: "Python_PHP"
---
**PHP 8.6.0 was released on 19 November 2026.** The biggest additions are partial function application, a new `clamp()` function, a `Duration` class for time spans and safer session defaults. Below we explain each one with a short example, show what it means for Joomla sites today, and give our view on when Joomla itself will start using it.

## Should you switch your Joomla site to PHP 8.6 today?

**Not yet on a live site.** Joomla 6.2 on PHP 8.6 still triggers deprecation warnings that the Joomla team is fixing right now (more on that below). In one test report, with PHP's error display switched on, these warnings were printed before the page and the administrator did not load. Our advice:

- Keep live sites on **PHP 8.4 or 8.5** for now. Both are fully supported by PHP and by Joomla 6.
- Try PHP 8.6 on a **staging copy** of your site first. Click through logins, forms and checkout, and watch the logs.
- Move the live site once Joomla and your extensions have released their PHP 8.6 fixes.

Not sure which PHP version your site runs? Go to **System → System Information** in the Joomla administrator. Our PHP2Shell article shows the same check step by step.

## The new features, with examples

### 1. Partial function application

This is the headline feature. You can call a function with some arguments filled in and leave a `?` where the rest go. Instead of running the function, PHP gives you back a closure that waits for the missing values. A `...` placeholder means "all remaining arguments".

Before, you wrote a small arrow function around the call:

```
$safeTitles = array_map(
    fn ($title) => htmlspecialchars($title, ENT_QUOTES, 'UTF-8'),
    $titles,
);
```
In PHP 8.6 the `?` marks the spot where each title goes:

```
$safeTitles = array_map(
    htmlspecialchars(?, ENT_QUOTES, 'UTF-8'),
    $titles,
);
```
It works with methods and static methods too, for example `$logger->log(?, 'warning')`. It does not work with `new`. PHP 8.6 can also compile `array_map()` calls like the one above into a plain loop, so the shorter code is faster too.

### 2. clamp(): keep a value inside a range

Everyone has written `max(min(...))` to keep a number between two limits. Now there is a function for it:

```
// Before
$limit = max(1, min($requestedLimit, 100));
// PHP 8.6
$limit = clamp($requestedLimit, 1, 100);
```
The order is value, minimum, maximum, which is much easier to read than nested `min` and `max`. It works with any values PHP can compare, not only numbers. Think of the "items per page" value a visitor can change in the URL.

### 3. Duration: time spans without unit mistakes

`\Time\Duration` is a small value object for "how long": timeouts, retry delays, benchmark times. It holds nanoseconds and has no time zone, so you no longer have to remember whether a number means seconds, milliseconds or microseconds.

```
use Time\Duration;
$timeout = Duration::fromSeconds(30);
$step    = Duration::fromMilliseconds(250);
$delay = $step->multiplyBy(2 ** $attempt);
$delay = min($delay, $timeout);   // Durations can be compared
time_nanosleep($delay->seconds, $delay->nanoseconds);
```
### 4. Readonly properties can have a default value

Before PHP 8.6, a `readonly` property could only be set in the constructor. Now it can have a default, and that default counts as the final value:

```
// Before
final readonly class ExportSettings
{
    public string $format;
    public function __construct()
    {
        $this->format = 'csv';
    }
}
// PHP 8.6
final readonly class ExportSettings
{
    public string $format = 'csv';
}
```
### 5. A SortDirection enum

Joomla list views pass the sort order around as the text `'ASC'` or `'DESC'`. A typo there is only noticed when the query runs. PHP 8.6 adds a built-in `SortDirection` enum with two cases, so a wrong value is caught as soon as it is passed in:

```
public function orderBy(
    string $column,
    SortDirection $direction = SortDirection::Ascending,
): static {
    $this->order[] = $column . ' ' . match ($direction) {
        SortDirection::Ascending  => 'ASC',
        SortDirection::Descending => 'DESC',
    };
    return $this;
}
$query->orderBy('title', SortDirection::Descending);
```
### 6. Clear errors from file and stream functions

Functions such as `file_get_contents()` used to fail with a warning and `false`, and you had to read `error_get_last()` to guess what went wrong. In PHP 8.6 you can ask for an exception with a real error code instead:

```
$context = stream_context_create([
    'stream' => ['error_mode' => StreamErrorMode::Exception],
]);
try {
    $json = file_get_contents($feedUrl, context: $context);
} catch (StreamException $e) {
    foreach ($e->getErrors() as $error) {
        // $error->code is a StreamErrorCode enum case
        Log::add($error->code->name . ': ' . $error->message, Log::WARNING);
    }
}
```
### 7. Smaller changes worth knowing

- **Doc comments on parameters** , readable with the new`ReflectionParameter::getDocComment()` .
- **The polling API** (`Io\Poll` ) watches thousands of network connections at once. It is mainly for event-loop libraries such as ReactPHP and AMPHP.
- **`mysqli::quote_string()`** escapes and quotes a value in one call, like`PDO::quote()` .
- **JSON errors show where the problem is** in the input, which makes broken API responses quicker to debug.
- **`#[\Override]`** now works on class constants and enum cases.
- **Post-quantum encryption** (ML-KEM-768 and X-Wing) in the Sodium extension.
- **`ZipArchive::openString()`** opens a ZIP file straight from memory.

## What can break on a Joomla site

Most code runs unchanged. These are the changes we expect Joomla sites and extensions to notice first:

- **Safer session defaults.** Strict session IDs and HttpOnly cookies are now on by default, and session cookies get`SameSite=Lax` . Joomla already sets HttpOnly on its own session cookie.`SameSite=Lax` matters for anything that comes back to your site with a cross-site POST, such as some payment gateways and single sign-on services. Chrome already treats cookies this way, but test these flows in Firefox and Safari too.
- **`spl_object_hash()` is deprecated.** Use`spl_object_id()` . Joomla core still calls it when it loads plugins.
- **Session handlers need `create_sid()` and `validateId()`.** Handlers without them now cause a deprecation notice, and in PHP 9.0 they will stop working.
- **`return` inside `finally` is deprecated** , because it silently discards exceptions.
- **Old aliases are deprecated:**`is_integer()` ,`is_long()` ,`is_double()` and`doubleval()` . Use`is_int()` ,`is_float()` and`floatval()` .
- **The `mb_ereg*()` functions and `mb_split()` are deprecated.** Use`preg_*` with the`u` modifier.
- **More `ValueError` exceptions.** Many functions now throw on invalid flags, out-of-range values or strings with NUL bytes, where they used to ignore them.
- **`trim()` now also removes form feed characters** (`\f` ).

## What the Joomla project is doing now

The work is public on GitHub, and so far it is about making Joomla run cleanly on PHP 8.6, not about using the new features:

- In August, Joomla contributor Brian Teeman opened issue #48218, "php 8.6 support", to track the deprecations ahead of the release.
- A tester running Joomla 6.2 on a PHP 8.6 beta got warnings on the error page: missing `create_sid()` and`validateId()` in the session handlers, and the`spl_object_hash()` deprecation in the plugin helper.
- The plugin helper fix is pull request #48520, aimed at Joomla 6.3 and still open.
- The session handlers live in the separate Joomla Framework. That fix is tracked in joomla-framework/session issue #69, which has no reply yet.

We wrote about the current Joomla release in Joomla 6.2 "Mwezi" explained.

## Our thoughts: when will Joomla core use PHP 8.6 code?

*This section is our own estimate. Joomla has not announced a PHP 8.6 plan.*

Running on PHP 8.6 and using PHP 8.6 features are two different things. Core code can only use new syntax such as `?` placeholders or readonly defaults once every supported site has PHP 8.6, which means once it is Joomla's **minimum** PHP version. Joomla only raises that minimum with a new major version, and the last two majors show a clear pattern:

| Joomla | Released | Minimum PHP | That PHP came out | 
|---|---|---|---|
| 5.0 | October 2023 | 8.1 | November 2021 | 
| 6.0 | October 2025 | 8.3 | November 2023 | 
| 7.0 | October 2027 (expected) | 8.5? *our estimate* | November 2025 | 
| 8.0 | around October 2029 | 8.6 or newer? *our estimate* | November 2026 or later | 

Joomla picks the PHP version released about two years before the new major. Two more things point the same way. PHP 8.3, the current minimum, stops getting security fixes on 31 December 2027, two months after Joomla 7 is due. And the Joomla 7 development branch still says PHP 8.3 today, so the decision is still open.

So this is how we see it:

- **Joomla 6.x (now to 2027):** compatibility fixes only. Joomla 6 will run on PHP 8.6 without warnings, probably from 6.3, but the code will not use the new features.
- **Small functions could come sooner.**`clamp()` and`SortDirection` are plain functions and classes, and the symfony/polyfill-php86 package already provides them for older PHP. Joomla has used Symfony polyfills before, so this is possible, but there is no sign of it yet.
- **New syntax in core: Joomla 8, around 2029.** Partial function application, readonly defaults and parameter doc comments cannot be polyfilled. Old PHP versions cannot even read them. If Joomla 7 requires PHP 8.5, as the pattern suggests, Joomla 8 is the first version that can require 8.6. If Joomla 7 keeps 8.3 or 8.4, it is still Joomla 8.

For site owners this means no hurry. PHP 8.6 is an option for the next few years, not something Joomla will force on you soon.

## What we are doing at NextSoftware

We are testing all our Joomla extensions on PHP 8.6 now and will release fixes for anything we find. Until those tests are done, we recommend PHP 8.4 or 8.5 for live sites.

If you build Joomla extensions too, here is the approach we use:

- **Write code for the Joomla minimum, test on the newest PHP.** Today that means PHP 8.3 syntax, with PHP 8.6 added to your tests.
- **Fix deprecations now.** Replace`spl_object_hash()` ,`is_integer()` and`return` in`finally` . They are cheap to fix now and will be errors in PHP 9.
- **If you want `clamp()` or `SortDirection` today,** add`symfony/polyfill-php86` to your extension's own dependencies. Don't count on Joomla to ship it.
- **Test cross-site login and payment returns** with the new session cookie defaults.

Have custom extensions that need to be ready for PHP 8.6? Our Joomla Migration & Upgrades service includes compatibility updates for custom extensions.
