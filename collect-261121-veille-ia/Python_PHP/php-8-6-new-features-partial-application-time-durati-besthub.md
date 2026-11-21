---
id: collect-261121-veille-ia/python-php/php-8-6-new-features-partial-application-time-durati-besthub
title: "PHP 8.6 New Features: Partial Application, Time\Durati… | BestHub"
domain: python-php
role: reference
task: article
actors: []
dates: ["2026-11-19"]
keywords: ["duration", "clamp", "partial", "time", "function", "application", "readonly", "admin"]
source: https://www.besthub.dev/articles/php-8-6-new-features-partial-application-time-duration-clamp-and-more-84497c4ac977
source_anchor: ""
source_lines: [1, 281]
section: "Python_PHP"
---
## Timeline

PHP 8.6 is scheduled for release on November 19, 2026, with RC5 on November 5, 2026. The feature freeze ended in September, so the feature set is final.

## 1. Partial Function Application

The headline feature is partial function application. You write a function call with `?` placeholders for missing arguments, and PHP returns a closure that accepts those arguments.

```
$startsWithAdmin = str_starts_with(?, 'admin.');
array_filter(['admin.users', 'web.home', 'admin.posts'], $startsWithAdmin);
// ['admin.users', 'admin.posts']
```
This replaces verbose arrow functions like `fn ($value) => something($value, 10)`. Three placeholders are supported: `?` replaces exactly one argument. `...` replaces all remaining arguments. `?name` (named placeholder) replaces only that named parameter, preserving defaults.

```
function greet(string $greeting, string $name, string $punctuation = '!') {
    return "$greeting, $name$punctuation";
}
$hi = greet('Hi', ...);
$hi('Nuno'); // "Hi, Nuno!"
$hi('Taylor', '?'); // "Hi, Taylor?"
$hey = greet(name: 'Nuno', greeting: ?);
$hey('Hey'); // "Hey, Nuno!"
Str::limit(?, end: '…', limit: 5); // named arguments in any order
```
The returned closure is a real function with proper types, visible via reflection. Bound variables retain original types, defaults, and names. A `?` placeholder always makes the parameter required, even if the original had a default; `...` preserves defaults. PHP throws compile-time errors for duplicate named placeholders or unknown/variadic parameters.

## Interaction with Pipe Operator

PHP 8.5 introduced the pipe operator `|>`, but each step required a single-argument callable. Partial application solves this:

```
$slug = 'Hello World'
    |> strtolower(...)
    |> str_replace(' ', '-', ?);
// "hello-world"
$title = ' Hello PHP'
    |> trim(...)
    |> Str::limit(?, 5)
    |> strtoupper(...);
// "HELLO..."
```
When a partial with a single placeholder sits on the right side of a pipe, OPcache compiles it to a direct function call — no closure allocation.

## 2. Time\Duration

PHP finally gets a first-class time duration type. `Time\Duration` is a `final readonly` class with three properties: `$seconds` (int), `$nanoseconds` (int), `$negative` (bool). All methods return new instances.

```
use Time\Duration;
$timeout = Duration::fromSeconds(90)->add(Duration::fromMilliseconds(500));
$timeout->seconds; // 90
$timeout->nanoseconds; // 500000000
$meeting = Duration::fromIso8601DurationString('PT1H30M');
$meeting->seconds; // 5400
Duration::compare($meeting, Duration::fromMinutes(90)); // 0
Duration::fromHours(2)->divideBy(3)->seconds; // 2400
```
Factory methods: `fromHours()`, `fromMinutes()`, `fromSeconds()`, `fromMilliseconds()`, `fromMicroseconds()`, `fromNanoseconds()`, `fromIso8601DurationString()`. Operations: `add()`, `sub()`, `multiplyBy()`, `divideBy()`, `negate()`, `absolute()`, `compare()`. Intended to replace ad-hoc `int $seconds` or `int $timeoutInMs` parameters for cache TTL, HTTP timeouts, queue delays, lock durations.

## 3. clamp()

Clamps a value between `$min` and `$max`: `clamp($value, $min, $max)`. Works with any comparable type, including strings.

```
clamp(150, 1, 100); // 100
clamp(-5, 1, 100); // 1
clamp(42, 1, 100); // 42
clamp(1.5, 0.0, 1.0); // 1.0
clamp('m', 'a', 'f'); // "f"
```
Classic use case: `$perPage = clamp((int)($_GET['per_page'] ?? 15), 1, 100);`. If `$min > $max`, a `ValueError` is thrown.

## 4. Readonly Properties with Default Values

Readonly properties can now declare default values directly:

```
final class Settings {
    public readonly int $perPage = 15;
    public readonly string $theme = 'dark';
}
$settings = new Settings();
$settings->perPage; // 15
$settings->perPage = 30; // Error: Cannot modify readonly property
```
Previously defaults had to be set in the constructor. Useful for DTOs, messages, value objects with fixed configuration.

## 5. Writing to Objects Stored in Constants

Constants holding objects can now have their properties mutated:

```
final class Config {
    public array $items = [];
    public int $hits = 0;
}
const CONFIG = new Config();
CONFIG->items['app.name'] = 'Acme';
CONFIG->hits++;
// CONFIG still references the same object; only its state changes.
// Enum properties remain immutable.
```
## 6. #[\Override] for Constants and Enums

Extends the `#[\Override]` attribute (introduced for methods in 8.3, properties in 8.5) to class constants and enum cases. Ensures a parent constant exists; compilation fails if the parent constant is renamed or removed.

```
interface HasTable {
    const TABLE = 'models';
}
final class User implements HasTable {
    #[\Override] const TABLE = 'users';
}
```
Error:

`Fatal error: Post::TABLE has #[\Override] attribute, but no matching parent constant exists`
.

## 7. __debugInfo() for Enums

Enums can now control output of `var_dump()`, `dump()`, `dd()` via `__debugInfo()`.

```
enum Status: string {
    case Active = 'active';
    case Banned = 'banned';
    public function __debugInfo(): array {
        return ['label' => ucfirst($this->value)];
    }
}
var_dump(Status::Active);
// enum(Status::Active) (1) { ["label"]=> string(6) "Active" }
```
## 8. JSON Error Location

`json_decode()`
errors now include the byte location in the input string.

```
json_decode('{"name": "Nuno", "age": }');
json_last_error_msg();
// "Syntax error near location 1:25"
```
`JsonException`
thrown with `JSON_THROW_ON_ERROR` includes the same line and column.

## 9. Stream Errors as Exceptions

Stream contexts accept an `error_mode` option with three modes via `StreamErrorMode` enum: `StreamErrorMode::Error` (default): emit warnings, return false. `StreamErrorMode::Exception`: throw `StreamException`. `StreamErrorMode::Silent`: suppress errors, use `stream_last_errors()` / `stream_clear_errors()`.

```
$context = stream_context_create(['stream' => ['error_mode' => StreamErrorMode::Exception]]);
try {
    file_get_contents('/does/not/exist.txt', context: $context);
} catch (StreamException $e) {
    $e->getMessage(); // "Failed to open stream: No such file or directory"
}
```
Also affects `fclose()`, `file_put_contents()` (flush/close failures), `copy()` (returns false on write failure, e.g., disk full). `StreamErrorCode` enum avoids string comparison.

## 10. True Polling API: Io\Poll

Foundational async I/O API using `epoll` (Linux) and `kqueue` (macOS) via a unified object API, no extensions required. Targeted at event-loop authors (ReactPHP, Amp, Revolt) who currently fall back to `stream_select()` or need extensions.

```
use Io\Poll\Context;
use Io\Poll\Event;
use Time\Duration;
[$a, $b] = stream_socket_pair(STREAM_PF_UNIX, STREAM_SOCK_STREAM, STREAM_IPPROTO_IP);
$poll = new Context();
$poll->add(new StreamPollHandle($b), [Event::Read], data: 'worker-1');
fwrite($a, 'ping');
foreach ($poll->wait(Duration::fromSeconds(1)) as $watcher) {
    fread($b, 4); // "ping"
    $watcher->getData(); // "worker-1"
}
```
Backends: `Poll\Auto`, `Poll\Epoll`, `Poll\Kqueue`, `Poll\EventPorts`, `Poll\WSAPoll`.

## 11. SortDirection Enum

Global enum with two cases: `Ascending`, `Descending`. Prevents packages from inventing their own `'asc'` / `'desc'` strings.

```
enum SortDirection {
    case Ascending;
    case Descending;
}
function orderBy(string $column, SortDirection $direction = SortDirection::Ascending): string {
    return $column . ' ' . match ($direction) {
        SortDirection::Ascending => 'asc',
        SortDirection::Descending => 'desc',
    };
}
orderBy('created_at', SortDirection::Descending); // "created_at desc"
```
## 12. URI Builder

Complements PHP 8.5's URI parsing extension. Two builders: `Uri\Rfc3986\UriBuilder` and `Uri\WhatWg\UrlBuilder` (browser rules). Also `getUriType()`, `getHostType()` (IPv4/IPv6/domain), `Uri\WhatWg\url_percent_encode()`.

```
$uri = (new Uri\Rfc3986\UriBuilder())
    ->setScheme('https')
    ->setHost('nunomaduro.com')
    ->setPath('/posts')
    ->setQuery('page=2')
    ->build();
$uri->toString(); // "https://nunomaduro.com/posts?page=2"
```
## 13. Performance Improvements (No Code Changes)

`array_map()`
with first-class callables or partials (e.g., `array_map(strtoupper(...), $names)`) compiles to a `foreach` loop — no closure allocation, no user-space callback. `printf()` with only `%s` and `%d` compiles to string interpolation.

Stateless closures ( `static fn` capturing nothing) are cached and reused. `var_dump(make() === make()); // bool(true)` where `make()` returns `static fn () => 1`.

Faster argument passing to known constructors (e.g., `new self()`). `json_encode()` faster on arrays/objects and with `JSON_PRETTY_PRINT`.

Optimized `array_sum()`, `array_product()` for integer arrays; `array_unshift()`, `array_walk()` for single-element arrays; `array_intersect()`, `str_pad()`, `str_split()`, `mb_str_pad()`, `pathinfo()`.

Faster deep recursion when VM allocates new stack pages.

JIT now handles ZTS builds on Apple Silicon; ZTS builds faster (used by FrankenPHP).

RFC measured ~3% improvement on a real-world template, but includes an unmerged optimization — treat as directional, not a guarantee.

## 14. Minor Additions

`trim()`
, `ltrim()`, `rtrim()` now remove form feed `"\f"` by default. `ARRAY_FILTER_USE_VALUE` constant for explicit default mode of `array_filter()`. `WeakMap` supports reference retrieval: `$ref = &$map[$object]` works even if key absent. `ReflectionProperty::isReadable()` and `isWritable()` for asymmetric visibility and hooks. `ReflectionParameter::getDocComment()` reads parameter doc comments. `ReflectionAttribute::getShortName()`, `getNamespaceName()`, `inNamespace()`. `mysqli::quote_string()` quotes and escapes in one call. `grapheme_strrev()` reverses by grapheme clusters (keeps emoji intact); requires `intl`. `ini_get_all()` includes `builtin_default_value` per directive.

Built-in server ( `php -S`) accepts HTTP QUERY method, no longer reflects `Host` header. `error_include_args` INI option (off by default) includes function arguments in error messages.

OpenSSL: TLS session resumption, external PSK, TLS 1.3 early data for streams.

Sodium: post-quantum ML-KEM768, X-Wing KEM (libsodium 1.0.22), SHAKE, TurboSHAKE, IP address encryption.

Curl: `CURLOPT_SEEKFUNCTION` for rewinding stream uploads on redirect.

Intl: `IntlNumberRangeFormatter` for localized ranges like "€3 – €5".

Unicode 18.0 in `mbstring`.

## 15. Breaking Changes

Session defaults are now secure: `session.cookie_httponly=true`, `session.cookie_samesite="Lax"`, `session.use_strict_mode=true`. May break JS access to session cookies or cross-site requests. `ValueError` thrown where warnings previously occurred: `file_exists()`, `is_file()`, `filesize()`, `stat()` with NUL bytes; `sleep()` with invalid patterns; `array_filter()`, `pathinfo()`, `scandir()`, `preg_grep()` return `false` on error (e.g., invalid UTF-8) instead of partial results. `trim()` now removes `"\f"`; use `trim($value, " \n\r\t\v\0")` to preserve form feed. `SplFileObject` changes: `next()`, `fgets()`, `current()` share current line — test line-by-line reading. `base_convert()`, `bindec()`, `hexdec()`, `octdec()` emit deprecation notices when precision loss occurs. `session_encode()` returns empty string for empty session instead of `false`.

Out-of-memory aborts with exit code 134 (was 1).

## 16. Deprecations

E_DEPRECATED notices (code runs but warns for PHP 9):

Returning a value from `__construct()` / `__destruct()` or generator constructors/destructors. `let` and `is` reserved as future keywords (cannot name classes, interfaces, traits, enums, functions, constants). `readonly` allowed as function name, namespace, class constant, constant. `is_a()` / `is_subclass_of()` with string second argument. `?array` return type for `__debugInfo()` — use `array`.

Functions: `is_integer()` → `is_int()`; `is_long()`, `is_double()` → `is_float()`; `doubleval()` → `floatval()`; `spl_object_hash()` → `spl_object_id()`. `mysqli_get_charset()`, `mysqli_stmt_init()`. `php://filter` URLs with >16 filters. `ReflectionProperty::setValue()` with wrong-class object; `ReflectionMethod::invoke()` with object for static method.

Session handler `create_sid()` without `validateId()`.

## Trying PHP 8.6 Now

Compile from the PHP-8.6 branch:

```
git clone --depth 1 --branch PHP-8.6 https://github.com/php/php-src.git
cd php-src
./buildconf --force
./configure --disable-all --enable-cli --enable-mbstring --disable-mbregex --enable-tokenizer --enable-ctype --enable-filter
make -j8
sapi/cli/php -r 'var_dump(clamp(150, 1,100));'
```
Signed-in readers can open the original source through BestHub's protected redirect.

This article has been distilled and summarized from source material, then republished for learning and reference. If you believe it infringes your rights, please contactand we will review it promptly.

### 0 Comments

Thoughtful readers leave field notes, pushback, and hard-won operational detail here.
