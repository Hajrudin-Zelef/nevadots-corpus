---
id: collect-261121-veille-ia/python-php/php-8-6-for-laravel-developers-5-features-worth-using-aiwithghost
title: "PHP 8.6 for Laravel Developers: 5 Features Worth Using | AIWithGhost"
domain: python-php
role: reference
task: article
actors: []
dates: []
keywords: ["laravel", "clamp", "features", "function", "value", "developers", "worth", "using"]
source: http://aiwithghost.com/news/news-php-86-for-laravel-developers-5-features-worth-using/
source_anchor: ""
source_lines: [1, 223]
section: "Python_PHP"
---
# PHP 8.6 for Laravel Developers: 5 Features Worth Using

PHP 8.6 ships on November 19, 2026. It’s already at the release candidate stage, so the feature list is locked. Most “what’s new” posts list every RFC. This one doesn’t. It picks the five features that will actually chan

PHP 8.6 ships on **November 19, 2026**. It’s already at the release candidate stage, so the feature list is locked. Most “what’s new” posts list every RFC. This one doesn’t. It picks the five features that will actually change how you write Laravel code, and shows each one in a Laravel context: collections, form requests, queued jobs and the query builder.

At the end there’s an upgrade checklist so you know when it’s safe to move production apps over.

## 1. Partial function application

This is the headline feature. You call a function with some arguments filled in, mark the rest with placeholders, and get back a closure instead of a result. `?` stands for one argument; `...` stands for all the remaining ones.

You’ve written this kind of closure a hundred times:


```
$slugify = fn (string $s) => str_replace(' ', '-', $s);
```
In PHP 8.6 it becomes:


```
$slugify = str_replace(' ', '-', ?);
$slugify('Hello World'); // Hello-World
```
The closure keeps the original function’s parameter names and types, so static analysis and IDE autocompletion still work.

### Where it shines: the pipe operator

PHP 8.5 gave us `|>`, which passes one value through a chain of single-argument callables. Many useful functions take more than one argument, which used to force a closure into the middle of the chain. Partials fix that:


```
$slug = $request->input('title')
    |> trim(...)
    |> strtolower(...)
    |> str_replace(' ', '-', ?)
    |> substr(?, 0, 60);
```
No closures, no temporary variables, reads top to bottom.

### In Laravel code

Partials work with any callable, including static methods, so Laravel’s helpers fit right in:


```
use Illuminate\Support\Str;
$limit = Str::limit(?, 100);
$excerpts = $posts->pluck('body')->map($limit);
```
**Two gotchas to know:**

-   Arguments you fill in are evaluated **when the partial is created** , not when it’s called.`now()->addDay()` passed into a partial is frozen at that moment.
-   Collection methods like `map()` pass the key as a second argument. Test how your partial handles that on the 8.6 RC before refactoring a codebase.

**Verdict:** you’ll use this weekly. Combined with `|>`, it removes most one-line closures.

## 2. clamp()

`clamp()` keeps a value inside a range: it returns the value if it’s within bounds, otherwise the nearest bound. It replaces the `min(max($value, $min), $max)` pattern that’s easy to write backwards.


```
clamp(10, min: 0, max: 100);  // 10
clamp(101, min: 0, max: 100); // 100
clamp(-1, min: 0, max: 100);  // 0
```
### In Laravel code

Capping a user-supplied page size is the classic case:


```
public function index(Request $request)
{
    $perPage = clamp($request->integer('per_page', 15), min: 1, max: 100);
    return PostResource::collection(Post::paginate($perPage));
}
```
Or in a form request’s `prepareForValidation()`, so the rest of your code never sees an out-of-range discount:


```
protected function prepareForValidation(): void
{
    $this->merge([
        'discount' => clamp((int) $this->discount, min: 0, max: 100),
    ]);
}
```
It also works on any comparable type, including strings and `DateTime` objects, so you can clamp a requested report date into an allowed window.

**Laravel already has `Number::clamp()`.** It’s numbers-only. The native function is faster, handles dates, and throws a `ValueError` if `$min` is greater than `$max` (or either bound is `NAN`), which catches config mistakes early.

**Verdict:** small, but you’ll use it everywhere you handle request input.

## 3. Time\Duration

`Time\Duration` is a readonly value object for a stopwatch-style length of time, with nanosecond precision. It has factory methods for each unit, arithmetic methods, and real comparison operators. It’s the first piece of a modernized date and time API in PHP.


```
use Time\Duration;
$oneSecond  = Duration::fromSeconds(1);
$halfSecond = $oneSecond->divideBy(2);
$total      = $oneSecond->add($halfSecond);
$total > $halfSecond; // true, comparison operators work
```
### Why Laravel devs should care

Laravel config is full of bare integers where the unit lives in your head: `retry_after => 90`, `$timeout = 120`, `$backoff = [1, 5, 10]`. Was that seconds or milliseconds? `Duration` makes the unit part of the type.

Exponential backoff in a queued job reads like the math it is:


```
use Time\Duration;
class SyncToCrm implements ShouldQueue
{
    public function backoff(): array
    {
        return collect(range(0, 4))
            ->map(fn (int $attempt) => Duration::fromSeconds(2)->multiplyBy(2 ** $attempt))
            ->map(/* convert to seconds for Laravel today */)
            ->all();
    }
}
```
The last line is the honest part: **Laravel’s APIs don’t accept `Duration` yet.** For now you’ll convert at the boundary. Expect framework support to follow, the same way Carbon became the standard for dates.

A better fit today is timing things you log:


```
$started = hrtime(true);
$report->generate();
$elapsed = Duration::fromNanoseconds(hrtime(true) - $started);
if ($elapsed > Duration::fromSeconds(5)) {
    Log::warning('Slow report', ['report' => $report->id]);
}
```
**Verdict:** use it in your own code now; watch for Laravel to adopt it.

## 4. The SortDirection enum

PHP 8.6 adds a global `SortDirection` enum with two cases, `Ascending` and `Descending`. It’s deliberately unbacked (no `'asc'`/`'desc'` string values), because SQL, APIs and query strings all spell sort direction differently. You map it at the boundary.

Every framework has invented its own version of this. A shared type means packages can finally agree on one. The RFC itself uses a query builder as the example.

### In Laravel code

Eloquent’s `orderBy()` still takes `'asc'` or `'desc'` today. But you can use the enum in your own API layer right now, and get type safety instead of validating strings:


```
final class ListPosts
{
    public function __invoke(SortDirection $direction = SortDirection::Descending)
    {
        return Post::query()
            ->orderBy('published_at', match ($direction) {
                SortDirection::Ascending  => 'asc',
                SortDirection::Descending => 'desc',
            })
            ->paginate();
    }
}
```
In the controller, turn the query string into the enum once:


```
$direction = $request->query('sort') === 'asc'
    ? SortDirection::Ascending
    : SortDirection::Descending;
```
The enum is trivial to polyfill on PHP 8.1+ if a package wants to use it before dropping older versions.

**Verdict:** low effort now, likely high payoff once Eloquent accepts it directly.

## 5. The Polling API (Io\Poll)

This is the one you probably won’t write yourself, but it may matter most long term. The new `Io\Poll` namespace gives PHP native access to the operating system’s polling mechanisms: epoll on Linux, kqueue on macOS and BSD, and WSAPoll on Windows. Until now, userland PHP was stuck with the much slower `stream_select()`.


```
use Io\Poll\{Context, Event, StreamPollHandle};
$poll = new Context();
$poll->add(new StreamPollHandle($server), [Event::Read], ['type' => 'server']);
foreach ($poll->wait(1) as $watcher) {
    if ($watcher->hasTriggered(Event::Read)) {
        // accept the connection
    }
}
```
### Why a Laravel dev should care

You’ll almost never touch this in a controller. Its value is in the tools you already depend on. Laravel Reverb (WebSockets) and long-running servers under Octane sit on event loops, and event loops are built on exactly this kind of polling. The RFC’s main motivation is internal (signal handling, PHP-FPM improvements), with async libraries as the second audience.

The API is intentionally minimal in 8.6: it polls streams and nothing else. A follow-up RFC proposing timers, signals and process handles is already in draft. If it lands, building a serious event loop in pure PHP gets much easier.

**Verdict:** don’t refactor anything. Watch for Reverb, ReactPHP and Amp to adopt it and get faster for free.

## Upgrading a Laravel app to PHP 8.6

Don’t upgrade production on release day. Run this checklist first:

-   ☐ **Wait for Laravel’s official PHP 8.6 support** in a Laravel 13 point release, and update the framework before PHP.
-   ☐ **Add PHP 8.6 to your CI matrix now** as an allowed-failure job against the RC, so breakages show up before November 19.
-   ☐ **Check your key packages** (Filament, Livewire, Spatie packages, your payment SDK) for 8.6 support in their changelogs.
-   ☐ **Read the 8.6 deprecations list** and fix any warnings your test suite surfaces.
-   ☐ **Check for name clashes** : a global`clamp()` function or`SortDirection` class in your own code or helpers file will now collide with PHP’s.
-   ☐ **Native sessions only:** three`php.ini` session defaults change (strict mode, HttpOnly, SameSite=Lax). Laravel manages its own session cookies, so most apps are unaffected.
-   ☐ **Update Docker images, Forge or Cloud runtimes, and Composer’s `php` constraint** together, so local, CI and production match.

**Bonus feature:** readonly properties can now have default values, so `public readonly string $queue = 'emails';` finally compiles. Handy in DTOs and job classes.

## Which one will you use first?

If you only adopt one feature, make it partial function application. Paired with the pipe operator, it changes how everyday Laravel code reads. `clamp()` is the quickest win, `Duration` and `SortDirection` are bets on where the framework is heading, and `Io\Poll` is a quiet upgrade to the tools under your app.

Which feature are you most excited about? Tell me in the comments, and subscribe if you want the hands-on upgrade guide when Laravel ships 8.6 support.
