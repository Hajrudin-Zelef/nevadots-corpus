---
id: collect-261121-veille-ia/python-php/what-s-new-in-php-8-6-les-tilleuls-coop
title: "What's new in PHP 8.6 | Les-Tilleuls.coop"
domain: python-php
role: reference
task: article
actors: []
dates: ["2026-11-19"]
keywords: ["duration", "server", "function", "stream", "event", "type", "them", "context"]
source: https://les-tilleuls.coop/en/blog/whats-new-in-php-8-6
source_anchor: ""
source_lines: [1, 74]
section: "Python_PHP"
---
*This article is also available in French: PHP 8.6 : l’essentiel des nouveautés.*

PHP 8.6 was released on November 19, 2026. Three features stand out: a native polling API, a dedicated type for durations, and partial function application. Around them come a handful of smaller additions and a security change to session defaults. Here is what each one does and what it means for your projects.

Until now, the only way to watch several streams at once in PHP was `stream_select()`. It relies on the `select()` system call, which gets slower as the number of watched connections grows. A WebSocket server, or a worker that keeps many sockets open, hits that limit quickly.

The new `Io\Poll` namespace gives access to the mechanisms operating systems provide for this job: epoll on Linux, kqueue on BSD and macOS, event ports on Solaris and WSAPoll on Windows. PHP picks the right one for the platform.

Here is a minimal TCP server that waits for activity on its socket, with a one-second timeout:

```
<?php
use Io\Poll\{Context, Event, StreamPollHandle};
use Time\Duration;
$context = new Context(); // backend picked automatically
$server = stream_socket_server('tcp://0.0.0.0:8080');
stream_set_blocking($server, false);
$context->add(new StreamPollHandle($server), [Event::Read], ['type' => 'server']);
while (true) {
    $watchers = $context->wait(Duration::fromSeconds(1));
    // ...
}
```
This API does not add async to PHP: there is no event loop and no `async` keyword. It is a low-level building block. Libraries such as ReactPHP, Amp and Revolt, which needed PECL extensions to reach this level of performance, will be able to rely on the core language instead. The RFC also mentions internal uses, such as signal handling in ZTS (thread-safe) builds and improvements to PHP-FPM.

`sleep(2)`, `usleep(2000000)`, a timeout in float seconds, another one in milliseconds: in PHP, a duration has always been a number whose unit you had to guess. PHP 8.6 adds `Time\Duration`, a final, read-only class with nanosecond precision.

The snippet below computes an exponential backoff delay and compares it to a two-second limit:

```
<?php
use Time\Duration;
$attempt = 5;
$delay = Duration::fromMilliseconds(100)->multiplyBy(2 ** $attempt);
if ($delay > Duration::fromSeconds(2)) {
    // ...
}
```
You can create a duration from the unit of your choice or from an ISO 8601 string, add durations, multiply them, and compare them directly with `<` or `>`. The goal is to make it the shared duration type across the core language.

Partial function application (PFA) means calling a function with only some of its arguments and getting back a closure that expects the rest. The `?` placeholder stands for one argument, `...` for all the remaining ones.

```
<?php
$makeSlug = str_replace(' ', '-', ?);
echo $makeSlug('Hello World'); // Hello-World
```
PFA is most useful with the pipe operator `|>`, introduced in PHP 8.5, which expects a callable with a single parameter. You no longer need an arrow function at every step:

```
<?php
$slug = 'Hello World'
    |> str_replace(' ', '-', ?)
    |> strtolower(...);
// hello-world
```
One thing to watch: arguments you pass up front are evaluated when the closure is created, not when it is called. A first version of the proposal was rejected in 2021. This one passed unanimously.

- **`clamp()`** : this new function keeps a value between a minimum and a maximum, replacing`min(max($value, $min), $max)` .
- **Readonly defaults** : a`readonly` property can now have a default value, which makes it easier to implement interfaces with the property hooks introduced in PHP 8.4.
- **Parameter DocComments** : DocComments can now sit directly on a parameter and are read with`ReflectionParameter::getDocComment()` .
- **Debuggable enums** : enums can implement`__debugInfo()` to customize`var_dump()` output.
- **Stream errors** : streams get a unified error model. With the`error_mode` option, a failed`fopen()` can throw an exception instead of raising a warning.
- **URI builders** : the URI extension added in PHP 8.5 gets builder classes to assemble a URI step by step.
- **`SortDirection` enum** : a`SortDirection` enum (`Ascending` ,`Descending` ) gives ORMs and query builders a shared type.
- **Reflection** :`ReflectionProperty` gains`isReadable()` and`isWriteable()` to check whether a property is accessible from a given scope.

PHP 8.6 changes three php.ini defaults. `session.use_strict_mode` and `session.cookie_httponly` now default to `1`, and `session.cookie_samesite` now defaults to `Lax`. Strict mode blocks session fixation attacks. HttpOnly stops JavaScript from reading the session cookie. SameSite=Lax stops the cookie from being sent with `POST` requests coming from another site.

In practice, few teams will notice: Symfony and Laravel already configure their own session cookies. Only applications that rely on PHP’s native sessions are affected.

The change matters most for what it signals. For years, settings that PHP’s own documentation recommended stayed off by default so that nothing would break. With this RFC, PHP makes the safest configuration the starting point and leaves it to those who need to loosen it. We hope other directives follow in future releases, starting with `expose_php`, which is still on by default.

PHP 8.6 is a minor release: a project that is up to date on PHP 8.4 or 8.5 will upgrade without rewrites. PHP 8.2 is another story, as it stops receiving security fixes after December 31, 2026. Our teams run these upgrades regularly, from audit to production, on Symfony applications as well as custom PHP projects. We also offer training courses on the new language features, taught alongside people who maintain and contribute to PHP directly. Get in touch!
