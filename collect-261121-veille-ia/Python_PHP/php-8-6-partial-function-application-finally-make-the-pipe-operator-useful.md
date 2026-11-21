---
id: collect-261121-veille-ia/python-php/php-8-6-partial-function-application-finally-make-the-pipe-operator-useful
title: "PHP 8.6 Partial Function Application: Finally Make the Pipe Operator Useful"
domain: python-php
role: reference
task: article
actors: []
dates: ["2026-11-06"]
keywords: ["string", "replace", "function", "arguments", "pipe", "operator", "argument", "array"]
source: https://shocm.me/posts/2026-11-06-php-86-partial-function-application-pipe-operator/
source_anchor: ""
source_lines: [1, 157]
section: "Python_PHP"
---
PHP 8.6 ships on November 19, 2026, and one of its best features is also one of its smallest: a `?` character. Partial function application (PFA) lets you call a function with some arguments filled in and get back a closure that waits for the rest.

If you tried the pipe operator in PHP 8.5 and found yourself wrapping half of your steps in arrow functions, this is the feature that fixes it. Here is how it works, where it helps, and where it stops.

## The problem: pipes want exactly one argument

PHP 8.5 added the pipe operator (`|>`). The right-hand side must be a callable that accepts exactly one parameter. That is fine for `trim(...)` or `strtolower(...)`, but most useful functions take more than one argument. As Brent Roose points out on stitcher.io, you end up writing this:

```
$output = $input
    |> trim(...)
    |> (fn (string $string) => str_replace(' ', '-', $string))
    |> (fn (string $string) => str_replace(['.', '/', '…'], '', $string))
    |> strtolower(...);
```
It works, but the noise buries the intent. First-class callable syntax (`strlen(...)`, added in PHP 8.1) only references a function as a whole. It cannot pre-fill arguments.

## The fix: the `?` placeholder

With PFA, a `?` marks the argument you will supply later. Everything else is bound right now. The result is a closure.

```
$makeSlug = str_replace(' ', '-', ?);
echo $makeSlug('PHP Tek 2027 Schedule');
// PHP-Tek-2027-Schedule
```
Drop that into the pipe and the chain reads like a recipe:

```
$slug = $title
    |> trim(...)
    |> str_replace(' ', '-', ?)
    |> str_replace(['.', '/', '…'], '', ?)
    |> strtolower(...);
```
No wrapper closures, no throwaway parameter names. Each line says what it does.

## Placeholders can go anywhere

The `?` does not have to be the last argument. Place it wherever the missing value belongs, and use more than one if you need to. These examples come from the RFC walkthrough on stitcher.io:

```
// Two placeholders
$replaceWhitespace = str_replace(' ', ?, ?);
$replaceWhitespace('=', $input);
// Placeholder first, callback bound
$filterNumbers = array_filter(?, is_numeric(...));
$filterNumbers(['a', 1, 'b', 2]);
```
That second one is handy. Functions like `array_filter()` and `array_map()` disagree about argument order, which is a classic PHP papercut. With `?` you pick the slot, so the closure fits whatever pipeline you are building.

## The `...` variadic placeholder

A `...` placeholder stands in for zero or more remaining arguments. Use it when you want to bind the first few arguments and pass everything else through:

```
$replaceWhitespace = str_replace(' ', ...);
$replaceWhitespace('=', $input);
```
It also works with named arguments. Named placeholders let you reorder parameters, and `...` collects the leftovers:

```
$filterNumbers = array_filter(
    mode: ?,
    callback: is_numeric(...),
    ...
);
$filterNumbers(ARRAY_FILTER_USE_KEY, ['a', 1, 'b', 2]);
```
Because `...` can match zero arguments, you can also use it to freeze a fully specified call and run it later. The RFC walkthrough shows this as a way to delay a heavy operation until you know you need it:

```
$performQuery = query('UPDATE table SET …', ...);
if ($needToUpdate) {
    $performQuery();
}
```
## It works on methods and static calls too

PFA is not limited to plain functions. Instance methods and static methods are fair game:

```
$makeThriller = Book::make(
    title: ?,
    category: Category::THRILLER,
);
$book = $makeThriller(title: 'Timeline Taxi');
```
Magic methods work as well. A `__call()` handler receives the arguments when you invoke the resulting closure.

## A practical example: normalizing a CSV import

Say PHP Architect is importing a subscriber list where each email needs cleaning. With PFA and the pipe operator, a one-off normalizer becomes a short pipeline:

```
function normalizeEmail(string $raw): string
{
    return $raw
        |> trim(...)
        |> strtolower(...)
        |> str_replace(' ', '', ?);
}
$emails = array_map(normalizeEmail(...), $rows);
```
Compare that to the pre-8.6 version, which needs an arrow function in the middle of the chain just to supply the second argument to `str_replace()`. The new version has no ceremony, and it is easy to extend. Adding another step is one more line.

You can also build reusable pieces up front and compose them:

```
$stripPunctuation = str_replace(['.', ',', '!'], '', ?);
$collapseSpaces   = preg_replace('/\s+/', ' ', ?);
$headline = $rawTitle
    |> trim(...)
    |> $stripPunctuation
    |> $collapseSpaces;
```
Named closures like these are easy to unit test on their own, which is a nice side effect.

## What PFA cannot do

There are a few exclusions. According to the stitcher.io write-up, you cannot use PFA with:

- constructor calls
- `compact()`
- `extract()`
- `func_get_args()`
- `get_defined_vars()`

Everything else should work. The RFC also aims for clear error messages when you write an invalid placeholder, so mistakes should be quick to spot. Read the full Partial Function Application v2 RFC for the exact rules.

## Is it just sugar for arrow functions?

Mostly, yes. Stitcher.io notes that first-class callables create a closure wrapping the original function, and PFA behaves the same way: the placeholder call creates a new closure around the underlying function. You could always write `fn ($x) => str_replace(' ', '-', $x)`. What changes is how much you have to type and how much you have to read.

The gain is largest in two places:

- **Pipe chains** , where every step must be a one-argument callable.
- **Callbacks to `array_map()` and friends** , where you want to bind configuration once and pass the closure around.

If a step needs real logic, such as a branch or a loop, an arrow function or a named function is still the right tool.

## Try it before November 19

You do not have to wait for the final release. If you want to experiment, I covered how to test your app on a PHP 8.6 release candidate with Docker and GitHub Actions. While you are at it, run through the PHP 8.6 deprecations scanner post so the upgrade is clean.

Remember that PFA syntax is new, so older tooling such as linters, formatters, and static analyzers may need updates before they understand `?` placeholders. Check your editor and CI tools on an 8.6 branch before you roll the syntax out across a codebase.

## Wrap up

Partial function application is a small syntax addition that removes a real annoyance. Combined with the pipe operator from PHP 8.5, it lets you write data transformations as flat, readable pipelines instead of nested closures. When 8.6 lands, start with your string and array cleanup code. That is where you will feel the difference first.

## Sources

- Brent Roose, Partial function application in PHP 8.6, stitcher.io
- Brent Roose, What’s new in PHP 8.6, stitcher.io
- RFC: Partial Function Application v2, wiki.php.net
- PHP 8.6 release schedule, wiki.php.net
