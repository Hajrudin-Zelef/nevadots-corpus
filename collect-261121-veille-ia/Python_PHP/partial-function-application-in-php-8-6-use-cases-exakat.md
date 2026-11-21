---
id: collect-261121-veille-ia/python-php/partial-function-application-in-php-8-6-use-cases-exakat
title: "Partial Function Application in PHP 8.6 use cases - Exakat"
domain: python-php
role: reference
task: article
actors: []
dates: []
keywords: ["function", "array", "value", "partial", "string", "closure", "arrow", "call"]
source: https://www.exakat.io/partial-function-application-in-php-8-6-use-cases/
source_anchor: ""
source_lines: [1, 218]
section: "Python_PHP"
---
Here is a classic PHP line:

<?php
$names = array_map(fn($user) => strtoupper($user), $users);
?>

It could be possibly shortened with a string, but that would look like so PHP 4 that this post starts with an arrow function. It is short, the intent is clear, and not so recent PHP versions made it cheap to type. And, when you look closely at what that function is doing, you discover it exists only to receive a value and hand it straight to another call: I call them relay functions. We need it to hand over a function call to another place.

PHP has come a long way to do that in various forms. PHP 5.3 brought closures in 2009, with the `use` keyword to capture variables from the surrounding scope. PHP 7.4 added arrow functions, so the capture became automatic and the syntax shrank. PHP 8.1 introduced the first-class callable syntax, `strtoupper(...)`, which turns any function into a value without writing a wrapper at all, or at least explicitely. Each step removed a little more ceremony between “I have a function” and “I have a handle to that function”.

PHP 8.6 goes one step further with partial function application, or `PFA` (Peefa, anyone?). The idea is simple. You set some arguments of a function now. You leave the others open, and so, you get a new callable that is half backed. The open spots are marked with a question mark, and the function you get back is a real closure with a real signature. Once you start looking for them, a surprising number of arrow functions in a typical codebase turn out to be partial applications in disguise.

There are already articles which cover the definitions for PFA. Taking my hat of a static code analysis author, I want to cover what current situations may be improved with PFA. So, this article walks through the situations where PFA can replace code you already have. We start with the simplest case and end with the most niche ones, grouped in five parts. Every example is a refactoring of something PHP developers write today, so you can judge the gain for yourself. Among so many of them, there are certainly a few that you are currently using.

## Part I: Feeding the function one hole at a time

### Unary callbacks

Let us start with the elementary form. You know some arguments of a function, and you want to leave exactly one slot open for the data. This is the flagship case, and it is the example the RFC uses to introduce the feature.

<?php
// Before
$numbers = array_filter($values, fn($value) => is_numeric($value));
// After
$numbers = array_filter($values, is_numeric(?));
// Competing syntax: identical to the above, because is_numeric() has one argument.
$numbers = array_filter($values, is_numeric(...));
?>

The arrow function existed only to route a value into a call. The partial does the same job with the routing built into the syntax. Note the question mark rather than three dots. A single placeholder accepts exactly one argument, and any extra value that `array_filter` or `array_map` might pass along is simply not forwarded. We will come back to why this matters in the caveats, because a variadic placeholder can surprise you in the least pleasant way.

### Fixing several arguments

This is a direct extension of the previous case. You bake in several known values at build time, and the function runs when the data finally arrives. The closure factory is the classic pattern here: a function that builds a closure with a `use` clause, so that the configuration gets stored somewhere.

```
<?php
// Before
function makeLogger(string $channel, string $level): Closure {
    return function (string $message) use ($channel, $level) {
        log_message($channel, $level, $message);
    };
}
$logger = makeLogger('payments', 'warn');
// After
$logger = log_message('payments', 'warn', ?);   // a closure taking one string
$logger('Payment declined');
?>
```
The factory function did nothing except hold on to two strings. Now the holding is done by the language itself. Nobody will miss the factory, although its unit tests may need a moment of silence. Nobody will miss that the arguments are fixed here with literal values. How these possible `magic numbers` reach that place is another problem entirely.

### Pipelines and holes

PHP 8.5 gave us the pipe operator, `|>`, which passes the result of each step to the next one. In a pipeline, each step is usually a small function call with the data implied. Partial applications fit this shape perfectly, because the data is the hole, and the hole is the only thing the pipe needs to fill.

```
<?php
$slug = $title
    |> str_replace(' ', '-', ?)
    |> strtolower(?)
    |> mb_substr(?, 0, 200);
?>
```
The result reads from top to bottom like a recipe, with no intermediate variables to invent and no nested calls to untangle from the inside out. The pipe is good for readability, and the partials are good at giving each step its arguments. Together they make a slug that you can explain to a colleague in one breath. Good names are hard, and the best variable is the one you don’t have to name.

## Part II: Naming the holes

### Named arguments

Positional partials can only fix leading arguments. You fill the first slots, then the holes come after. Named arguments remove that limit. You can fix any parameter by name, and the holes follow the signature that remains.

<?php
$roundTwo = round(precision: 2, num: ?);             // Closure(float $num)
$isoDate  = date(format: 'Y-m-d', timestamp: ?);      // Closure(?int $timestamp = null)
?>

Note two important details. First, the default values of the remaining parameters are preserved, so `$isoDate` still accepts a missing timestamp and uses the current time: there is no need to repeat them. Secondly, the hole itself needs a name. A positional question mark placed after a named argument is refused by the compiler with a fatal error. The compiler is right to be strict, and the error message is clear enough to make you feel slightly embarrassed about the attempt.

### Turning the order around

Functions are written in the order that suited their author. As per convention, the needle comes first in `in_array()`, the haystack second. And the strictness flag last. When you create a callback, the order may work against you. Before PFA, the fix was a hand-written closure that swapped the arguments back.

<?php
// Before
$hasAccess = fn($role) => in_array($role, $allowedRoles, true);
// After
$hasAccess = in_array(?, $allowedRoles, true);      // Closure($needle)
?>

Named placeholders go one step further. They let you choose which hole comes first, independently of the position in the signature.

```
<?php
function stuff(int $i1, string $s2): string { /* ... */ }
$c = stuff(s2: ?, i1: ?);                            // Closure($s2, $i1)
?>
```
The second hole in the signature becomes the first parameter of the new closure. The dependency between your data and your configuration is inverted without any ceremony at all.

This might come very handy: when the list of argument is long, the interesting arguments might happen at the end of that list, after a long list of default parameters. The PFA give you a chance to promote arguments to their right precedence.

### PFA of PFA

Partials can themselves be partialed, one argument at a time. This is the progressive refinement pattern, and it replaces the telescoping builder that developers sometimes use to configure an object step by step.

<?php
$c  = $e->foo(?, ?, 3);   // Closure($a, $b)
$c2 = $c(1, ?);           // Closure($b)
?>

Builders are a perfectly honest pattern, and many libraries use them well. But a builder with `->withContext()->withField()`chains is a way to fill in a form one method at a time, and each method has to return a new object for the next one to work. A partial fills the same form in a single expression, and the result is a plain callable that you can store, pass around, or throw away.

## Part III: Deferring execution

### The first-class callable, the degenerate case

Here is the case where nothing is fixed at all. `count(...)` creates a callable from `count` with every argument still open. This is the first-class callable syntax that arrived in PHP 8.1, and it is the partial application with all its holes left empty.

There is no refactoring value in this entry on its own. It is included because every other example in this article builds on the same mechanism, and because it reminds us that PFA is not a brand new idea wearing an old name. It is the same idea, with a few more ways to fill the gaps.

### Thunks: everything is applied, call it later

Sometimes all the arguments are known, and the only thing you want to delay is the execution itself. The result is a thunk, a parameterless callable that runs the work only when you ask for it. You get it by supplying every argument and keeping the ellipsis at the end.

```
<?php
$default = expensive(3, 4, $point, ...);
if ($some_condition) {
    $result = $default();
}
?>
```
This is useful for pushing a heavy computation off until someone actually needs its result. The thunk is a niche tool, and most codebases will not need it every day. Still, it is pleasant to have a way to write a deferred call without wrapping it in a closure with an empty `use`clause, which is the kind of code that makes readers stop and ask why it exists. This might not replace a job queue, but it may help postpone a smaller task after the client has received the results, and it is a little gain.

I also didn’t know that PHP had thunk. Now, you know too.

## Part IV: Object territory

### Methods and invokable objects

PFA works with any callable, and that includes the callables that live inside objects. Static methods, instance methods with `$this`already bound, invokable objects, and magic `__call` methods all accept holes. This is where the boilerplate tends to pile up in object-oriented code.

```
<?php
$eMaker  = E::make(1, ?);             // static method, a Closure
$debug   = $logger->log('debug', ?);  // $this is bound to $logger
$handler = $middleware(?, $request);  // invokable object
?>
```
Think about how many times you have written `fn($x) => $this->service->transform($x, $this->config)`. Each of those lines is a small wrapper that exists only to fix two arguments and leave one open. The partial version says the same thing in fewer characters and without a `$this` lookup hidden inside a closure. Your future self will read the code in the order the ideas were written, which is always a relief.

### The object is the hole

The previous case fixes the arguments of one object. The next one turns the question around. You have a list of different objects, and you want to apply the same method to each of them. Only the instance changes from one call to the next. In PFA, the `this` slot can be a hole, which means the object becomes the parameter of the new callable.

<?php
$formatted = array_map(DateTimeImmutable::format(this: ?, format: 'c'), $dates);
$c = DateTimeImmutable::setTimestamp(?, this: ?);   // fn(int, DateTimeImmutable)
?>

The class name becomes the type of the hole, so the same code works against interfaces and parents as well. The `this` slot can be reordered like any named placeholder, which means the object does not have to come first in the signature. This is the point where a method call starts to behave like a function that happens to know about its object. The RFC is precise about the syntax here, and its rules are worth reading before you rely on them in production code.

### Scope and late static binding

A partial inherits the scope where it was created. This matters for `static::`, which is resolved at creation time, not at call time. Private methods can be partialed inside their class and passed outside, so the caller gets the operation without getting the access.

```
<?php
class P {
    private static function m($x) { return static::class . ": $x"; }
    public static function get() {
        return static::m(?);   // P::get() and C::get() each yield their own m()
    }
}
?>
```
This removes a whole family of public wrapper methods whose only purpose was to expose a private operation to the outside world. The wrapper used to be a small act of diplomacy. Now the partial carries the permission with it, and the class can keep its private methods private.

## Part V: Meta and ecosystem cases

### Preconfigured callables in constants

Partial applications are legal in constant expressions, property defaults, and parameter defaults. This means you can declare a callable at the place where the class is defined, rather than building it lazily the first time someone uses it.

```
<?php
class Example {
    public const BASE = 10;
    // two nested PFA as a default value
    private Closure $arrayToInt = array_map(intval(?, self::BASE), ?);
}
?>
```
The lazy initializer is a familiar pattern: `if (!$this->callback) { $this->callback = ...; }` scattered through the methods, each one a small place where a bug can hide. A declarative default says what the callback is, once, in the same place as the data it depends on. It is a modest improvement, but modest improvements add up across a large codebase.

### Reflection-friendly callables

A partial application produces a real Closure, and that Closure carries a full signature. Names, types, default values, by-reference parameters, and attributes such as `#[SensitiveParameter]` are all visible through `ReflectionFunction`. Dependency injection containers and framework code can inspect the callable and decide what to pass to it, without guessing from a string or an array.

This is mainly good news for framework authors. A string callable like `'strtoupper'` tells the container almost nothing, and an array callable is not much better. A Closure with a precise signature is the kind of thing a type-safe tool can work with, and it is the kind of thing that a static analyzer can count, check, and report on.

## Caveats, or the places where it bites

Partial function application is clean, but a few rules are worth keeping in mind before you rewrite a large part of your code.

**Arguments are evaluated at creation time**. The expression `speak(?, getArg())` calls `getArg()` immediately, not when the partial is finally invoked. If you expected the call to be deferred, the surprise will arrive at the worst possible moment, usually in a production log.

**Question mark or dots as a callback**. A variadic placeholder forwards every extra argument. Consider `array_find` passing the key as a second argument. That key may land in the optional `$base` parameter of `intval`, and your code will convert strings in a base that nobody chose on purpose. For callbacks, prefer the single question mark, which accepts exactly one value and ignores what it does not need.

**What is not supported**. The `new` expression cannot be partialed, so use a static factory method or the lazy objects of PHP 8.4 instead. Functions that work on the current scope, such as `compact`, `extract`, and the `func_get_args` family, are also out of reach. The `this:` placeholder is only available with the `Class::method()` syntax, and it can only be used as a placeholder.

**Common errors**. Too many or too few placeholders, a positional placeholder after a named argument, and a named parameter that overwrites a placeholder all produce clear error messages. The compiler is strict, and its messages are a good place to learn the rules quickly.

## The bigger picture

PFA does make PHP faster, safer, and more modern, a little bit with each aspect. What it actually does is shrink the gap between a function and the values you need to run it. Each of the situations above is a small, local refactoring, and each one looks harmless on its own. Yet, I was first surprised to realize how often they are going to be useful. And the more interesting question is what happens when you put them all together and run them over an entire codebase.

New features are introduced by their own merit: easily create a closure by setting some of its parameters, and leaving the others open; or use it with the pipe operator `|>` for readability. And such aspects are critical, yet they are not the only impact in the code. It is always interesting to see what current patterns may be replaced by this new one, and what it tell us about the organisation of the code as a whole.
