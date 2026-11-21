---
id: collect-261121-veille-ia/python-php/what-is-new-in-php-8-6-with-code-you-can-run
title: "What is new in PHP 8.6, with code you can run"
domain: python-php
role: reference
task: article
actors: []
dates: []
keywords: ["class", "constant", "const", "clamp", "time", "code", "argument", "readonly"]
source: https://sdp.im/blog/what-is-new-in-php-86-with-code-you-can-run
source_anchor: ""
source_lines: [1, 136]
section: "Python_PHP"
---
## Where it stands

PHP 8.6.0 is at RC4 today, 31 October 2026. The release plan has RC5 on 5 November and the general availability release on 19 November. Everything below comes from the UPGRADING file on the PHP-8.6 branch, and every code sample was run on the official 8.6.0RC4 CLI image. Where a feature is new I also ran it on 8.5.12 to confirm that it fails there. An RC can still change, so check the final notes before you ship.

## Partial function application

This is the headline feature. Put `?` where an argument is missing and you get a closure for the rest. Put `...` to keep every remaining argument open.

```
function tax(float $rate, float $price): float {
    return round($price * (1 + $rate), 2);
}
$ppn = tax(0.11, ?);
echo $ppn(100000);                       // 111000
$rest = str_replace('a', 'b', ...);
echo $rest('banana');                    // bbnbnb
echo implode(',', array_map(tax(0.11, ?), [100, 200]));   // 111,222
$named = tax(price: 100, rate: ?);       // named arguments work too
```
It works on methods and static methods as well, and the result is a normal `Closure`. A partial call that leaves two placeholders open wants both arguments when you call it, and it throws an `ArgumentCountError` if you pass one. You cannot partially apply `new`: `new Price(?, 'USD')` is a fatal error ("Cannot create Closure for new expression"), so use a static factory.

It replaces most of the short arrow functions you write only to bind one argument. PHP also compiles `array_map()` with a first class callable or a partial application into the equivalent loop.

## readonly properties can have defaults

A readonly property used to need a constructor assignment. Now it can declare a default value.

```
final class Job {
    public readonly array $tags = ['api'];
    public function __construct(public readonly string $queue = 'default') {}
}
$job = new Job(queue: 'mail');
$job->tags = [];   // Error: Cannot modify readonly property Job::$tags
```
## Mutating an object held in a constant

A constant can hold an object created with `new`, and PHP 8.6 lets you write to its properties.

```
final class Options { public bool $debug = false; public int $retries = 1; }
const DEFAULTS = new Options();
DEFAULTS->retries = 5;
echo DEFAULTS->retries;                  // 5
class Config { const OPTS = DEFAULTS; }
Config::OPTS->debug = true;
```
On 8.5 the same code is a fatal error. Two things to know. The `new` itself is only allowed on a global `const`; a class constant cannot call `new`, but it can point at a global one. And this is shared mutable state: the constant is no longer constant in spirit, so treat it as configuration you set once at boot, not as a place for request data.

## #[\Override] on constants and enum cases

`#[\Override]` already protects methods from typos. It now covers class constants and enum cases.

```
interface HasQueue { const QUEUE = 'default'; }
class Mail implements HasQueue { #[\Override] const QUEUE = 'mail'; }   // fine
class Typo implements HasQueue { #[\Override] const QUEUEE = 'x'; }
// Fatal error: Typo::QUEUEE has #[\Override] attribute,
//              but no matching parent constant exists
```
## clamp()

A small function PHP should always have had.

```
clamp(15, 0, 10);      // 10
clamp(-3, 0, 10);      // 0
clamp(7, 0, 10);       // 7
clamp(1.5, 0.0, 1.0);  // 1.0
clamp(1, 10, 0);       // ValueError: argument #2 ($min) must be smaller than or equal to argument #3 ($max)
```
## Time\Duration

A new immutable value class for lengths of time, with nanosecond precision. The constructor is private, so you build one with a named factory.

```
$d   = Time\Duration::fromMinutes(90);
$sum = $d->add(Time\Duration::fromSeconds(30))->multiplyBy(2);
$sum->seconds;                                              // 10860
Time\Duration::compare($d, $sum);                           // -1
Time\Duration::fromIso8601DurationString('PT1H30M') == $d;  // true
```
The factories are `fromSeconds`, `fromMilliseconds`, `fromMicroseconds`, `fromNanoseconds`, `fromMinutes`, `fromHours` and `fromIso8601DurationString`. The operations are `add`, `sub`, `multiplyBy`, `divideBy`, `negate`, `absolute` and `compare`. The new poll API takes a `Time\Duration` as its timeout.

## Smaller language changes

Enums can define `__debugInfo()`, so `var_dump()` shows what you choose. Reflection gains `ReflectionProperty::isReadable()` and `isWritable()`, which take a scope and understand `private(set)` and `readonly`. `ReflectionParameter::getDocComment()` reads a comment placed before a parameter. `ReflectionAttribute` gets `getShortName()`, `getNamespaceName()` and `inNamespace()`. `json_last_error_msg()` and `JsonException` now say where the error is:

```
json_decode('{"items": [1, 2,, 3]}');
echo json_last_error_msg();   // 8.5: "Syntax error"   8.6: "Syntax error near location 1:17"
```
## Standard library and extensions

The URI extension gets builders, so you stop concatenating strings:

```
$uri = (new Uri\Rfc3986\UriBuilder())
    ->setScheme('https')->setHost('api.example.com')->setPort(8443)
    ->setPath('/v1/entries')->setQuery('type=post&limit=10')
    ->build(null);
echo $uri->toString();   // https://api.example.com:8443/v1/entries?type=post&limit=10
```
A new polling API (`Io\Poll\Context`) waits on many streams with the best backend the machine has, `epoll` here:

```
[$a, $b] = stream_socket_pair(STREAM_PF_UNIX, STREAM_SOCK_STREAM, STREAM_IPPROTO_IP);
$ctx = new Io\Poll\Context();
$ctx->add(new StreamPollHandle($a), [Io\Poll\Event::Read], data: 'conn-a');
fwrite($b, 'ping');
foreach ($ctx->wait(Time\Duration::fromMilliseconds(100)) as $watcher) {
    echo $watcher->getData(), ' ', fread($watcher->getHandle()->getStream(), 4);   // conn-a ping
}
```
Other additions in the notes, which I did not run: a stream errors API, TLS session resumption and 0-RTT, `pack()` and `unpack()` endianness modifiers, a `SortDirection` enum, libsodium bindings for ML-KEM768 and the X-Wing key exchange (libsodium 1.0.22 or later), `mysqli::quote_string()`, and `grapheme_strrev()` in intl.

## What can break an upgrade

Read this part before you bump a version.

1. **Session defaults changed.**`session.use_strict_mode` is now 1,`session.cookie_httponly` is 1, and`session.cookie_samesite` is`Lax` . On 8.6 I read back`1` ,`1` ,`Lax` . An app that reads its session cookie from JavaScript, or posts cross site with the session cookie, must set these explicitly.
2. **New deprecations** that show up at`E_ALL` . I ran the ones for return in`finally` , constructor return values,`spl_object_hash()` ,`is_long()` ,`doubleval()` and`is_a()` , and took the rest from the notes: return in a`finally` block, returning a value from`__construct()` or`__destruct()` ,`spl_object_hash()` (use`spl_object_id()` ),`is_long()` ,`is_integer()` and`is_double()` (use`is_int()` and`is_float()` ),`doubleval()` (use`floatval()` ),`is_a()` with a string when`$allow_string` is false,`metaphone()` , and`strcoll()` . Naming a function`readonly` , or a class, function or constant`let` or`is` , is deprecated too.
3. **`trim()` now strips form feed** (`\f` ) by default.
4. **Stricter errors.** Many functions now throw`ValueError` on NUL bytes or out of range values, and`fclose()` ,`file_put_contents()` and`copy()` now return`false` when the final flush fails.
5. **mbregex is deprecated** because the Oniguruma library is no longer maintained.

The fastest check is to run your test suite on the RC image with errors turned up:

`docker run --rm -v "$PWD":/app -w /app php:8.6-rc-cli php -d error_reporting=-1 vendor/bin/phpunit`
## Performance

The notes list several engine and library gains: `printf()` with only `%s` and `%d` is compiled to string interpolation, a stateless closure cache, a faster tail call VM, faster `json_encode()`, and quicker `array_map()`, `str_split()`, `str_pad()` and `array_sum()`. I did not benchmark any of it, so I will not give numbers. Measure on your own code.

## Should you move

Not on the day of the release. Run the suite on the RC now, fix the deprecations, and plan the move after the first patch release. Partial application and `clamp()` are the two things that I expect to change how people write everyday code.
