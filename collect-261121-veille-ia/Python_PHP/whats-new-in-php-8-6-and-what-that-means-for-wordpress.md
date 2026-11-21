---
id: collect-261121-veille-ia/python-php/whats-new-in-php-8-6-and-what-that-means-for-wordpress
title: "What’s new in PHP 8.6 and what that means for WordPress?"
domain: python-php
role: reference
task: article
actors: []
dates: []
keywords: ["wordpress", "core", "functions", "plugins", "deprecations", "function", "already", "found"]
source: https://developer.wordpress.org/news/2026/11/whats-new-in-php-8-6-and-what-that-means-for-wordpress/
source_anchor: ""
source_lines: [1, 105]
section: "Python_PHP"
---
The new version of PHP, 8.6, is set to be released on November 19th, just 2 days after WordPress 7.2 RC1 (release candidate 1). When we talk about PHP in relation to WordPress, we usually talk about the minimum version supported by core and plugins, which is, since WordPress 7.0, PHP 7.4.

PHP 8.6 will bring:

- Newly added 1 INI directive, 15 enums, 17 functions, 24 classes, 47 constants, and 1 interface;
- Improvements to 27 functions;
- Changes to 39 INI directives, 21 functions, 1 class, and 11 constants;
- Deprecations for 26 functions and 2 constants;
- Removals of 2 extensions, 15 functions, and 2 classes.

The most talked-about new feature is partial function application (PFA), and rightfully so. With placeholders such as `?` converting every parameter into required, and `…` keeping the original statuses and default values, it almost feels like magic. This will have a great impact on the pipe operator (|>), added in PHP 8.5. Unfortunately, we won’t see any of these in WordPress core any time soon, as long as we don’t move minimum supported PHP versions more closely to more recently released  PHP code.

Another useful change in PHP 8.6 is the addition of the clamp() function, which frankly already exists in many frameworks. In WordPress, we saw the benefits of this logic in the 6.1 release when fluid typography entered the core. Even though fluid typography is a CSS enhancement, the `clamp()` function in PHP works basically the same: there’s a value and a defined range of minimum and maximum bounds for the returned value. While in CSS it is very useful for various numeric values, in PHP it can have broader applications, one of which is the range of dates.

But rather than listing all the changes in the new PHP release, which you can already find in several places (I’ll link to them at the end of this article), I’d like to take the opportunity to look at this progress through WordPress’s eyes. Looking at WordPress’s relation to PHP 8.6, **compatibility** and **feature adoption** are two separate questions.

## What needs fixing?

On the compatibility side, WordPress is already actively working on it. PHP 8.6 is added to the PHPUnit matrix for the 7.2 development cycle, ~~but the job is still allowed to fail~~. As @desrosj noted in comments, tests running on PHP 8.6 are not allowed to fail since 10th November (r65384). What we can expect are mostly deprecation notices, and mostly in tests.

The list of deprecations in 8.6, proposed for removal in PHP 9, is a long one, and WordPress core, as well as extenders, should carefully search through the code to locate them all. For example, PHP 8.6 is deprecating Oniguruma-based mbstring regex functionality. If you use `mb_ereg()`, `mb_ereg_replace()`, `mb_split()`, etc., you should follow the core example and start replacing them with modern counterparts.

Another problem that core is already fixing is incorrectly-cased function calls that do not match their declaration.

Other deprecations that need auditing include:

- `spl_object_hash()` (found 8,772 matches across 2,359 WordPress plugins),
- `is_double()` (found 790 matches across 311 WordPress plugins),
- `is_integer()` (found 2,720 matches across 887 WordPress plugins),
- `is_long()` (found 10,809 matches across 2,258 WordPress plugins), etc.

As it seems, for compatibility needs, PHP 8.6 will make us look most closely at the following components:

- **Build/Test Tools** : PHPUnit failures and deprecations
- **External libraries** : deprecated PHP functionality in bundled dependencies
- **Formatting** : URL handling, string functions, encoding changes
- **HTTP API** : URI handling and changed extension behaviour

## What could we gain?

Let’s stop for a moment and imagine a world in which WordPress can start using new PHP features right away.

My first heartbeat goes to URI/URL processing. Even though PHP introduced the URI extension in 8.5, 8.6 adds 2 new classes and more functionality. WordPress core has an enormous amount of URL-handling code (wp_parse_url(), home_url(), site_url(), admin_url(), esc_url(), etc.), plus URL handling through HTTP, REST, canonical redirects, multisite, block parsing, media, etc. Our lives would be much easier if we used the native PHP functionality for these.

Further down the PHP 8.6 changes, a Duration class (Time\Duration) caught my attention. Object-cache TTLs, Cron intervals, HTTP timeouts, REST behavior, authentication expiry, nonce lifetimes, update checks… WordPress is full of durations. A proper `Time\Duration` type could make APIs substantially less ambiguous. Maybe one day we could have something like:

`set_transient( string $transient, mixed $value, int|Time\Duration $expiration ): bool`
But not all the 8.6 updates are only wishful thinking. PHP 8.6 introduces static closure inference and stateless closure caching as part of closure optimizations. Static closure inference seems quite interesting, as it turns non-static closures into static ones when they guarantee not to use `$this`. On the other hand, stateless closures will be cached between uses. The best part is that WordPress doesn’t have to do anything to use this. Well, it has to be compatible with PHP 8.6 and, when running on PHP 8.6, it gets these runtime optimizations for free.

Going out of PHP 8.6 scope, and looking at the missing opportunities that PHP 8.x offers, having 7.4 as the minimum supported PHP version prevents core from natively designing around:

- 8.0 – named arguments, attributes, union types, match, nullsafe operator…
- 8.1 – enums, readonly properties, fibers, intersection types…
- 8.2 – readonly classes, DNF types, Random extension…
- 8.3 – typed class constants, `#[\Override]` attribute,`json_validate()` *…
- 8.4 – property hooks, asymmetric visibility, new DOM/HTML5 API, new `array_*()` functions…
- 8.5 – pipe operator, URI extension, `array_first()` and`array_last()` functions**…

*There was an attempt to polyfill json_validate() in userland, but the proposal was dropped because it couldn’t avoid a performance hit.

** WordPress core added polyfills for array_first() and array_last() functions in 6.9.

~~Perhaps we could have a polyfill for 8.6’s~~  As @westonruter noted in comments, `clamp()` function as well.`clamp()` is already in core since 7.1 via r62719 for #65143.

## Tale as old as Date/Time…

Having a reputation for long backward compatibility, as stable as it sounds, has an ugly face of dragging performance and security down, as well as keeping the shiny new functionalities away from WordPress core developers.

A significant success in moving the minimum supported PHP version was made with the Site Health project in 2018/2019. WordPress introduced administrative notices and upgrade prompts to alert site owners and hosting providers running EOL PHP versions. These prompts were later integrated into the Site Health component.

The latest discussion at WordCamp US 2026 points out that reasons for sites to upgrade have been less compelling recently.

### How do we decide on the minimum supported PHP version?

There are several criteria, explained in the description of this Trac ticket. One of these is the percentage of PHP version in use. Once the version drops below 5%, we bump the minimum supported version one point above. Given that the rest of the criteria are satisfied.

In April this year, ~20.5% of WordPress websites were running on PHP 7.4, and now, at the end of October, we’re looking at ~17% (source).

## What can we do to have nice things?

As the Site Health initiative in 2018 proved, the easiest and most logical way to reach users is through the WordPress dashboard. But old “secure and fast” arguments don’t work the way they used to. However, with fast-paced AI changes, it seems we have developed a taste for new, more powerful features.

I like the idea of feature gating, as someone suggested at the WordCamp US discussion. The basic idea is to offer a very nice feature, but only if the minimum PHP version is 8.x. Which is the exact reality – if you ran a more up-to-date PHP version, I could code cooler functionalities for you. But we remove the “could” part and have it ready now. Instead of coding to keep the past, we code to have the future ready now. And from the user’s perspective, the future is just one “Upgrade” click away.

Maybe it’s a logistics nightmare to do this with Core, but extenders could implement it more fluidly. Especially with commercial products and services (pro versions of plugins, premium support, etc.). Maybe Core could offer these extenders something in return. Showcasing their free plugins as featured, adding an “innovator” badge, creating the “open-source drivers” page, etc.

We are riding a complex, Byzantine machine. One cog, no matter how big, can not move every part in the same direction. Many cogs, even smaller ones, strategically positioned, can easily build enough force to turn stagnation into movement. And once moved, this monumental piece of internet pie could be unstoppable. Again.

## Resources and further reading

WordPress core:

PHP:

- Stitcher: What’s new in PHP 8.6
- Laravel news: What’s New in PHP 8.6
- PHP Watch: PHP 8.6 changelog
- PHP upgrading notes: UPGRADING
- PHP Wiki: PHP 8.6 deprecations
- PHP 8.6 RC4: NEWS
- PHP Foundation: News

*Props @areziaal and @javiercasares for peer review.*

## Leave a Reply
