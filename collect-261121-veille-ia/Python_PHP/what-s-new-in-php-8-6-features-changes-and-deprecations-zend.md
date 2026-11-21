---
id: collect-261121-veille-ia/python-php/what-s-new-in-php-8-6-features-changes-and-deprecations-zend
title: "What's New in PHP 8.6: Features, Changes, and Deprecations | Zend"
domain: python-php
role: reference
task: article
actors: []
dates: []
keywords: ["release", "features", "function", "support", "callback", "changes", "more", "application"]
source: https://www.zend.com/blog/php-8-6
source_anchor: ""
source_lines: [1, 180]
section: "Python_PHP"
---
PHP 8.6 arrived on November 19, 2026. With it comes a number of new features, improvements on existing functionality, and the latest round of language deprecations.

While this release doesn't contain as many changes as some of the other PHP 8 releases, many of the new features are laying the groundwork for future improvements and functionality. That means, despite the seemingly minor shifts, it's still a PHP version well worth watching.

In this blog, I break down new PHP 8.6 features and some of the more useful additions it will bring, including Partial Function Application and changes to Stream API. I also outline several of the more impactful deprecations and walk through a few considerations for teams looking to upgrade to this latest version.

Back to top
## PHP Release Date and Support Timeline

The PHP 8.6 release date was on **November 19, 2026**.

### PHP 8.6 Release Schedule

As with previous PHP versions, PHP 8.6 follows the project’s annual release cadence, moving through alpha, beta, release candidate, and general availability milestones throughout the year.

| **Milestone** | **Date** | 
|---|---|
| Alpha 1 | July 2, 2026 | 
| Alpha 2 | July 16, 2026 | 
| Alpha 3 | July 30, 2026 | 
| Beta 1 (Feature Freeze Begins) | August 13, 2026 | 
| Beta 2 | August 27, 2026 | 
| Beta 3 | September 10, 2026 | 
| Hard Feature Freeze | September 22, 2026 | 
| Release Candidates | September 14 to November 5, 2026 | 
| General | November 19, 2026 | 

### PHP 8.6 Support Timeline

PHP 8.6 will receive 2 years of active support followed by 2 years of security-only support. Based on PHP’s established lifecycle, PHP 8.6 will likely follow this support timeline:

| **Support Phase** | **Expected Timeline** | 
|---|---|
| Initial Release | November 19, 2026 | 
| Active Support Ends | December 31, 2028 | 
| Security Support Ends (EOL) | December 31, 2030 | 

The predictable PHP lifecycle gives your team ample time to evaluate new PHP 8.6 features, test application compatibility, and plan upgrades according to your business requirements rather than release deadlines.

Back to top
## What Are New Features in PHP 8.6?

While PHP 8.6 doesn't deliver a feature set on the scale of PHP 8.0 or PHP 8.1, it includes a number of meaningful improvements for developers. From new language functionality like Partial Function Application to ongoing Stream API modernization, many of the changes in this release are designed to make PHP more consistent, expressive, and future-ready.

### Partial Function Application

The feature I'm unreasonably excited about is Partial Function Application. But what does that phrase mean, exactly?

Partial Function Application (PFA) is the act of providing one or more arguments to a function in order to return a callable that accepts the remaining arguments. Execution of that callable then evaluates with the complete set of arguments.

PHP 8.1 introduced First Class Callables. These allow you to easily transform a function or method call into a closure, by "invoking" it with ellipses:

```
$callback = $this->transform(...); // Previously, [$this, 'transform']
$callback = Result::fromJSON(...); // Previously, [Result::class, 'fromJSON']
$callback = array_map(...);        // Previously "array_map"
```
The feature has made type-hinting on and passing callbacks much more robust and simpler. It answers the question of: but what if I want a specific value for one or more arguments?

Previously, you'd write that like this:

```
$callback = fn (string $message): string => str_replace('[GREETING]', 'Hello', $message);
$callback = fn (string $needle): bool => in_array($needle, $someArray, true);
```
In other words, it always required *wrapping* the function in a callback.

PFA allows you to do this like first class callables, using placeholders:

```
$callback = str_replace('[GREETING]', 'Hello', ?);
$callback = in_array(?, $someArray, true);
```
You can also use ellipsis (`...`) at the end of a PFA to indicate the PFA should accept any additional arguments and pass them on to the original function:

`$callback = array_map($mapper, ...); // Call with one or more arrays!`
Last year, I wrote that this feature will likely be what enables adoption of the PHP pipe operator, as it simplifies chaining functions where parameter order and count changes:

```
$emails = explode(';', $input)
    |> array_filter(?, is_string(...))
    |> array_map(trim(...), ?)
    |> array_map(strtolower(...), ?)
    |> array_filter(
        ?,
        filter_var(?, FILTER_VALIDATE_EMAIL | FILTER_FLAG_EMAIL_UNICODE, null)
    );
```
Previously, this would have required a ton of arrow functions and closures to accomplish. Now, we can simply use the function names and placeholders.

### Stream API Additions and Changes in PHP 8.6

PHP's Stream API is used for a variety of purposes, ranging from making HTTP requests to providing building blocks for async processing. It's largely agreed by the community to be a huge mess.

PHP 8.6 offers a number of improvements and updates that will set the stage for a more robust API that will help enable future async processing features:

- **Polling API** — This is a low-level API that will help with threaded signal handling, improve FPM performance, and provide timer implementations, which is a key building block for async.
- **Improvements to Stream API's Error Handling** — With PHP 8.6, PHP internals developers introduced a number of improvements to the Stream API's error handling. It adds consistent error modes, error types, and exceptions to make debugging and identifying issues easier. It's opt-in, but expect to see libraries start making heavy use of it.
- **TLS Session Resumption** — Finally, the team introduced a performance improvement, in the form of TLS session resumption. In the past, even when using the same connection, the Streams API would have to re-perform the TLS handshake for each and every request. The changes introduced allow re-use of that handshake.

### Time\Duration

PHP's DateTime extension is one of its gems; work with similar libraries in other languages, and you'll quickly wish you had PHP's API.

One place it has a shortcoming: while it has a `DateInterval` class for calculating new dates based on specified intervals, this only works on calendar dates, not time values.

PHP 8.6 introduces `Time\Duration`, which allows detailing time durations with down to nanosecond precision. This is also the first step in several planned additions, changes, and rewrites to improve the DateTime library even further!

### clamp()

A common operation in development is to check if a value falls within a certain range, and if not, set it to either the start or end of that range, depending on which is closer.

In programming, this is often termed "clamp", and PHP 8.6 introduces it with the following signature:

`function clamp(mixed $value, mixed $min, mixed $max): mixed`
Any values that can be compared can be used, but the caveat is that they must be directly comparable; you cannot compare a DateTime to a float, for instance. If and/or when PHP gets generics, this will almost certainly get generics support to tighten up the signature.

### Other Improvements in PHP 8.6

The bulk of the changes in PHP 8.6 are minor improvements to existing functionality. There are far more than those listed here, but these are some that I expect will be welcomed as useful updates.

| Additional PHP 8.6 Changes |  | 
|---|---|
| **PHP 8.6 Feature** | **Impact** | 
| **DocComments for function parameters** | Previously, if you wanted to document a specific parameter, it had to be done in the function or method docblock, and you would need to parse out the information based on standardized annotations. This change allows providing information immediately prior or following the parameter (but *before* a trailing comma or function parameter definition end). | 
| **#[\Override] for class constants** | Previously, if you redefined a constant in a class extension, there was no mechanism to flag to the compiler that the definition was intentional, and not an accident, which could lead to subtle errors. Extending this attribute to cover class constants resolves those. | 
| **Allow property writes on objects referenced by constants** | PHP has allowed assigning objects as constant values for a while now, but had a functionality gap: manipulating the referenced object was leading to fatal errors, even if the referenced object allowed the change. This resolves that issue. | 
| **Display function arguments in errors** | While opt-in (you have to enable the `error_include_args` INI directive), this will help when debugging an application, as you'll be able to see the actual values passed to functions and methods as arguments, while honoring the`#[\SensitiveParameter]` attribute. Disabled by default, because if`display_errors` is enabled in production, it could leak operational details. | 
| **Readonly property defaults** | The original readonly property RFC indicated that defaults for readonly properties were unnecessary, but real-life usage has shown it would be useful. However, this counts as the single assignment, so it can only be used as a default with an overridable value with constructor property promotion. | 
| **SortDirection enum** | Several existing sorting functions and operators exist, but there's no common way the directions are expressed. Some use constants, some use strings, some booleans, some work as function pairs — e.g. `sort()` vs.`rsort()` . `SortDirection` is opt-in currently, but expect it to be gradually rolled out as an acceptable type for sort functions in future versions. In the meantime, it's an enum you can now consume directly in your own code. | 
| **mysqli_quote_string()** | While `PDO::quote()` exists and is robust, there hasn't been an equivalent in the mysqli extension —`real_escape_string()` is often used, it has a number of footguns that can make it unsafe.`mysqli_quote_string()` provides the robustness of`PDO::quote()` natively for the extension. | 
| **Secure session configuration defaults** | Sets the defaults of `session.use_strict_mode` to 1,`session.cookie_httponly` to 1, and`session.cookie_samesite` to Lax. Combined, these prevent session fixation, XSS via JS access to session cookies, and cross-site request forgery, respectively. | 

## What Deprecations Come With PHP 8.6?

Every new PHP feature release also includes deprecations. These are used to help reduce bugs, improve security, and — despite the fact they often require that developers make changes to their applications — improve user experience by improving consistency.

There were 26 deprecations voted into this release, with the following three being the ones that are likely going to hit a lot of folks.

| Key PHP 8.6 Deprecations |  | 
|---|---|
| **Deprecation** | **Impact** | 
| **Limit max number of filter chains** | Usage of the `php://filter` URL when using the Stream API allows providing multiple filters to chain together. It has become a popular attack vector, and attackers will throw a ton of filters together in order to try and generate errors or bypass validation, leading to LFI and RCE bugs. The new limit of 16 is designed to mitigate such vectors; you can configure a higher count if needed, though. | 
| **Deprecate return values from __construct() and __destruct()** | PHP has silently discarded return values from these methods for ages, and neither method allows declaring a return type. This introduces a deprecation notice if PHP detects a return from either method, with the notice becoming an error in PHP 9.0. | 
| **End of maintenance of oniguruma and deprecation of mbstring regex functions** | Oniguruma, which is the regex engine behind the mbstring extension, ended maintenance in April 2025. Since the PHP project can no longer expect upstream security fixes on it, all mbstring regex functionality is now deprecated, including the `mb_ereg*()` family and`mb_split()` , with removal of in PHP 9.0. A PECL package, mb_onig, has been created for those who rely on this functionality, but the recommendation is to use PCRE functions with the /u modifier instead. | 

## PHP 8.6 Upgrade and Migration Considerations

For many organizations, upgrading to PHP 8.6 will be a relatively straightforward process. The new release focuses primarily on additive features, developer experience improvements, and incremental modernization rather than disruptive language changes.

That said, every upgrade should begin with a thorough compatibility assessment, especially for applications with older dependencies, custom PHP frameworks, or limited test coverage.

Free Code Assessment

### What's Hiding in Your Code?

### Should I Find a Developer to Upgrade My Web App to PHP 8.6?

If your team has experienced PHP developers, strong test coverage, and a solid understanding of your application's dependencies, an upgrade to PHP 8.6 may be manageable in-house.

However, I find that many organizations lack the time or expertise needed to evaluate compatibility issues, keep up with PHP dependency management, test business-critical functionality, and stay on top of deployment risk. This is backed up by data from the 2026 PHP Landscape Report, which shows that 1 in 3 teams did not complete a migration in the past year, and an additional 1 in 5 stating they had no migration planned at time of survey.

In those cases, partnering with third-party PHP experts – like the PHP engineers on the Zend Professional Services team – can be an effective strategy. We’ll help you assess readiness, identify potential issues, and guide the migration process, meaning your business can adopt PHP 8.6 with less risk and less disruption.

### What if I Can’t Upgrade to PHP 8.6 Right Away?

Not every team can move to the latest PHP version immediately. If you’re dealing with legacy frameworks, custom integrations, regulatory requirements and PHP compliance issues, or limited resources, then you may soon find yourself facing lengthy (and expensive) delays.

This often results in teams feeling like they’re in a race against the clock. This makes sense, as older PHP versions eventually lose community support, leading to critical security and compliance concerns. Plus, with PHP releasing a new version every year, it can be difficult to keep pace with the community.

If you need additional time, then PHP Long-Term Support (LTS) from a trusted provider like Zend can bridge the gap. For example, Zend PHP LTS delivers ongoing security fixes, support, and maintenance for supported and end-of-life PHP versions. You can modernize on your schedule while remaining secure and safeguarded against new threats.

Back to top
## Final Thoughts

PHP 8.6 may not be the most feature-packed PHP release in recent years (particularly compared to PHP 8.0, which introduced 10 major RFCs including named arguments, union types, and match expressions), but it continues the language's steady modernization. Features like Partial Function Application, `clamp()`, and `Time\Duration` improve developer productivity today, while Stream API enhancements help lay the groundwork for future capabilities.

For most teams, the upgrade should be relatively straightforward. Even so, it's worth testing against the Release Candidates early to identify deprecations, validate dependencies, and prepare for the November release. Whether you plan to adopt PHP 8.6 immediately or later in its lifecycle, understanding these changes now will help you make more informed upgrade decisions.

Free Trial
