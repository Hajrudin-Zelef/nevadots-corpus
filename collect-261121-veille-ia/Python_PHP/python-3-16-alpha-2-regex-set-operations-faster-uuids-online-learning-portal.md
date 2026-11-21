---
id: collect-261121-veille-ia/python-php/python-3-16-alpha-2-regex-set-operations-faster-uuids-online-learning-portal
title: "Python 3.16 Alpha 2: Regex Set Operations & Faster UUIDs - Online Learning Portal"
domain: python-php
role: reference
task: article
actors: []
dates: ["2026-11-10"]
keywords: ["python", "uuid", "alpha", "operations", "uuids", "regular", "faster", "release"]
source: https://onlinelearningportal.website/python-3-16-alpha-2-regex-uuid/
source_anchor: ""
source_lines: [1, 130]
section: "Python_PHP"
---
**Python 3.16.0 alpha 2** was released on 10 November 2026. It is the second of seven planned alphas, so it is a developer preview, not something for production. But it already contains changes you can try today: much more expressive regular expressions, C-accelerated UUIDs, faster startup, a new `stop_exception` option for `iter()` and better error messages. This tutorial shows how to install the alpha safely and test the most useful new features.

## Release timeline at a glance

From the official release page:

- Next alpha (3.16.0a3): 15 December 2026
- Beta 1 (feature freeze): 4 May 2027
- First release candidate: 27 July 2027
- Final release: planned for 5 October 2027

Features can still be added until beta 1, and changed or removed until the RC, so treat everything below as “subject to change”.

## Install the alpha without touching your main Python

The safest way is a separate install plus a throwaway virtual environment. Download the installer or source from python.org, or use a tool such as `pyenv` once it lists 3.16.0a2. Then:

```
python3.16 -m venv ~/venvs/py316
source ~/venvs/py316/bin/activate
python -VV
```
Run your test suite inside that venv. Alpha releases are the best time to find breakages, because bugs reported now can still be fixed before the API is frozen.

## 1. Set operations and nested sets in regular expressions

This is the headline feature. The `re` module now supports set operations inside character classes, following Unicode Technical Standard #18:

- **Difference:**`[A--B]`
- **Intersection:**`[A&&B]`
- **Union:**`[A||B]`

Each operand can be a nested set in square brackets. The example from the docs matches an ASCII lowercase consonant:

```
import re
consonant = re.compile(r"[a-z--[aeiou]]")
print(consonant.findall("regular expressions"))
# every lowercase letter except a, e, i, o, u
```
Before 3.16 you had to write the consonants out by hand, or use a negative lookahead such as `(?![aeiou])[a-z]`. The set syntax is shorter and easier to review.

### Watch out for old patterns

Since Python 3.7, `re` has emitted a `FutureWarning` for patterns that *look like* set operations or nested sets, such as `[a--b]` or `[[a]`. That warning existed precisely so this feature could land. If your codebase has ever produced those warnings, re-test those patterns on 3.16, because they may now mean something different. Escaping the characters (for example `[a\-\-b]`) keeps the old literal meaning.

## 2. Unicode property escapes: \p{…} and \P{…}

You can now match characters by Unicode property, something many developers previously installed the third-party `regex` package for:

```
import re
# \p{Lu} = any uppercase letter, in any script
print(re.findall(r"\p{Lu}", "Hello Ñandú Ωmega"))
# \P{...} is the negation: anything that is NOT ASCII
print(re.findall(r"\P{ASCII}", "café naïve"))
```
Combine both features for precise matching, for example “letters that are not ASCII” with an intersection. Check the regular expression syntax page in the 3.16 docs for the list of supported properties.

## 3. UUIDs are now implemented in C

`uuid.UUID`, `uuid.uuid4()` and `uuid.uuid7()` now have C implementations, with a pure Python fallback. The What’s New page reports these microbenchmark speedups over the pure Python versions:

| Operation | Speedup | 
|---|---|
| `uuid4()` generation | about 15x | 
| `uuid7()` generation | about 30x | 
| `UUID` from a string | about 6.8x | 
| `str()` of a UUID | about 6.7x | 

You can measure it on your own machine:

```
python -m timeit -s "import uuid" "uuid.uuid7()"
python -m timeit -s "import uuid" "str(uuid.uuid4())"
```
Run the same commands on 3.15 to compare. If you generate IDs for database rows or log events at high volume, this is a free win.

**Porting note:** `uuid.UUID` now consistently raises `TypeError` when the `bytes` or `bytes_le` argument is not a `bytes` object. Code that caught `ValueError` or `AssertionError` for that case should be updated.

## 4. iter() can stop on an exception

The two-argument form of `iter(callable, sentinel)` has existed for years. In 3.16 the second parameter is named `stop_value`, and there is a new `stop_exception` parameter: the iterator stops when the callable raises that exception.

```
import queue
q = queue.Queue()
for n in range(3):
    q.put(n)
# Drain the queue without a try/except loop
for item in iter(q.get_nowait, stop_exception=queue.Empty):
    print("got", item)
```
`aiter()` accepts the same `stop_value` and `stop_exception` parameters for async callables.

## 5. Small additions you will actually use

### Milliseconds in strftime with %Nf

```
from datetime import datetime
print(datetime.now().strftime("%H:%M:%S.%3f"))  # e.g. 14:05:09.123
```
`%Nf` (N from 1 to 6) controls how many fractional-second digits you get, and it works in `strptime()` too.

### Half-turn trigonometry

New `math.sinpi()`, `math.cospi()`, `math.tanpi()` and inverse functions such as `math.atan2pi()` work in half turns instead of radians, as recommended by IEEE 754-2019. `math.sinpi(1)` avoids the tiny rounding error you get from `math.sin(math.pi)`.

### Friendlier syntax errors

```
>>> if x > 1 && y < 2: pass
# SyntaxError now suggests 'and' or '&'
```
Keywords from other languages also get hints: `switch x:` suggests `match`, and `function f():` suggests `def`.

## Things that were removed

Several APIs deprecated since 3.14 are gone in 3.16. The ones most likely to affect real code:

- The asyncio event loop **policy system** (`asyncio.set_event_loop_policy()` and friends). Use`asyncio.run()` or`asyncio.Runner` with`loop_factory` .
- `asyncio.iscoroutinefunction()` . Use`inspect.iscoroutinefunction()` .
- The `'u'` format code in`array` . Use`'w'` .

Search your codebase for these names now so the final 3.16 upgrade is boring.

## Key takeaway

Python 3.16.0a2 is worth installing in a separate venv today. Try regex set operations and `\p{...}` escapes, measure the faster UUIDs, and run your test suite to catch removals early. Report anything that breaks on the CPython issue tracker while there is still time to fix it. Full details: What’s New in Python 3.16.
