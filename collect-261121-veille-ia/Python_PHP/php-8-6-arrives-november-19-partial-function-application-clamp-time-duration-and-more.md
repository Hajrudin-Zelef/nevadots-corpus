---
id: collect-261121-veille-ia/python-php/php-8-6-arrives-november-19-partial-function-application-clamp-time-duration-and-more
title: "PHP 8.6 Arrives November 19: Partial Function Application, clamp(), Time\Duration and More"
domain: python-php
role: reference
task: article
actors: []
dates: []
keywords: ["function", "clamp", "duration", "november", "partial", "time", "application", "parameter"]
source: https://pbxscience.com/php-8-6-arrives-november-19-partial-function-application-clamp-timeduration-and-more/
source_anchor: ""
source_lines: [1, 134]
section: "Python_PHP"
---
## PHP 8.6 Arrives November 19: Partial Function Application, clamp(), Time\Duration and More

PHP 8.6 is scheduled for general availability on November 19, 2026, and the feature set is locked. The hard feature freeze took effect on September 22, release candidates have been circulating since late September, and the final release candidate is due on November 5. The headline addition is partial function application, which reshapes how developers write callbacks. Alongside it come a long-awaited `clamp()` function, a native duration type, a new polling API, safer session defaults, and a set of deprecations to address ahead of PHP 9.

### Release timeline

| Milestone | Date | 
|---|---|
| Alpha 1–3 | July 2–30, 2026 | 
| Beta 1 (soft feature freeze) | August 13, 2026 | 
| Feature freeze | September 22, 2026 | 
| RC 1 | September 24, 2026 | 
| RC 4 | November 5, 2026 | 
| PHP 8.6.0 GA | November 19, 2026 | 

The release managers are Daniel Scherzer, Matteo Beccati and Joe Ferguson. Dates follow the PHP 8.6 preparation page on the PHP wiki.

### Partial function application

Partial function application (PFA) lets you call a function with only some of its arguments and get back a closure that accepts the rest. The `?` placeholder marks a single argument to fill in later, and `...` stands in for all remaining arguments. The proposal had been declined in 2021; the revised RFC passed 33 to 0.

```
$makeSlug = str_replace(' ', '-', ?);
$makeSlug('Hello World'); // Hello-World
$titles = array_map(strtolower(?), $titles);
```
The resulting closure keeps the parameter names, types and defaults of the original function, which matters for tools that rely on reflection, such as dependency injection containers and routers. Arguments you supply are evaluated when the partial is created, not when it is called. A follow-up RFC settled that every `?` placeholder becomes a required parameter on the closure, even when the original parameter was optional.

It pairs naturally with the pipe operator introduced in PHP 8.5, removing the need for throwaway arrow functions in each step:

`$slug = 'Hello World' |> strtolower(...) |> str_replace(' ', '-', ?);`
### The clamp() function

The new `clamp()` returns the value if it falls within the bounds, or the nearest bound if it does not, replacing the easily reversed `min(max($value, $min), $max)` pattern. It works with any comparable type, including strings and `DateTime` objects, and passing a `$min` greater than `$max` throws a `ValueError`.

```
clamp(10, min: 0, max: 100);  // 10
clamp(101, min: 0, max: 100); // 100
clamp(-1, min: 0, max: 100);  // 0
```
### Time\Duration

PHP gains a dedicated type for lengths of time. `Time\Duration` is a final readonly class with nanosecond precision, factory methods for each unit, arithmetic methods, ISO 8601 parsing and comparison support. It is intended as a shared type that core functions and the new polling API can accept instead of loose integers and floats.

```
use Time\Duration;
$oneSecond  = Duration::fromSeconds(1);
$halfSecond = $oneSecond->divideBy(2);
$total      = $oneSecond->add($halfSecond);
$delay = Duration::fromMilliseconds(100)->multiplyBy(2 ** $attempt);
```
### Readonly property defaults

Readonly properties can now declare a default value. Previously this was a compile-time error. The change only lifts that restriction; the property still cannot be reassigned after initialization.

```
final readonly class CreateBooksTable implements Migration
{
    public string $name = '2026_01_01_create_books_table';
}
```
### Doc comments on parameters

Doc comments can now be attached directly to a function parameter, and `ReflectionParameter::getDocComment()` returns them. IDEs and static analyzers can read the description from the parameter itself instead of a separate `@param` tag.

### A unified polling API: Io\Poll

The new `Io\Poll` namespace offers a single interface to platform polling mechanisms: epoll on Linux, kqueue on BSD and macOS, event ports on Solaris, and WSAPoll on Windows. It targets authors of event loops and async runtimes as an alternative to `stream_select()`. According to the RFC, the main motivation is internal use such as signal handling and FPM improvements, with userland async frameworks as the secondary audience.

### SortDirection enum

A global `SortDirection` enum with `Ascending` and `Descending` cases is now built in. Nothing in core accepts it yet; it exists as a shared type so libraries, query builders in particular, stop defining their own.

`$query->orderBy('created_at', SortDirection::Descending);`
### Enums can implement __debugInfo()

Enums were restricted from defining most magic methods when they shipped in PHP 8.1. PHP 8.6 lifts that restriction for `__debugInfo()`, so `var_dump()` output for enum cases can be customized.

### Unified stream error handling

Streams get a consistent error model. A new `error_mode` context option chooses between warnings (the current default), exceptions or silence, and `stream_last_errors()` returns structured error objects for the last operation. The related RFC defines more than 50 semantic error codes in a `StreamErrorCode` enum.

```
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
```
### URI extension follow-ups

The URI extension that arrived in PHP 8.5 gains builder classes for assembling a URI without creating intermediate objects for each component. The follow-up RFC also adds `getUriType()` and `getHostType()` methods, plus percent-encoding functions for individual URI components.

```
$uri = new Uri\Rfc3986\UriBuilder()
    ->setScheme('https')
    ->setHost('example.com')
    ->setPath('/foo/bar')
    ->build();
```
### Other changes reported during the beta cycle

- New endianness modifiers for `pack()` and`unpack()` .
- An overhaul of the SNMP extension and additional session hardening, both landing in Beta 2.
- A stateless closure cache, noted in the Beta 1 change record.

### What may break: secure session defaults

Three `php.ini` session defaults change for new installations. Applications that use native PHP sessions and rely on the old values should review their configuration; frameworks that manage their own session cookies, such as Laravel, are largely unaffected.

| Setting | Old default | New default | 
|---|---|---|
| `session.use_strict_mode` | 0 | 1 | 
| `session.cookie_httponly` | 0 | 1 | 
| `session.cookie_samesite` | unset | Lax | 

### Deprecations to plan for

Deprecations do not stop code from running, but they raise `E_DEPRECATED` notices that signal changes needed before PHP 9. Reported items include:

- Using `return` inside a`finally` block.
- `spl_object_hash()` , replaced by`spl_object_id()` . Drupal core has already logged new warnings from this change during compatibility testing.
- The SplFileObject CSV methods.

The full list is maintained in the PHP 8.6 deprecations RFC.

### Should you upgrade now?

PHP 8.6 betas and release candidates are for testing only, not production. Teams can build the pre-release from the official PHP pre-release pages or the `PHP-8.6` branch of php-src and run their test suites to catch deprecation warnings and session behavior changes early. Ecosystem support is following: Drupal core, for example, already runs tests against PHP 8.6 on its development branch but does not yet list it as a supported production version. Wait for the November 19 general availability release before moving live workloads.
