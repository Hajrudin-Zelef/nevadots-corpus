---
id: collect-261121-veille-ia/python-php/what-s-new-in-php-8-6
title: "What's New in PHP 8.6"
domain: python-php
role: reference
task: article
actors: []
dates: ["2026-11-19"]
keywords: ["class", "duration", "function", "override", "november", "clamp", "readonly", "property"]
source: https://laravel-news.com/php-8-6
source_anchor: ""
source_lines: [1, 287]
section: "Python_PHP"
---
PHP's annual release is just around the corner! PHP 8.6 will be released on November 19, 2026. This post covers the new features, changed defaults, and deprecations in the release.

## PHP 8.6 Release Timeline

- Alpha 1 through Alpha 3: July 2 to July 30, 2026
- Beta 1 (soft feature freeze): August 13, 2026
- Beta 3: September 10, 2026
- Feature freeze: September 22, 2026
- RC 2: September 24, 2026 (RC 1 was skipped)
- RC 3: October 8, 2026
- RC 4: October 21, 2026
- RC 5: November 5, 2026
- RC 6: November 9, 2026
- RC 7: November 12, 2026 (RC 6 was tagged and then skipped)
- GA: November 19, 2026

The release managers are Daniel Scherzer, Matteo Beccati, and Joe Ferguson.

## Partial Function Application

Partial function application (PFA) lets you call a function with some arguments filled in and get back a closure that takes the rest. The `?` placeholder marks a single argument to fill later, and `...` stands in for all remaining arguments:

$makeSlug = str_replace(' ', '-', ?);
$makeSlug('Hello World'); // Hello-World
$titles = array_map(strtolower(?), $titles);

The resulting closure keeps the parameter names, types, and defaults of the original function. Arguments you supply are evaluated when the partial is created, not when it is called. Every `?` placeholder becomes a required parameter on the closure, even if the original parameter was optional.

## The clamp() Function

The new `clamp()` function returns the value if it falls within the bounds, or the nearest bound if it does not:

clamp(10, min: 0, max: 100);  // 10
clamp(101, min: 0, max: 100); // 100
clamp(-1, min: 0, max: 100);  // 0

It works with any comparable type, including strings and `DateTime` objects. Passing a `$min` greater than `$max` throws a `ValueError`. This replaces the common `min(max($value, $min), $max)` pattern, which is easy to get backwards.

## A Duration Class

PHP 8.6 adds `Time\Duration`, a final readonly class that represents a stopwatch-style length of time with nanosecond precision. It has factory methods for each unit, arithmetic methods, and comparison support:

use Time\Duration;
$oneSecond = Duration::fromSeconds(1);
$halfSecond = $oneSecond->divideBy(2);
$total = $oneSecond->add($halfSecond);
$delay = Duration::fromMilliseconds(100)->multiplyBy(2 ** $attempt);
$total > $delay; // comparison operators work

Durations can also be built from an ISO 8601 duration string. The class is intended as a shared type that core functions and the new polling API can accept instead of loose integers and floats. See the Duration RFC for the full method list.

## Readonly Property Defaults

Starting in PHP 8.6, readonly properties can have a default value. Before 8.6 this was a compile-time error, which made it awkward to satisfy get-only interface properties from PHP 8.4 with a fixed value:

final readonly class CreateBooksTable implements Migration
{
    public string $name = '2026_01_01_create_books_table';
}

The RFC only removes the restriction. Readonly semantics are unchanged, so the property still cannot be reassigned after initialization.

## #[\Override] on Class Constants

PHP 8.3 added the `#[\Override]` attribute for methods, and PHP 8.5 extended it to properties. In PHP 8.6 it also works on class constants and enum cases. A constant marked with the attribute must override a public or protected constant from a parent class or interface, or PHP throws a fatal error:

class Base
{
    protected const VERSION = '1.0';
}
class Child extends Base
{
    #[\Override]
    public const VERSION = '2.0'; // OK
    #[\Override]
    public const VERSOIN = '2.0'; // Fatal error: Child::VERSOIN has #[\Override] attribute, but no matching parent constant exists
}

## Property Writes on Objects Stored in Constants

Since PHP 8.1, a global constant can hold an object created with `new`. Writing to a property of that object failed with "Cannot use temporary expression in write context". PHP 8.6 allows property writes, `isset()`, and `unset()` on objects referenced by global and class constants:

const SETTINGS = new stdClass();
SETTINGS->timeout = 30;
var_dump(SETTINGS->timeout); // int(30)

The constant itself is still immutable, so `SETTINGS` always points to the same object. Array writes through a constant are still an error.

## DocComments for Function Parameters

Doc comments can now sit directly on a parameter, and `ReflectionParameter::getDocComment()` returns them. This avoids repeating the parameter name in a `@param` tag above the function:

function search(
    /** Terms to search for in the database */
    string $query,
    /** Maximum number of entries to return */
    int $limit = 10,
): array {
    // ...
}

Static analyzers and IDEs can read the comment from the parameter itself.

## A Polling API

The new `Io\Poll` namespace gives PHP a unified interface to platform polling mechanisms: epoll on Linux, kqueue on BSD and macOS, event ports on Solaris, and WSAPoll on Windows. It replaces `stream_select()` for anyone building event loops or async runtimes in userland:

use Io\Poll\{Context, Event};
use Time\Duration;
$poll = new Context();
$server = stream_socket_server('tcp://0.0.0.0:8080');
stream_set_blocking($server, false);
$poll->add(new StreamPollHandle($server), [Event::Read], ['type' => 'server']);
while (true) {
    foreach ($poll->wait(Duration::fromSeconds(1)) as $watcher) {
        if ($watcher->hasTriggered(Event::Read)) {
            // accept the connection
        }
    }
}

The `wait()` timeout is a `Time\Duration`, and `StreamPollHandle` is a global class that wraps a stream resource. The RFC states the main motivation is internal, such as signal handling and FPM improvements, with userland async frameworks as the secondary audience.

## SortDirection Enum

The SortDirection enum provides a shared, type-safe way to express sort order with its Ascending and Descending cases. Previously, every library had to define its own way of representing sort direction, resulting in a mix of incompatible integers, strings, booleans, and custom enums. Since SortDirection is built into PHP, libraries can accept it in their public APIs without needing adapter code.

$query->orderBy('created_at', SortDirection::Descending);

## Enums Can Implement __debugInfo()

Enums were not allowed to define most magic methods when they shipped in 8.1. PHP 8.6 lifts that restriction for `__debugInfo()`, since it needs no state, so `var_dump()` output can be customized:

enum Status: int
{
    case Ok = 200;
    public function __debugInfo(): array
    {
        return [__CLASS__ . '::' . $this->name . ' = ' . $this->value];
    }
}

## Stream Error Handling

Streams get a unified error model. A new `error_mode` context option chooses between the current warnings, exceptions, or silence, and `stream_last_errors()` returns structured `StreamError` objects for the last operation:

$context = stream_context_create([
    'stream' => ['error_mode' => StreamErrorMode::Exception],
]);
try {
    $stream = fopen('/nonexistent/file.txt', 'r', false, $context);
} catch (StreamException $e) {
    foreach ($e->getErrors() as $error) {
        echo $error->code->name . ': ' . $error->message;
    }
}

## URI Extension Follow-ups

The URI extension that shipped in PHP 8.5 gains builder classes, so you can assemble a URI without creating intermediate objects for each component:

$uri = new Uri\Rfc3986\UriBuilder()
    ->setScheme('https')
    ->setHost('example.com')
    ->setPath('/foo/bar')
    ->build();

This also adds `getHostType()` to both URI classes, `getUriType()` to `Uri\Rfc3986\Uri`, and `isSpecialScheme()` to `Uri\WhatWg\Url`. A new `Uri\WhatWg\url_percent_encode()` function encodes a string for a specific URL component, chosen with the `UrlPercentEncodingMode` enum.

## JSON Errors Report the Line and Column

The messages from `json_last_error_msg()` and `JsonException` now include the line and column where parsing failed:

$json = '{
    "key1": "value1",
    "key2" "value2"
}';
json_validate($json);  // false
json_last_error_msg(); // "Syntax error near location 3:12"

Before 8.6, the message was just a "Syntax error".

## Function Arguments in Warnings

A new `error_include_args` INI setting adds the arguments a built-in function received to the warnings it raises. The RFC uses this example:

```
// Before
Warning: chmod(): Operation not permitted in /app/chmod.php on line 5
// With error_include_args=1
Warning: chmod('/', 511): Operation not permitted in /app/chmod.php on line 5
```
The setting is off by default. It uses the same formatting as exception stack traces, so arguments marked with `#[\SensitiveParameter]` are hidden and long strings are cut at the `zend.exception_string_param_max_len` limit.

## Check Property Access With Reflection

`ReflectionProperty::isPublic()` reports only the visibility modifier, which no longer tells you whether a property can be written after PHP 8.1's `readonly` and PHP 8.4's asymmetric visibility. The new `isReadable()` and `isWritable()` methods answer that for a given scope, where `null` means global scope:

class Post
{
    public function __construct(
        public private(set) string $title,
    ) {}
}
$property = new ReflectionProperty(Post::class, 'title');
$property->isReadable(null);       // true
$property->isWritable(null);       // false
$property->isWritable(Post::class); // true

Pass an object as the second argument to account for its state, such as a `readonly` property that is already initialized.

## Secure Session Defaults

Three `php.ini` session defaults change for new installations:

| Setting | Old default | New default | 
|---|---|---|
| `session.use_strict_mode` | `0` | `1` | 
| `session.cookie_httponly` | `0` | `1` | 
| `session.cookie_samesite` | unset | `Lax` | 

Laravel manages its own session cookies, so most apps are unaffected. Apps that use native PHP sessions and rely on the old defaults should check the session defaults RFC.

## Performance Improvements

Several common patterns are now compiled into faster code:

- `array_map()` with a first-class callable or partial function application callback, such as`array_map(strtolower(...), $names)` , compiles into the equivalent`foreach` loop, with no intermediate closure
- `printf()` calls that use only`%s` and`%d` compile into string interpolation
- Stateless closures (static, with no captured or static variables) are cached and reused instead of being created on every evaluation
- Writes to `private(set)` and`protected(set)` properties cost the same as writes to public properties after the first write at each call site, through direction-aware property resolution
- The JIT now works on thread-safe (ZTS) builds on Apple Silicon. FrankenPHP uses ZTS builds
- `json_encode()` is faster for arrays and objects, and for`JSON_PRETTY_PRINT` indentation

## Other Additions

- `grapheme_strrev()` reverses a string by grapheme cluster, so combining characters and emoji stay intact
- `pack()` and`unpack()` accept`<` and`>` endianness modifiers on signed integer and float format codes
- `mysqli::quote_string()` and`mysqli_quote_string()` escape a string and wrap it in single quotes, like`PDO::quote()`
- `ZipArchive::openString()` and`ZipArchive::closeString()` read and write zip archives in memory
- Stream sockets support TLS session resumption through the new `Openssl\Session` class, TLS 1.3 early data, and keepalive, linger, and buffer size context options
- Sodium adds the ML-KEM768 and X-Wing post-quantum key encapsulation functions when PHP is built against libsodium 1.0.22 or later
- Intl adds `IntlNumberRangeFormatter` ,`Locale::getDisplayKeyword()` , and`Locale::getDisplayKeywordValue()`
- `ini_get_all()` with`$details` set to`true` includes each directive's built-in default value
- The built-in development server accepts requests that use the HTTP `QUERY` method

## Backward-Incompatible Changes

The UPGRADING file has the full list. These are the changes most likely to affect application code:

- `trim()` ,`ltrim()` , and`rtrim()` now strip form feed (`\f` ) by default
- `fclose()` ,`file_put_contents()` , and`copy()` return`false` when flushing or closing the stream fails. These failures were ignored before
- `unpack()` reads`<` or`>` after a format code as an endianness modifier, so a format such as`s<value` now produces the key`value` instead of`<value`
- `session_encode()` returns an empty string instead of`false` for an empty session
- `preg_grep()` returns`false` on a PCRE error instead of a partial array
- Many functions now throw a `ValueError` for invalid arguments or strings containing NUL bytes, including`getenv()` ,`putenv()` ,`setlocale()` ,`parse_str()` ,`stat()` , and`file_exists()`
- A hard out-of-memory error now calls `abort()` , so the process exits with code 134 instead of 1

## Deprecations

The PHP 8.6 deprecations RFC and several separate RFCs deprecate the following:

- Returning from a `finally` block, which can silently discard an exception
- Returning a value from `__construct()` or`__destruct()` , or making either one a generator. A bare`return;` is still allowed (RFC)
- Using `is` or`let` as the name of a class, function, or constant, and`_` as a constant name. These names are reserved for future pattern matching and block scoping proposals
- Using `namespace` as a class constant name, naming a function`readonly` , and passing a third argument to`define()`
- `is_double()` ,`is_integer()` ,`is_long()` , and`doubleval()` , in favor of`is_float()` ,`is_int()` , and`floatval()`
- `spl_object_hash()` in favor of`spl_object_id()` . Laravel 14 already replaced the framework's remaining calls
- `strcoll()` and the`SORT_LOCALE_STRING` flag, in favor of the`Collator` class
- `spl_classes()` ,`metaphone()` ,`mysqli_get_charset()` , and`mysqli_stmt_init()`
- The flag, sorting, and serialization methods `ArrayIterator` inherits from`ArrayObject` , and the CSV methods on`SplFileObject`
- Passing objects to `array_walk()` ,`http_build_query()` , and`mb_convert_variables()`
- Passing a string to `is_a()` or`is_subclass_of()` when`$allow_string` is`false`
- Session handlers that do not implement `create_sid()` and`validateId()`
- More than 16 filters in a `php://filter` URL, unless you set the new`filter.max_filter_count` context option
- The `mb_ereg*` functions and`mb_split()` , because upstream maintenance of the Oniguruma library ended (RFC)

A proposal to deprecate the `list()` construct did not pass, so `list()` is not deprecated.

## Learn More

- UPGRADING with every backward-incompatible change, new feature, and deprecation
- NEWS with the changes in each pre-release
- PHP 8.6 preparation tasks and timeline
- PHP RFC index with the full list of RFCs implemented in 8.6
- PHP 8.6 deprecations
- More PHP 8.6 coverage
