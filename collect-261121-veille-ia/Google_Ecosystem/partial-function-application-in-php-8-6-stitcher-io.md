---
id: collect-261121-veille-ia/google-ecosystem/partial-function-application-in-php-8-6-stitcher-io
title: "Partial function application in PHP 8.6 | stitcher.io"
domain: google-ecosystem
role: reference
task: article
actors: ["Google"]
dates: ["2026-01-23"]
keywords: ["function", "replace", "input", "partial", "application", "string", "first-class", "callables"]
source: https://stitcher.io/blog/php-86-partial-function-application
source_anchor: ""
source_lines: [1, 136]
section: "Google_Ecosystem"
---
# Partial function application in PHP 8.6

Written on 2026-01-23
Just recently we got the new PHP 8.5 release, but internals are already working on the next one. PHP 8.6 will be released around November 19th, 2026, and there's already an upcoming killer feature that I can barely wait for: partial function application.

Now, to understand partial function application, it's best to start with first-class callables, a feature introduced years ago in PHP 8.1. With it, you could make references to callables using a special `...` notation:

$replace = str_replace(...);

With such a reference, you could call the function at a later point in time:

// …
$replace([' ', '_'], '-', $title);

In isolation, this might not seem all that useful, but first-class callables had some use cases, for example, when using a function that takes another callable:

$trimmedStrings = array_map(
    trim(...),
    $strings,
);

Without first-class callables, that code would look like this:

$trimmedStrings = array_map(
    fn (string $input) => trim($input),
    $strings,
);

Indeed, under the hood, first-class callables create a closure wrapping the original function, similar to the full written version in the example above.

There is a serious shortcoming with first-class callables, though; you might have guessed it based on our `str_replace()` example, which takes three arguments instead of one; however, first-class callables can only create a reference to a function as a whole, without the option of "filling in some of its parameter values up front". In case of `str_replace()`, for example, it would be a lot more useful if we had a shorthand way for writing this:

$slugReplace = fn (string $input) => str_replace([' ', '_'], '-', $input);

And that is exactly the problem that partial function application — PFA — solves. With it, you can write the above example like so:

$slugReplace = str_replace([' ', '_'], '-', ?);

Notice that question mark as the third parameter? That's a placeholder, essentially creating a new closure under the hood that wraps the underlying `str_replace()` function.

There is a lot more you can do with PFA, and we'll talk about it in this blog post. For me, the reason I'm especially excited for it is because of the pipe operator that was added in PHP 8.5. The pipe operator always requires a function with *exactly one parameter* on its right-hand side, which means you'd oftentimes have to wrap functions in another closure:

$output = $input 
    |> trim(...)
    |> (fn (string $string) => str_replace(' ', '-', $string))
    |> (fn (string $string) => str_replace(['.', '/', '…'], '', $string))
    |> strtolower(...);

With PFA, that code can be rewritten like so:

$output = $input 
    |> trim(...)
    |> str_replace(' ', '-', ?)
    |> str_replace(['.', '/', '…'], '', ?)
    |> strtolower(...);

In my opinion, PFA is the key to making the pipe operator really useful. Unfortunately it'll be almost another year before we get it, but at least we're sure it'll be added in the future!

## Partial function application in depth

The `?` argument placeholder already seems powerful, but there's a lot more you can do with PFA. First up, you can have **more than one argument placeholders**:

$replaceWhitespace = str_replace(' ', ?, ?);
$replaceWhitespace('=', $input);

Placeholders can also **appear anywhere**, not just at the end of a function:

$filterNumbers = array_filter(
    ?, // The input array must be passed 
    is_numeric(...) // We'll call `is_numeric` on each item
    // and use first-class callable syntax to reference it
);
$filterNumbers(['a', 1, 'b', 2]);

On top of the `?` argument placeholder, **there's also a `...` variadic placeholder**, which represents zero or more parameters at once and can be used to capture any parameters not yet specified:

$replaceWhitespace = str_replace(' ', ...);
$replaceWhitespace('=', $input);

Next, **PFA works with named arguments** and the variadic placeholder is smart enough to know how to deal with "the leftovers":

$filterNumbers = array_filter(
    mode: ?, // We make the `$mode` parameter the first one
    callable: is_numeric(...),
    ... // The variadic placeholder will include all leftover parameters,
    // which is only `$array`, in this case 
);
$filterNumbers(ARRAY_FILTER_USE_KEY, ['a', 1, 'b', 2]);

You may need some time to wrap your head around this one, but the RFC has a bunch more examples. Even better: PFA has very clear error messages, so if you end up writing invalid code, it should be clear how to fix it immediately.

Next, because the variadic placeholder means *zero* or more parameters, **it can be used to create a reference to a function that already has all its arguments but simply hasn't been called yet**:

$performQuery = query('UPDATE table SET …', ...);
if ($needToUpdate) {
    $performQuery();
}

As you can imagine, this might be useful to delay execution for functions that are resource-heavy.

Finally, you can also **use PFA on objects and static methods**:

$makeThriller = Book::make(
    title: ?,
    category: Category::THRILLER,
);
$book = $makeThriller(title: 'Timeline Taxi');
$publish = $book->publish(DateTime::now(), ...);
if ($shouldPublish) {
    $publish();
}

Note that there are a small number of functions where you cannot use PFA:

- constructor calls
- `compact()`
- `extract()`
- `func_get_args()`
- `get_defined_vars()`

Apart from those, PFA should work everywhere. Even with magic methods!

class Model
{
    public function __call($method, $args) {
        printf("%s::%s\n", __CLASS__, $method);
        print_r($args);
    }
}
$model = new Model();
$magic = $model->magic(?);
$magic('Hello!');

In closing, I think PFA is an amazing feature, and it will especially be useful combined with the pipe operator. I'm already looking forward to PHP 8.6!

By the way, if you have any questions or thoughts, you can always leave a comment, send an email, join my Discord server, or subscribe to my newsletter.
