---
id: collect-261121-veille-ia/python-php/php-8-6-is-released
title: "PHP 8.6 is released!"
domain: python-php
role: reference
task: article
actors: []
dates: []
keywords: ["function", "value", "readonly", "php", "released", "post", "features", "defaults"]
source: https://laravel-news.com/php-8-6-release-day
source_anchor: ""
source_lines: [1, 11]
section: "Python_PHP"
---
PHP 8.6 is now out! This post covers the new features, changed defaults, and deprecations in the release. For the release announcement, see our full What's new in PHP 8.6 post.

## PHP 8.6 Features

- Partial function application (PFA) lets you call a function with some arguments filled in and get back a closure that takes the rest.
- Clamp Function returns the value if it falls within the bounds, or the nearest bound if it does not
- A Time\Duration Class, a final readonly class that represents a stopwatch-style length of time with nanosecond precision. It has factory methods for each unit, arithmetic methods, and comparison support:
- Readonly Property Defaults, readonly properties can have a default value. Before 8.6 this was a compile-time error, which made it awkward to satisfy get-only interface properties from PHP 8.4 with a fixed value
- Plus much more!

You can check out our coverege of What's new in PHP 8.6 and the official PHP 8.6 landing page.
