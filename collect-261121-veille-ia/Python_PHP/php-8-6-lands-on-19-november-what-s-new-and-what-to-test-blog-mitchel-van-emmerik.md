---
id: collect-261121-veille-ia/python-php/php-8-6-lands-on-19-november-what-s-new-and-what-to-test-blog-mitchel-van-emmerik
title: "PHP 8.6 lands on 19 November: what's new and what to test | Blog | Mitchel van Emmerik"
domain: python-php
role: reference
task: article
actors: []
dates: ["2026-11-19"]
keywords: ["session", "function", "november", "readonly", "cookie", "through", "error", "clamp"]
source: https://www.vanemmerik.net/blog/php-8-6-whats-new/en/
source_anchor: ""
source_lines: [1, 101]
section: "Python_PHP"
---
PHP 8.6 is two weeks away. The release is planned for Thursday 19 November 2026. RC4 came out on 21 October and RC5 is scheduled for 5 November. The feature set has been locked since the feature freeze on 22 September, so you can already see what's changing.

Below I go through what's new, what might break and how to prepare your upgrade.

## What's new #

### Partial function application #

This is the biggest change to the language. If you put a `?` or `...` in a function call instead of an argument, PHP doesn't call the function. You get a closure back. `?` stands for exactly one argument, `...` for all remaining arguments.

```
$makeSlug = str_replace(' ', '-', ?);
$makeSlug('Hello World'); // Hello-World
$titles = array_map(strtolower(?), $titles);
```
Until now you'd write an arrow function like `fn ($s) => str_replace(' ', '-', $s)` for this. The closure you get keeps the parameter names and types of the original function.

Two things work differently from an arrow function:

- The arguments you do fill in are evaluated **right away** , when the partial is created, not when it's called. If you put a function call in there, it runs immediately.
- A follow-up RFC makes every `?` a required parameter, even if the original parameter was optional. You only keep optional parameters through`...` .

It doesn't work with `new`: `new Foo(1, ?)` is a compile error. A static factory such as `Foo::make(1, ?)` is fine.

### clamp() #

Finally a built-in way to keep a value between two bounds, without `max($min, min($max, $value))`:

```
clamp(10, min: 0, max: 100);  // 10
clamp(101, min: 0, max: 100); // 100
clamp(-1, min: 0, max: 100);  // 0
```
If `$min` is greater than `$max`, or either bound is `NAN`, you get a `ValueError`.

### Readonly properties with a default value #

Until now a default on a readonly property was a compile error. In 8.6 it's allowed:

```
final class RuleA implements Rule
{
    public readonly string $className = SomeParser::class;
}
```
Note that you can't set such a property in the constructor afterwards. That gives `Cannot modify readonly property`.

### Smaller additions #

- **`Time\Duration`** : a readonly class for durations with nanosecond precision, with addition, division and comparison. Useful for retry logic, for example`Duration::fromMilliseconds(100)->multiplyBy(2 ** $attempt)` .
- **Polling API** (`Io\Poll` ): one interface on top of epoll, kqueue and WSAPoll, as a replacement for`stream_select()` in event loops.
- **Better stream error handling** : with the`error_mode` context option you can have streams throw exceptions instead of warnings.
- **`#[\Override]` on class constants** , so you notice when a constant in the parent gets renamed.
- **Enums can have `__debugInfo()`** , for more readable`var_dump()` output.
- **Doc comments on parameters** , readable through`ReflectionParameter::getDocComment()` .
- **`enum SortDirection`** with`Ascending` and`Descending` . Nothing in core uses it yet, but frameworks can now accept it as a shared type.
- **`grapheme_strrev()`** reverses a string without breaking emoji and combined characters.

## What might break #

### Sessions are safer by default #

Three defaults in `ext/session` change:

| Setting | Was | Becomes | 
|---|---|---|
| `session.use_strict_mode` | 0 | 1 | 
| `session.cookie_httponly` | 0 | 1 | 
| `session.cookie_samesite` | empty | `Lax` | 

For most sites this is simply a win. Expect problems if:

- your JavaScript reads the session cookie through `document.cookie` ;
- you need a cross-site POST with a session, such as SAML login started from your own site, or an old form that posts from another domain. Those requests no longer send the session cookie;
- you pass in a session ID from outside yourself, for example to hand a session over between subdomains.

If these values are set explicitly in your `php.ini`, that wins. Laravel manages its own session cookies, so it's barely affected. This is mostly about code that calls `session_start()` directly.

### Deprecations and small behaviour changes #

- **Returning a value from `__construct()` or `__destruct()`** raises a deprecation. A bare`return;` is still fine. In the next major it becomes a compile error.
- `trim()` ,`ltrim()` and`rtrim()` now also strip**form feed** (`\f` ) by default.
- `array_filter()` throws a`ValueError` for an invalid`$mode` .
- The **mbregex functions** (`mb_ereg*` ,`mb_split()` and related) are deprecated and will be removed in PHP 9.0, because the underlying Oniguruma library is no longer maintained. If you use them, now is a good time to move to`preg_*` .

## How to prepare your upgrade #

**Wait for your framework.** Laravel 13 officially supports PHP 8.3 through 8.5. 8.6 isn't in the support table yet. Only upgrade production once your framework and your main packages support 8.6.

**See which packages hold you back.** Composer can tell you which dependencies block PHP 8.6:

`composer why-not php 8.6.0`
**Test now in a separate environment.** Run your test suite against the RC in a separate container or an extra CI job, with deprecations turned on (`error_reporting=E_ALL`). Search your code for places that are affected:

```
grep -rn "document.cookie" resources/ public/
grep -rn "mb_ereg" app/ src/
```
**Then upgrade in small steps:** first locally and in CI, then staging, then production. Keep the previous PHP version available as an image or package, so you can roll back quickly.

PHP 8.6 isn't a release you need to rush for. You don't have to rewrite anything, and the new features make everyday code shorter. The one thing to really watch is the session settings if you run older PHP code without a framework.
