---
id: collect-261121-veille-ia/python-php/python-3-16-0-alpha-2-is-now-available-python-insider
title: "Python 3.16.0 alpha 2 is now available | Python Insider"
domain: python-php
role: reference
task: article
actors: []
dates: []
keywords: ["python", "alpha", "release", "more", "faster", "support", "cpython", "now"]
source: https://blog.python.org/2026/11/python-3160-alpha-2-is-now-available/
source_anchor: ""
source_lines: [1, 53]
section: "Python_PHP"
---
**This is an early developer preview of Python 3.16**

## What’s new in Python 3.16?

Python 3.16.0a2 is ready to try! This is the second of seven planned alpha releases, with new features, bug fixes, and performance improvements to test.

Python 3.16 is still in development. Features can be added until beta 1 on May 4, 2027, and may be modified or removed until the first release candidate on July 27, 2027. The final release is planned for October 5, 2027. This preview is intended for testing and is not recommended for production use.

Here are a few highlights from Python 3.16 so far, compared with 3.15.

- More expressive regular expressions. The `re` module now supports nested character sets, set operations such as intersection and difference, and matching characters by Unicode properties, such as uppercase letters.
- Faster startup. Python starts faster, and several common operations have been optimized, including garbage collection and the construction of built-in objects.
- Faster UUID generation. `uuid4()` and`uuid7()` now have C implementations, making generation about 15-30x faster than the pure Python implementations in microbenchmarks.
- Improved error messages
- Encrypted Client Hello. The `ssl` module adds support for ECH when built with OpenSSL 4.0 or later.
- Build support for more Apple platforms. CPython can now be built for tvOS, watchOS, and visionOS. These platforms are not yet part of the supported platform tiers in PEP 11.

See What’s new in Python 3.16 for more changes.

(Hey, **fellow core team member,** if there’s a feature you’d like highlighted in a future announcement, let Savannah know.)

Please try this alpha with your libraries and applications and report Python bugs on the CPython issue tracker.

The next alpha, Python 3.16.0a3, is scheduled for December 15, 2026. The full schedule is in PEP 826.

## More resources

## And now for something completely different

Next up in the Python 3.16 Film Festival is *The Martian* (2015). Stranded on Mars, Mark Watney has to keep himself alive with what he has on hand. The plan to bring him home takes shape much like CPython does, with people around the world bringing different skills to a shared effort.

So we do this together or not at all.

— Commander Melissa Lewis, *The Martian* (2015)


## Enjoy the new release

Thanks to everyone who contributed to this alpha and to everyone giving it a try. Please consider volunteering your time or asking your organisation to support the Python Software Foundation.

Happy testing!

Regards from Washington state,

Your release team,

Savannah Ostrowski

Ned Deily

Steve Dower

Julien Palard
