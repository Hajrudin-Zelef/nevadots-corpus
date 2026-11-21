---
id: collect-261121-veille-ia/python-php/php-8-6-partial-function-application-hands-on-guide-online-learning-portal
title: "PHP 8.6 Partial Function Application: Hands-On Guide - Online Learning Portal"
domain: python-php
role: reference
task: article
actors: []
dates: ["2026-11-19"]
keywords: ["function", "partial", "arguments", "string", "application", "array", "guide", "placeholders"]
source: https://onlinelearningportal.website/php-8-6-partial-function-application/
source_anchor: ""
source_lines: [1, 137]
section: "Python_PHP"
---
PHP 8.6.0 is due for general availability on **19 November 2026**. Because the php2shell security fix changed the 8.6 ABI, the release managers added an extra release candidate, **RC6**, this week (if you compile extensions against 8.6 test builds, rebuild them against RC6). With the final release days away, now is a good time to learn the biggest language feature in PHP 8.6: **partial function application** (PFA). This hands-on guide explains the syntax, the rules and the patterns you will actually use.

## What partial function application does

Partial application means calling a function with only some of its arguments and getting back a new function that waits for the rest. In PHP 8.6 you do this by replacing arguments with placeholders:

- `?` stands for**exactly one** missing argument.
- `...` stands for**all remaining** arguments (zero or more).

When the engine sees a placeholder, it does not call the function. It returns a `Closure` that remembers the values you already supplied. Here is the example from the official PHP 8.6 migration guide:

```
<?php
function add(int $a, int $b, int $c = 0): int
{
    return $a + $b + $c;
}
// "?" replaces exactly one argument
$addOne = add(1, ?);
var_dump($addOne(5));        // int(6)
// "..." replaces all remaining arguments
$addToOne = add(1, ...);
var_dump($addToOne(2, 3));   // int(6)
// Works with internal functions too
$stripSpaces = str_replace(' ', '', ?);
var_dump($stripSpaces('a b c')); // string(3) "abc"
```
If `add(...)` looks familiar, that is because first-class callable syntax from PHP 8.1 is simply the “everything is a placeholder” case of PFA.

## Why it matters: cleaner callbacks

Before 8.6, adapting a function for `array_map()` or `array_filter()` meant writing an arrow function that repeats the call:

```
// PHP 8.5 and earlier
$clean = array_map(fn(string $s): string => str_replace('hello', 'hi', $s), $lines);
// PHP 8.6
$clean = array_map(str_replace('hello', 'hi', ?), $lines);
```
The PFA version is shorter, and the resulting closure keeps the **parameter names, types, defaults and return type** of the underlying function, so reflection and static analysis still see full type information.

### A filter you can read at a glance

```
$routes = ['admin.users', 'web.home', 'admin.settings'];
$isAdmin = str_starts_with(?, 'admin.');
print_r(array_values(array_filter($routes, $isAdmin)));
// ['admin.users', 'admin.settings']
```
### PFA with the pipe operator

PHP 8.5 introduced the pipe operator `|>`, which passes a value into a single-argument callable. PFA is the missing piece that makes pipelines practical:

```
$slug = '  Hello World From PHP  '
    |> trim(...)
    |> strtolower(...)
    |> str_replace(' ', '-', ?);
echo $slug; // hello-world-from-php
```
## The rules you need to know

### Every required parameter must be covered

A partial must account for all required parameters, either with a value, a `?` or a trailing `...`. Otherwise PHP throws an `ArgumentCountError` when the partial is created:

```
function stuff(int $i, string $s, float $f, int $m = 0) {}
$ok  = stuff(1, ?, ?);      // fine: $s and $f are placeholders, $m keeps its default
$ok2 = stuff(1, ...);       // fine: "the rest"
$bad = stuff(?);            // ArgumentCountError: expects at least 3 arguments
```
Note that an optional parameter you do not mention (like `$m` in the first line) will always receive its default value.

### Named arguments and placeholders

You can fill parameters by name, and named placeholders appear in the closure *in the order you list them*:

```
function connect(string $host, string $user, string $password, int $port = 5432) {}
$connectLocal = connect(host: '127.0.0.1', user: ?, password: ?);
$connectLocal('app', 'secret');
```
Positional placeholders cannot come after a named argument; that is a compile-time error. Keep positional values first, then named ones, then an optional `...`.

### Values are captured when the partial is created

Arguments you provide are evaluated once, at the moment the closure is created, not each time it is called. This matters if an argument has side effects or reads changing state:

```
$stamp = sprintf('[%s] %s', date('H:i:s'), ?);
sleep(2);
echo $stamp('done'); // shows the time when $stamp was created
```
If you need a fresh value on every call, use a regular closure.

### Methods, static methods and invokable objects

```
class Mailer
{
    public static function make(string $dsn, int $timeout): self { return new self(); }
    public function send(string $to, string $subject, string $body): bool { return true; }
}
$makeMailer = Mailer::make(?, 10);               // static partial
$mailer     = $makeMailer('smtp://localhost');
$notifyAdmin = $mailer->send('[email protected]', ?, ?);
$notifyAdmin('Backup finished', 'All good.');
```
One exception: you cannot partially apply a constructor. `new Mailer(?)` is a compile error (“Cannot create Closure for new expression”). Use a static factory method instead, as above.

### Thunks: delay a call entirely

If you supply every argument and add `...`, you get a zero-argument closure that runs the call later:

```
// warmProductCache() is your own function
$warmCache = warmProductCache('products', 500, ...);
register_shutdown_function($warmCache);
```
## Other PHP 8.6 features to try alongside PFA

- **Default values for readonly properties:**`public readonly int $timeout = 30;`
- **`#[\Override]` on class constants** and enum cases.
- **`Time\Duration`** : an immutable, nanosecond-precision duration type, e.g.`\Time\Duration::fromSeconds(2)->add(\Time\Duration::fromMilliseconds(500))` .
- **Endianness modifiers in `pack()`/`unpack()`** , such as`pack('l>', 1)` .

## How to test PFA today

1. Install an 8.6 release candidate in a container or a separate PHP build. Do **not** use RCs in production.
2. Run your test suite and static analysis on 8.6. Tool support is arriving fast; for example, PhpStorm 2026.3 added PHP 8.6 support.
3. Refactor a few noisy arrow-function callbacks into PFA and compare readability in code review.
4. Plan the production upgrade after GA on 19 November, once your extensions (Redis, Imagick, loaders) publish builds for the final 8.6 API.

## Key takeaway

Partial function application is the most visible new syntax in PHP 8.6: `?` for one missing argument, `...` for the rest, and a fully typed `Closure` as the result. It makes callbacks and pipe-operator chains shorter and clearer. Read the PFA v2 RFC for edge cases (variadics, named placeholders) and the PHP 8.6 new features page, then start experimenting before GA lands.
