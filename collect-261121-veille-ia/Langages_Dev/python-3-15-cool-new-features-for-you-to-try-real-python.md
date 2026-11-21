---
id: collect-261121-veille-ia/langages-dev/python-3-15-cool-new-features-for-you-to-try-real-python
title: "Python 3.15: Cool New Features for You to Try – Real Python"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-10-09"]
keywords: ["python", "json", "lazy", "features", "imports", "youll", "import", "need"]
source: https://realpython.com/python315-new-features/
source_anchor: ""
source_lines: [1, 813]
section: "Langages_Dev"
---
Python 3.15 was released on October 9, 2026, so now is a good time to see what the new version has in store for you.

This release trims the time you spend waiting. Imports can wait until you need them, the JIT compiler is faster, and a new profiler shows you where the remaining slow parts hide. There’s also plenty in it for everyday code, including two new built-in types, sharper type hints, and error messages that speak a few other programming languages.

**In this tutorial, you’ll explore features like:**

- **Lazy imports** with the new`lazy` soft keyword
- **`frozendict` and `sentinel`** , two new built-in types
- **`TypeForm` and closed `TypedDict` types** for more precise type hints
- **A sampling profiler** that you can point at a running process
- **Smarter error messages** and a friendlier REPL
- **UTF-8** as the default encoding everywhere
- **An upgraded JIT compiler** with measurable speedups

To try the examples yourself, you’ll need uv. If you don’t have it yet, then follow the installation instructions first. Once you’ve set up uv, you don’t need to install Python 3.15 by hand because uv downloads the interpreter for you. One command drops you into a Python 3.15 REPL:

```
$ uv run --python 3.15 python
```
An older version of uv might fetch a 3.15 pre-release instead, which still runs every example in this tutorial. To get the latest stable build, update uv first with `uv self update` or through the package manager you used to install it.

The sections below are independent, so feel free to jump to the ones that interest you. Most features come with a short example, either a REPL session or a script that you can run with uv, along with its real output. You’ll explore the static typing features through a type checker instead, and a few under-the-hood changes, like the new Windows interpreter and the garbage collector rollback, get a brief summary rather than a demo.

Six of these features—namely lazy imports, `frozendict`, sentinel values, the sampling profiler, UTF-8 by default, and the upgraded JIT compiler—already have their own in-depth tutorials. For those, you’ll get a short overview here and a link to the full tutorial at the end of each section.

**Try It Yourself:** Get the code examples you need to try lazy imports, frozendict, TypeForm, the sampling profiler, and more new features in Python 3.15.

 **Test your knowledge with our interactive “Python 3.15: Cool New Features for You to Try” quiz. You’ll receive a score upon completion to help you track your learning progress:**

**Interactive Quiz**

Test your knowledge of Python 3.15's new features, from lazy imports, frozendict, and sentinels to TypeForm, Tachyon, and UTF-8 by default.

Two changes introduce new syntax this year. One of them, lazy imports, is arguably the headline feature of the whole release. The other is a small comprehension upgrade that you’ll start using without thinking about it. Both are purely additive, so your existing code keeps working unchanged.

Every `import` at the top of a file runs before your first line of code does. For a command-line tool that pulls in heavy dependencies, that can mean a noticeable pause before even `--help` prints. The usual workaround is to bury imports inside functions, which works but scatters your dependencies all over the file.

The new release offers a cleaner fix through PEP 810. A new `lazy` keyword lets you keep your imports at the top of the file while deferring the work of importing until you first use the module:

`lazy_json.py`
    ```
import sys
lazy import json
print("json loaded:", "json" in sys.modules)
config = json.dumps({"theme": "dark", "autosave": True})
print("json loaded:", "json" in sys.modules)
print(config)
```
The `lazy import json` line binds the name but doesn’t load the module. Only the `json.dumps()` call triggers the import:

```
$ uv run --python 3.15 python lazy_json.py
json loaded: False
json loaded: True
{"theme": "dark", "autosave": true}
```
The `from` form works too, as in `lazy from pathlib import Path`. Lazy imports are only allowed at module level, so you can’t use them inside functions, class bodies, or `try` blocks. If you’d rather not edit every file, then the `-X lazy_imports=all` option makes all imports in a program lazy.

Keep in mind that lazy imports move the cost of importing rather than remove it. A module still loads in full the first time you use it, so a program that ends up touching everything it imports pays the same total price, just later, even with `-X lazy_imports=all`. The real win comes with short-lived commands, like those that print `--help` and exit before they ever need most of their heavy dependencies.

Because `lazy` is a soft keyword, existing code that uses it as a variable name keeps working. The main trap is error timing, because a typo in a lazy import only surfaces at first use. Fortunately, the resulting traceback points at both the line that first used the module and the original `lazy import` statement.

**Note:** To learn how lazy imports speed up real command-line tools and when you shouldn’t use them, check out Python Lazy Imports: Speed Up Startup With the lazy Keyword.

Lazy imports change what happens when your program starts. The next feature is much smaller in scope, but you’ll probably use it more often.

Flattening a list of lists used to mean a list comprehension with a double `for` clause or a trip to `itertools.chain()`. The new release lets you use the `*` and `**` unpacking operators directly inside comprehensions:

`Python 3.15`
    ```
>>> daily_temps = [[18.2, 21.7], [17.9], [19.4, 23.1]]
>>> [temp for temps in daily_temps for temp in temps]
[18.2, 21.7, 17.9, 19.4, 23.1]
>>> [*temps for temps in daily_temps]
[18.2, 21.7, 17.9, 19.4, 23.1]
>>> configs = [{"host": "db.local"}, {"port": 5432}]
>>> {**config for config in configs}
{'host': 'db.local', 'port': 5432}
```
The first two comprehensions do the same thing, but the starred version reads the way you’d say it out loud. The `**` form gives dict comprehensions a merging syntax that previously required `functools.reduce()` or a loop.

Generator expressions support the starred form, too. Because you can pass a lone generator expression to a function without extra parentheses, you can feed flattened data straight into a reducing function without building an intermediate list:

`Python 3.15`
    ```
>>> max(*temps for temps in daily_temps)
23.1
>>> sum(*temps for temps in daily_temps)
100.3
>>> from itertools import chain
>>> sum(chain.from_iterable(daily_temps))
100.3
```
The last expression shows the old way of doing the same thing lazily. It still works, of course, but you no longer need an import to express such a common idea.

When you merge dictionaries this way, later keys win, just as they do with the `|` operator. That makes the new syntax a natural fit for layered configuration, where each layer overrides the previous one:

`Python 3.15`
    ```
>>> layers = [
...     {"host": "localhost", "port": 8000},
...     {"port": 5432},
...     {"debug": True},
... ]
>>> {**layer for layer in layers}
{'host': 'localhost', 'port': 5432, 'debug': True}
```
The starred expression has to be the topmost expression in the comprehension, and `**` only makes sense in dict comprehensions. If you try to mix it with other elements or hide it inside parentheses, then the parser stops you with a targeted message:

`Python 3.15`
    ```
>>> [*temps, 0 for temps in daily_temps]
  File "<python-input-0>", line 1
    [*temps, 0 for temps in daily_temps]
     ^^^^^^^^^
SyntaxError: did you forget parentheses around the comprehension target?
>>> [(*temps) for temps in daily_temps]
  File "<python-input-1>", line 1
    [(*temps) for temps in daily_temps]
      ^^^^^^
SyntaxError: cannot use starred expression here
```
Asynchronous comprehensions follow the same rules, so `(*chunk async for chunk in stream())` flattens data as it arrives.

Why did this take so long? The idea is actually older than f-strings. Early drafts of PEP 448, which brought you `[*a, *b]` and `{**a, **b}` back in Python 3.5, allowed unpacking inside comprehensions as well. It “was met with a mix of strong concerns about readability and mild support,” so the authors dropped it to get the rest of the proposal accepted. A decade later, PEP 798 by Adam Hartz and Erik Demaine picked up the leftover piece.

Okay, that covers the new syntax. Next up, you’ll meet two new names that appear in the built-in namespace without an import.

This release adds two built-in types. Both close gaps that Python developers have been working around with idioms for decades, so expect to see them in library code quickly. One freezes dictionaries, and the other gives a proper name to the absence of a value.

Dictionaries have long lacked an immutable counterpart. Sets have `frozenset`, and lists have tuples, but protecting a dictionary required `MappingProxyType` wrappers or discipline. This release adds `frozendict` as a built-in:

`Python 3.15`
    ```
>>> defaults = frozendict({"theme": "light", "autosave": True})
>>> custom = defaults | {"theme": "dark"}
>>> custom
frozendict({'theme': 'dark', 'autosave': True})
>>> custom["theme"] = "sepia"
Traceback (most recent call last):
  ...
TypeError: 'frozendict' object does not support item assignment
>>> custom == {"theme": "dark", "autosave": True}
True
```
Merging with `|` returns a new `frozendict`, and equality with a regular `dict` works the way you’d hope. Because a `frozendict` is hashable when its values are, you can also use one as a dictionary key or pass it to a function decorated with `@lru_cache`, which rejects regular dictionaries.

Note that `frozendict` isn’t a `dict` subclass. It implements the `Mapping` interface but inherits straight from `object`, so code that explicitly checks `isinstance(x, dict)` won’t accept it.

**Note:** To learn how to construct, merge, and hash frozen dictionaries and why the new type doesn’t subclass `dict`, check out Python’s frozendict: The Immutable Dictionary.

The second new built-in solves a different problem that’s just as old: how to distinguish a missing value from `None`.

When `None` is a meaningful value, functions need a different way to indicate that the caller didn’t pass anything. The traditional fix is a module-level `object()`, but it prints as a meaningless memory address and loses its identity when pickled. The new `sentinel` built-in solves both problems:

`Python 3.15`
    ```
>>> MISSING = sentinel("MISSING")
>>> MISSING
MISSING
>>> def get_option(options, name, default=MISSING):
...     value = options.get(name, default)
...     if value is MISSING:
...         raise KeyError(name)
...     return value
...
>>> print(get_option({"retries": None}, "retries"))
None
>>> print(get_option({}, "timeout", default=None))
None
>>> get_option({}, "timeout")
Traceback (most recent call last):
  ...
KeyError: 'timeout'
```
The last two calls look up the same missing key. Passing `default=None` explicitly gets you `None` back, while leaving the argument out raises a `KeyError`. If the function used `None` as its default, then it couldn’t tell those two calls apart.

Sentinels are truthy, compare equal only to themselves, and keep their identity when copied. As long as you define one at module level under a matching name, it also survives pickling, so the `is` check stays reliable even across process boundaries. Sentinels work well with type hints because `int | MISSING` produces a regular union that type checkers narrow with `is` comparisons.

**Note:** To see more patterns for sentinel values, including how they compare to the old idioms, check out Python Sentinel Values: Using the New sentinel Built-in.

With the new built-in types covered, it’s time to look at two additions that matter mostly to your type checker.

Python’s type system keeps growing, and this release adds two features that library authors have requested for years. Neither changes how your code runs. Instead, both let you describe existing runtime behavior precisely enough for a static type checker to follow along.

The examples here assume you’re comfortable with type hints, including generic functions. If static typing isn’t part of your workflow, then feel free to skip ahead to the developer experience improvements.

Some functions take a type as an argument and return a value of that type. Think of `typing.cast()`, a validation library that parses JSON into a given shape, or a dependency injection container. For a plain class, `type[T]` has always done the job. But the moment you pass `int | None` or `list[int]`, it falls apart because those aren’t classes. They’re **type expressions**, which `type[T]` can’t describe.

PEP 747 adds `typing.TypeForm` to fill that gap. To see the difference, compare two versions of the same hypothetical parsing function.

Both use the type parameter syntax from Python 3.12, where `[T]` after the function name links the type that you pass in to the type that you get back. The bodies are just an ellipsis (`...`) because only the signatures matter to a type checker, so this isn’t a working parser. The example wraps each call in `reveal_type()`, which asks the type checker to report the type that it infers:

`typeform_check.py`
    ```
from typing import TypeForm, reveal_type
def parse_old[T](value: str, typ: type[T]) -> T: ...
def parse_new[T](value: str, typ: TypeForm[T]) -> T: ...
reveal_type(parse_old("42", int))
reveal_type(parse_old("42", int | None))
reveal_type(parse_new("42", int | None))
reveal_type(parse_new("42", list[int]))
parse_new("42", 42)
```
When you run the Pyright type checker against this file, it reports what it thinks each call returns. The command below uses `uvx`, uv’s tool runner, which ships with uv and runs a command-line tool like Pyright without you having to install it:

```
$ uvx pyright@1.1.414 --pythonversion 3.15 typeform_check.py
typeform_check.py:7:13 - information: Type of "parse_old("42", int)" is "int"
typeform_check.py:8:13 - information: Type of "parse_old("42", int | None)" is "Unknown"
typeform_check.py:8:29 - error: Argument of type "UnionType" cannot be
⮑ assigned to parameter "typ" of type "type[T@parse_old]" in function "parse_old"
    Type "UnionType" is not assignable to type "type[T@parse_old]" (reportArgumentType)
typeform_check.py:9:13 - information: Type of "parse_new("42", int | None)" is "int | None"
typeform_check.py:10:13 - information: Type of "parse_new("42", list[int])" is "list[int]"
typeform_check.py:11:17 - error: Argument of type "Literal[42]" cannot be
⮑ assigned to parameter "typ" of type "TypeForm[T@parse_new]" in function "parse_new"
    "Literal[42]" is not assignable to "TypeForm[T@parse_new]" (reportArgumentType)
2 errors, 0 warnings, 4 informations
```
The old signature handles `int` but chokes on the union, leaving you with an error and an unknown return type. The new signature accepts both the union and the parameterized list, infers the matching return types, and still rejects the number `42`, which isn’t a type at all.

The same distinction matters when you store a type expression in a variable. On its own, `schema = int | None` gives your type checker an ordinary `UnionType` object. To keep it usable as a type form, either annotate the variable, as in `schema: TypeForm[int | None] = int | None`, or wrap the value in a call, as in `schema = TypeForm(int | None)`. At runtime, `TypeForm(x)` just returns `x` unchanged, so the payoff comes entirely from your type checker.

A `TypedDict` describes a dictionary with known keys, but until now it couldn’t say anything about keys it didn’t list. Type checkers had to guess whether extra keys were allowed. PEP 728 lets you decide explicitly with two new class arguments:

`typeddict.py`
    ```
from typing import TypedDict
class Movie(TypedDict, closed=True):
    title: str
    year: int
class Headers(TypedDict, extra_items=str):
    content_type: str
movie: Movie = {"title": "Brazil", "year": 1985, "rating": 8.0}
headers: Headers = {"content_type": "text/html", "x-trace-id": "abc123"}
bad_headers: Headers = {"content_type": "text/html", "x-retries": 3}
print(Movie.__closed__, Headers.__extra_items__)
```
A *closed* `TypedDict` forbids any keys beyond the ones in its body, which makes `Movie` behave like a record with a fixed shape. The `extra_items` argument goes the other way. It allows arbitrary additional keys but constrains their values to a single type, which is exactly how you’d describe HTTP headers or environment variables.

Running the script prints `True <class 'str'>` without raising any errors, since Python itself doesn’t validate dictionaries against these declarations. The checking happens statically:

```
$ uvx pyright@1.1.414 --pythonversion 3.15 typeddict.py
typeddict.py:10:50 - error: Type "dict[str, str | int | float]" is not assignable
⮑ to declared type "Movie"
    Type "float" is not assignable to item "extra_items"
      Type "float" is not assignable to type "Never" (reportAssignmentType)
typeddict.py:12:67 - error: Type "dict[str, str | int]" is not assignable
⮑ to declared type "Headers"
    "Literal[3]" is not assignable to "str" (reportAssignmentType)
2 errors, 0 warnings, 0 informations
```
Notice how Pyright explains the closed dictionary. Behind the scenes, `closed=True` is shorthand for `extra_items=Never`, so the stray `rating` key fails because no value can be of type `Never`. The `x-trace-id` header passes, while `x-retries` fails because its value isn’t a string.

Tool support is still catching up. At the time of writing, Pyright 1.1.414 understands both features, while mypy 2.3.1 still rejects the `extra_items` argument.

Much of this release focuses on the time between writing code and understanding it. Profiling, error messages, the REPL, terminal output, and text encodings all received attention. Some of these changes are big, while others are small touches that you’ll only notice once they save you time.

Finding slow code used to force you to choose. You could run `cProfile` and accept the overhead it adds to every function call, or install a third-party sampling tool like py-spy. The standard library now ships **Tachyon** in the new `profiling` package. It’s a statistical profiler that samples the stack from outside the process.

To see it in action, you can profile `wordstats.py`, a short script included in this tutorial’s downloadable materials. The script counts the words in a large sample text and prints the three most frequent ones. With the script in your current directory, run it under the profiler:

```
$ uv run --python 3.15 python -m profiling.sampling run wordstats.py
[('the', 200000), ('quick', 100000), ('brown', 100000)]
Captured 346 samples in 0.35 seconds
Sample rate: 1000.00 samples/sec
Error rate: 3.76
Profile Stats:
       nsamples   sample%  tottime (ms)    cumul%  cumtime (ms)  filename:lineno(function)
        242/242      72.7       242.000      72.7       242.000  wordstats.py:4(most_common_words)
          71/71      21.3        71.000      21.3        71.000  __init__.py:707(Counter.update)
         13/328       3.9        13.000      98.5       328.000  wordstats.py:8(<module>)
(...)
```
Instead of hooking into every function call as a tracing profiler does, Tachyon peeks at the target’s call stack at regular intervals, a thousand times per second by default, and counts where it finds the program. The trade-off is a statistical rather than exact picture, but the target process runs at nearly full speed. That’s also why your sample counts and timings will differ a little from run to run.

Each row of the report shows two views of the same function. The `sample%` and `tottime` columns count only the samples that caught the function running its own code, while `cumul%` and `cumtime` also include time spent in the functions that it called.

Here, `most_common_words()` is the clear hotspot. It accounts for 72.7 percent of all samples in its own code, so that’s the function to investigate first. The `<module>` row looks alarming at 98.5 percent cumulative, but only 3.9 percent is its own time. Almost every sample passes through the script’s top-level code on the way to somewhere else, so a high `cumul%` paired with a low `sample%` points to a caller, not to the slow code itself.

Because it works from the outside, Tachyon can also attach to a program that’s already running, show a live `top`-like view with `--live`, and export interactive flame graphs.

The old deterministic profiler moved to `profiling.tracing`, with `cProfile` kept as an alias, while the pure-Python `profile` module is deprecated. Under the hood, CPython now also compiles with frame pointers enabled by default, which helps native tools like `perf` produce complete stack traces.

**Note:** For flame graphs, async-aware profiling, and attaching to production processes, check out Python’s Sampling Profiler: Low-Overhead Profiling With profiling.sampling.

Finding slow code is one part of understanding a program. Figuring out why it crashed is another, and this release helps with that, too.

The interpreter keeps getting better at guessing what you meant, continuing a trend of friendlier error messages from recent releases. In 3.15, `AttributeError` looks deeper before giving up. It can now spot an attribute that lives one level down:

`Python 3.15`
    ```
>>> class Customer:
...     def __init__(self):
...         self.email = "ada@example.com"
...
>>> class Invoice:
...     def __init__(self):
...         self.customer = Customer()
...
>>> Invoice().email
Traceback (most recent call last):
  ...
AttributeError: 'Invoice' object has no attribute 'email'.
⮑ Did you mean '.customer.email' instead of '.email'?
```
`AttributeError` also recognizes vocabulary from other languages. When the usual spelling-based search comes up empty on a built-in type, the interpreter consults a lookup table of method names that trip up people coming from JavaScript, Java, C#, and Ruby:

`Python 3.15`
    ```
>>> ratings = [4.5, 3.8]
>>> ratings.push(4.9)
Traceback (most recent call last):
  ...
AttributeError: 'list' object has no attribute 'push'. Did you mean '.append'?
>>> inventory = {}
>>> inventory.put("apples", 12)
Traceback (most recent call last):
  ...
AttributeError: 'dict' object has no attribute 'put'. Use d[k] = v.
```
Sometimes the right answer isn’t a method at all, so the interpreter describes the construct you need. The same table also catches mutating methods called on immutable types, such as `.append()` on a tuple, and suggests the mutable counterpart.

The following widget lets you try every entry in the table and compare the message you’d get in Python 3.14 with the one in Python 3.15:

Each entry had to meet two criteria. There had to be evidence of real confusion, like Stack Overflow traffic or bug reports, and the wrong name had to differ too much from the right one for the regular spelling-based suggestions to catch it.

Fittingly, the lookup table in `Lib/traceback.py` is itself a `frozendict`. Besides method names, it covers bitwise operators on floats, but those hints are easy to miss. If you call a special method like `.__or__()` directly, as in `(1.5).__or__(2)`, then the error suggests using an `int` instead. The everyday `1.5 | 2` still raises a plain `TypeError` because the operator never raises an `AttributeError` for the hint to attach to.

Import error messages are smarter, too. Where Python 3.14 would only tell you that the module doesn’t exist, a misspelled module name now gets the same kind of suggestion that misspelled variables have had for years:

`Python 3.15`
    ```
>>> import pathilb
Traceback (most recent call last):
  ...
ModuleNotFoundError: No module named 'pathilb'. Did you mean: 'pathlib'?
```
If Python finds an extension module built for a different Python version, then the message says that, too, which helps when a stale compiled wheel lingers in a virtual environment.

On the syntax side, several vague `invalid syntax` errors now explain themselves. For example, a stray `case` without its `match` block gets a proper diagnosis:

`Python 3.15`
    ```
>>> case 42: pass
  File "<python-input-0>", line 1
    case 42: pass
    ^^^^^^^^
SyntaxError: case statement must be inside match statement
>>> with open("a.txt") as a, open("b.txt") as b,:
  File "<python-input-1>", line 1
    with open("a.txt") as a, open("b.txt") as b,:
                                               ^
SyntaxError: the last 'with' item has a trailing comma
```
Finally, `argparse` now suggests close matches by default when a user mistypes a choice or subcommand in your command-line interface, as in `invalid choice: 'zstdd', maybe you meant 'zstd'?`. In Python 3.14, you had to opt in with `suggest_on_error=True`.

The new REPL that arrived in Python 3.13 keeps collecting small conveniences. The most visible one in 3.15 is tab completion that understands what you’re typing and colors each suggestion by its kind:

In the clip above, pressing `Tab` twice after `from collections import` lists the names that the module defines. Python 3.14 could only complete submodule names in that spot, so it offered nothing more than `abc`. The completion menu also paints classes, functions, and modules in different colors, which is an idea borrowed from the third-party fancycompleter project. If you prefer the old look, then set the `PYTHON_BASIC_COMPLETER` environment variable.

The REPL now also catches a mistake that almost every beginner makes at least once, typing a shell command at the Python prompt:

`Python 3.15`
    ```
>>> pip install requests
  File "<python-input-0>", line 1
    pip install requests
        ^^^^^^^
SyntaxError: invalid syntax
The Python package manager (pip) can only be used outside of the Python REPL.
Try the 'pip' command in a separate terminal or command prompt.
```
The extra note tells a newcomer exactly what went wrong instead of leaving them to decode `invalid syntax`. The same attention to detail shows up elsewhere:

- Soft keywords like `lazy` and`type` get highlighted when they act as keywords.
- The built-in `help()` function has new topics for`lazy` ,`match` , and`case` .
- The `pdb` module, Python’s built-in debugger, now reads your commands through the new REPL, so the`(Pdb)` prompt gets the same editing and highlighting features.

None of these will change how you write code, but together they make the REPL a more pleasant place to try out the examples in the rest of this tutorial.

Python 3.13 brought color to tracebacks and the REPL prompt, and Python 3.14 added syntax highlighting to the REPL along with color in several command-line tools. This release spreads it around. Help text from `argparse` now prints with syntax-aware colors, as does help text from `python` itself and nearly twenty standard-library command-line tools.

The `sqlite3` command-line shell gets colored prompts plus tab completion for SQL keywords and your own table and column names.

To see the new colors for yourself, print Python’s own help, display a calendar for this month, or start the SQLite shell:

```
$ uv run --python 3.15 python --help
$ uv run --python 3.15 python -m calendar 2026 9
$ uv run --python 3.15 python -m sqlite3
```
The calendar, which started highlighting today’s date in Python 3.14, now colors its month and weekday headers as well. The `http.server` module colors its log lines by response status, and the `timeit`, `tokenize`, `pickletools`, and `ast` command-line tools all got a splash of color.

Additionally, two library functions gained an opt-in `color` parameter. The `difflib.unified_diff()` function can now produce output that looks like `git diff`, and `ast.dump()` can highlight the syntax trees it prints.

Colors respect the usual conventions everywhere, so setting `NO_COLOR=1` or `PYTHON_COLORS=0` turns everything back to plain text, and piped output stays clean automatically.

For years, `open("notes.txt")` did different things on different machines. Without an explicit `encoding` argument, Python used the operating system’s locale, which meant UTF-8 on most Linux and macOS systems and a legacy code page on many Windows setups. The new version flips the switch that ends this guessing game. UTF-8 Mode is now on by default, everywhere.

To see what that fixes, grab `menu.txt` from this tutorial’s downloadable materials. It contains a single line saved as UTF-8:

`menu.txt`
    ```
Crème brûlée — 7 €
```
The sessions below read that file on a machine configured with the legacy Latin-1 locale, `en_US.ISO-8859-1`. Most Linux and macOS systems use a UTF-8 locale, so you won’t see the problem there, but you can reproduce it with any Python version by spelling out the legacy encoding in `open("menu.txt", encoding="latin-1").read()`.

Here’s what Python 3.14 gives you on the Latin-1 machine:

`Python 3.14`
    ```
>>> import locale
>>> locale.getpreferredencoding()
'ISO-8859-1'
>>> open("menu.txt").read()
'CrÃ¨me brÃ»lÃ©e â\x80\x94 7 â\x82¬\n'
```
There’s no error, just silently garbled text, which is worse than a crash because nothing tells you that something went wrong. Now, repeat the experiment with the new version on the same machine:

`Python 3.15`
    ```
>>> import locale
>>> locale.getpreferredencoding()
'utf-8'
>>> open("menu.txt").read()
'Crème brûlée — 7 €\n'
```
The locale no longer matters. If you need the old behavior, then pass `encoding="locale"` for a single call, or set `PYTHONUTF8=0` for the whole interpreter. Code that must behave identically on 3.14 and 3.15 should keep passing `encoding=` explicitly.

Keep in mind that the new default changes how Python reads and writes text, not the files already on your disk. If your program saved files under a legacy locale, such as `cp1252` on many Windows setups, then Python 3.15 will now try to decode them as UTF-8, which usually ends in a `UnicodeDecodeError`.

Before upgrading, find out which encoding those files use and either pass it explicitly, as in `open("report.txt", encoding="cp1252")`, or convert the files to UTF-8 once.

**Note:** To find out what this change means for your existing code and how to write files that behave the same on every version, check out Python’s UTF-8 Default: Keep Your File I/O Portable.

With the developer experience covered, it’s time to look at what makes the new version faster.

The performance work in 3.15 concentrates on the just-in-time compiler, and the numbers are starting to justify the multiyear investment. Windows users get a separate boost, while two efforts from last year’s release took different turns. None of these changes require you to modify your code.

The JIT that shipped experimentally in Python 3.13 has matured in this release, though it’s still experimental and off by default. The biggest change is a new **tracing frontend** by Ken Jin, which records the paths that your code actually takes and optimizes those instead of estimating them. Together with basic register allocation, more constant propagation, and fewer reference-count updates, this lets the JIT speed up far more code than before.

On the official benchmark suite, the core team reports geometric mean speedups of 7 to 8 percent over the standard interpreter on x86-64 Linux and 11 to 12 percent over the tail-calling interpreter on AArch64 macOS.

Whether your interpreter includes the JIT at all depends on how it was built, so start by checking that. The first command below tells you whether the JIT is compiled in, and the second tells you whether it’s currently switched on:

```
$ uv run --python 3.15 python -c "import sys; print(sys._jit.is_available())"
True
$ uv run --python 3.15 python -c "import sys; print(sys._jit.is_enabled())"
False
```
If `is_available()` prints `False`, then your build doesn’t include the JIT, and no setting will turn it on. You’ll need a JIT-capable build first. The python.org installers for Windows and macOS include the JIT, and Python’s JIT Compiler: What’s New in Python 3.15 shows you how to build it yourself.

With a JIT-capable build, you can switch the JIT on for a single run by setting the `PYTHON_JIT` environment variable, without rebuilding anything:

The JIT stays off by default because it doesn’t help every workload yet, so measure your own program before you enable it in production.

**Note:** To build a JIT-enabled Python yourself and benchmark it on your own workload, check out Python’s JIT Compiler: What’s New in Python 3.15.

The JIT isn’t the only source of speed in this release, especially if you’re on Windows.

Python 3.14 introduced a new way of building the interpreter’s main loop, where each bytecode instruction jumps straight to the next one through a tail call. It needed a recent Clang compiler, which left the official Windows builds out.

This time around, the python.org 64-bit Windows installers are compiled with Visual Studio 2026, whose compiler supports the same trick. The docs report a 15 to 20 percent speedup over the old switch-based interpreter on the standard benchmarks and between 14 and 40 percent on real-world pure-Python code. You don’t have to do anything to get it other than install the new version on Windows.

The free-threaded build, which runs without the GIL, keeps its status from Python 3.14. It’s officially supported but optional, and you have to install it separately. The work in 3.15 focuses on making it practical for package maintainers rather than on flipping any defaults.

The most important piece is a new **stable ABI for free-threaded builds**, called `abi3t`, from PEP 803. Until now, a C extension had to ship a separate compiled wheel for every free-threaded Python version. With `abi3t`, one build can serve many versions, which should make free-threading support much less of a burden once build tools catch up. The new `sys.abi_info` namespace tells you which kind of interpreter you’re running:

`Python 3.15`
    ```
>>> import sys
>>> sys.abi_info
namespace(pointer_bits=64, free_threaded=False, debug=False, byteorder='little')
```
The `free_threaded=False` field tells you that this is a regular build with the GIL. Previously, you had to piece such details together from scattered sources, like `sysconfig` variables, `sys.maxsize`, and `sys.byteorder`.

The standard library also added helpers for sharing iterators between threads. A generator can’t be advanced by two threads at once, so `threading.synchronized_iterator()` wraps one with a lock, and `threading.concurrent_tee()` splits a single iterator into several thread-safe copies. On the memory side, free-threaded builds now use the mimalloc allocator for raw allocations, too.

Last year’s roundup of new features in Python 3.14 told you about a new incremental garbage collector, which promised shorter pauses on large heaps. It didn’t survive contact with production. After reports of significant memory pressure, the core team reverted to Python 3.13’s generational collector, starting with Python 3.14.5.

If this sounds familiar, then that’s because it’s the second retreat. The incremental collector was already rolled back late in the 3.13 release cycle before it shipped in Python 3.14. Garbage collection is one of those areas where a change that looks great on benchmarks can behave very differently under real workloads.

The 3.15 release ships with the same generational collector. If you tuned your application around the incremental collector’s behavior on an early 3.14 release, then it’s worth measuring its memory usage again.

The headline features aren’t the whole story. Here’s a quick tour of smaller changes you might otherwise miss. Some introduce new functions, while others remove long-standing annoyances or clear out old code. You can try most of the new functions and language tweaks in the same 3.15 REPL. Check the deprecations and removals at the end against your existing code.

Here are the standard-library highlights, grouped by what they help with. You’ll start with numeric helpers in the `math` module, move on to text and data handling in `re`, `json`, `tomllib`, `pprint`, and `unicodedata`, and finish with new tricks for `bytearray` and `Counter`. A short list of smaller additions that don’t need a demo wraps up the tour.

The `math` module has always focused on floating-point numbers, which made `math.factorial()` and friends feel out of place. The new release gives the integer-only functions their own `math.integer` submodule, while the old names keep working as aliases:

`Python 3.15`
    ```
>>> import math.integer
>>> math.integer.isqrt(2_000_000)
1414
```
New code can now make its intent explicit by importing from the integer namespace.

Floating-point math got new helpers, too. The `math.fmax()` and `math.fmin()` functions follow the C99 rule of ignoring NaN when one argument is a real number, whereas the built-in `max()` can leak the NaN because every comparison with NaN is false. Meanwhile, `math.signbit()` finally distinguishes `-0.0` from `0.0`:

`Python 3.15`
    ```
>>> import math
>>> max(float("nan"), 3.5)
nan
>>> math.fmax(float("nan"), 3.5)
3.5
>>> math.signbit(-0.0)
True
```
The built-in `max()` returns `nan` here only because NaN comes first. Swap the two arguments, and you’ll get `3.5`, which is exactly the kind of inconsistency that `math.fmax()` avoids.

Text and data handling got plenty of attention, too. Python’s `re.match()` anchors at the start of the string, which surprises people coming from languages where “match” means “match anywhere.” The new `re.prefixmatch()` function says exactly what it does:

`Python 3.15`
    ```
>>> import re
>>> re.prefixmatch(r"\d{4}", "2026-09-01").group()
'2026'
```
The old `re.match()` function is now soft deprecated. It’ll never be removed, but the documentation steers you toward the more explicit name.

The `json.loads()` function gained a new `array_hook` parameter, which lets you deserialize JSON arrays into something other than lists:

`Python 3.15`
    ```
>>> import json
>>> json.loads("[1, 2, 3]", array_hook=tuple)
(1, 2, 3)
```
Every array in the document becomes a tuple, no matter how deeply it’s nested.

The `tomllib` module now supports TOML 1.1, which allows trailing commas and newlines in inline tables, two of the most requested changes to the format:

`Python 3.15`
    ```
>>> import tomllib
>>> tomllib.loads("point = { x = 1, y = 2, }")
{'point': {'x': 1, 'y': 2}}
```
In Python 3.14, the trailing comma after `y = 2` would have raised a `TOMLDecodeError`. The same update also makes seconds optional in time values and adds a few new string escapes.

The `pprint` module for pretty printing data structures accepts a new `expand` argument. When a container doesn’t fit on one line, `expand=True` opens it up with one item per line, a trailing comma, and the closing bracket on its own line:

`Python 3.15`
    ```
>>> from pprint import pp
>>> config = {
...     "name": "backup",
...     "targets": ["/home", "/etc"],
...     "schedule": {"hour": 3, "days": ["mon", "thu"]},
... }
>>> pp(config, expand=True, indent=4)
{
    'name': 'backup',
    'targets': ['/home', '/etc'],
    'schedule': {'hour': 3, 'days': ['mon', 'thu']},
}
```
The top-level dictionary opens up, while the nested values stay on one line each because they fit within the default width of 80 characters. With a narrower `width`, such as `width=30`, the nested `schedule` dictionary would open up the same way.

A single emoji or flag can consist of several code points, so `len()` often disagrees with what you see on screen. The new `unicodedata.iter_graphemes()` function splits text into user-perceived characters, following the Unicode rules for text segmentation:

`Python 3.15`
    ```
>>> import unicodedata
>>> text = "Hi 🇵🇱!"
>>> len(text)
6
>>> [str(segment) for segment in unicodedata.iter_graphemes(text)]
['H', 'i', ' ', '🇵🇱', '!']
```
The Polish flag takes two code points but counts as one grapheme. The Unicode database itself moves to version 17.0, and a new `unicodedata.block()` function tells you which Unicode block a character belongs to.

Finally, two familiar containers have new capabilities. Network code often builds messages in a mutable buffer and then needs immutable `bytes`. The new `bytearray.take_bytes()` method removes data from the front of the buffer and returns it without making a copy:

`Python 3.15`
    ```
>>> buffer = bytearray(b"HTTP/1.1 200 OK\r\nrest")
>>> buffer.take_bytes(15)
b'HTTP/1.1 200 OK'
>>> buffer
bytearray(b'\r\nrest')
```
Notice that the buffer keeps only the bytes you didn’t take, so a protocol parser can peel off one message at a time.

The `collections.Counter` class now supports the `^` operator for symmetric difference. It keeps the absolute difference of each count and drops the counts that match:

`Python 3.15`
    ```
>>> from collections import Counter
>>> monday = Counter(coffee=3, tea=1)
>>> tuesday = Counter(coffee=1, tea=1, juice=2)
>>> monday ^ tuesday
Counter({'coffee': 2, 'juice': 2})
```
Tea disappears from the result because its count is the same on both days.

A few more additions don’t need a demo:

- **Early exit from task groups** :`asyncio.TaskGroup` gained a`.cancel()` method, so you can cleanly stop all remaining tasks in a group, for example, once you have enough results.
- **Subnormal float checks** : The new`math.isnormal()` and`math.issubnormal()` functions spot the tiny floats that sit below the normal range, like`5e-324` .
- **ISO date shorthand** : The`datetime.strptime()` method understands the`%F` directive for ISO dates, as in`"2026-10-01"` .
- **Unpadded Base64** : The`base64` functions accept`padded=False` for encodings without trailing`=` signs.
- **Custom HTTP headers** : The`python -m http.server` command can send custom response headers with`-H` , which is handy when testing CORS locally.

Each of these removes a small workaround that you may have written yourself.

Structural pattern matching has long accepted negative literals like `case -1:`, and now the unary plus works too, which makes symmetric cases read naturally:

`Python 3.15`
    ```
>>> def classify(delta):
...     match delta:
...         case +1:
...             return "up"
...         case -1:
...             return "down"
...         case 0:
...             return "flat"
...
>>> classify(1), classify(-1), classify(0)
('up', 'down', 'flat')
```
In Python 3.14, the `case +1:` line raises a syntax error, so you have to settle for a plain `case 1:` next to its negative counterpart.

You can use any `@contextmanager` function as a decorator. However, if you decorated a generator function with it in earlier versions, then the context closed as soon as the generator object was created, before your loop even started. To see the difference, decorate a generator function that yields two lines of text with a context manager that logs when it enters and exits:

`ctxmgr.py`
    ```
from contextlib import contextmanager
@contextmanager
def logged(label):
    print(f"enter {label}")
    yield
    print(f"exit {label}")
@logged("reading")
def read_lines():
    yield "first"
    yield "second"
for line in read_lines():
    print(line)
```
Running the script under both versions reveals the change in when the context exits:

```
$ uv run --python 3.14 python ctxmgr.py
enter reading
exit reading
first
second
$ uv run --python 3.15 python ctxmgr.py
enter reading
first
second
exit reading
```
The new version detects generator functions, coroutine functions, and async generator functions, and keeps the context open for as long as they run. That’s what most people expected in the first place, but if your code happened to rely on the old behavior, then this is a change to watch for.

A few other language-level changes are worth knowing about as well:

- **Generic slices** : The`slice` type now supports subscription, so an API that accepts slices can annotate them as precisely as`slice[int, int]` instead of a bare`slice` .
- **Disjoint base classes** : The new`@typing.disjoint_base` decorator from PEP 800 tells type checkers when two classes can’t share a subclass, mirroring a rule that CPython already enforces for built-in types. For example,`class Both(int, str)` fails at runtime with a layout conflict. The decorator mainly matters to authors of C extensions and type stubs because it lets checkers catch such combinations before your code runs.
- **Generator state** : Generator objects gained a`.gi_state` attribute, with`.cr_state` and`.ag_state` as the counterparts for coroutines and async generators. It tells you whether a generator has been started, is suspended, is running, or has finished, without calling`inspect.getgeneratorstate()` .
- **Auditable startup code** : Packages have long been able to run arbitrary code at interpreter startup through`import` lines in`.pth` files. PEP 829 introduces`.start` files, which list explicit entry points like`pkg.mod:callable` instead, and the`import` lines in`.pth` files are now deprecated. The deprecation is silent for now, so you won’t see a warning when a`.pth` file still uses them.

The last change matters most to package authors, but it’s also good news for anyone who wants to know exactly what runs when Python starts.

Every release clears out some old code, and 3.15 is no exception. None of the deprecations or removals below should surprise you if you’ve been paying attention to deprecation warnings:

- The `profile` module is deprecated in favor of`profiling.tracing` and will be removed in Python 3.17.
- Per-module `__version__` attributes are deprecated across two dozen standard-library modules. Use`sys.version_info` instead.
- The `sre_compile` ,`sre_constants` , and`sre_parse` modules are gone.
- The CGI handler in `http.server` and its`--cgi` flag have been removed.
- The `platform.java_ver()` function, which only ever made sense on Jython, has been removed.
- The keyword-argument form of `typing.NamedTuple()` , as in`NamedTuple("Point", x=int, y=int)` , no longer works. Use the class syntax instead.
- The `typing.ByteString` and`collections.abc.ByteString` classes are scheduled for removal in Python 3.17.

If your project runs its test suite with warnings enabled, then you’ve probably seen most of these coming already.

Before you decide whether to upgrade, it’s worth seeing how the new features combine in practice. The script below applies a user’s settings over a set of defaults and saves the result to a JSON file. This small chore uses several of this release’s features at once:

`python315_in_15_lines.py`
    ```
import sys
from pathlib import Path
lazy import json
defaults = frozendict({"theme": "light", "autosave": True})
MISSING = sentinel("MISSING")
def resolve(overrides, name, default=MISSING):
    value = (defaults | overrides).get(name, default)
    return "unset" if value is MISSING else value
profiles = [{"theme": "crème brûlée"}, {"autosave": False}]
merged = {**profile for profile in profiles}
print("json imported:", "json" in sys.modules)
Path("user.json").write_text(json.dumps(defaults | merged, ensure_ascii=False))
print("json imported:", "json" in sys.modules)
print("theme:", resolve(merged, "theme"), "| font:", resolve(merged, "font"))
print(Path("user.json").read_text())
```
It uses a lazy import, a frozen dictionary, a sentinel, dictionary unpacking in a comprehension, and UTF-8 file I/O without a single `encoding` argument. Here’s what it prints:

```
$ uv run --python 3.15 python python315_in_15_lines.py
json imported: False
json imported: True
theme: crème brûlée | font: unset
{"theme": "crème brûlée", "autosave": false}
```
Notice that `json` stays unloaded until the script first needs it to write `user.json`. The accented characters also survive the round trip through that file, even though neither `.write_text()` nor `.read_text()` specifies an encoding.

Now that you’ve seen the features working together, it’s time to decide when to bring them into your own projects.

If you’re already on 3.14, then the jump is small. Nothing headline-grabbing was removed, the new syntax is purely additive, and the UTF-8 change only affects code that silently depended on a legacy locale. Running your test suite on 3.15 should take an afternoon.

If your project uses uv, then you can run its tests on 3.15 without touching your regular environment. The `--isolated` flag builds a throwaway environment with your project’s dependencies, and `-W error::DeprecationWarning` turns deprecation warnings into test failures so that nothing slips by:

```
$ uv run --isolated --python 3.15 pytest -W error::DeprecationWarning
```
This assumes that pytest is one of your project’s development dependencies. With another test runner, keep the `-W` flag and swap the command, as in `python -W error::DeprecationWarning -m unittest`. For a project that doesn’t use uv, create a fresh 3.15 virtual environment, install your dependencies and test tools into it, and run your test runner there with the same `-W` flag.

That said, a handful of changes can break existing code in subtle ways. Expand the section below to see the ones most likely to affect you:

The first gotcha affects SQLite users. All parameters of `sqlite3.connect()` except the database path are now keyword-only:

`Python 3.15`
    ```
>>> import sqlite3
>>> sqlite3.connect(":memory:", 5.0)
Traceback (most recent call last):
  ...
TypeError: connect() takes at most 1 positional arguments (2 given)
```
Passing `timeout=5.0` by name works on every Python version.

Date parsing got stricter, too. Parsing a day of the month without a year now raises an error because the result would be ambiguous around leap days:

`Python 3.15`
    ```
>>> from datetime import datetime
>>> datetime.strptime("Feb 29", "%b %d")
Traceback (most recent call last):
  ...
ValueError: Day of month directive '%d' may not be used without a year directive.
⮑ Parsing dates involving a day of month without a year is ambiguous and fails
⮑ to parse leap day. Add a year to the input and format.
⮑ See https://github.com/python/cpython/issues/70647.
```
This has been deprecated since Python 3.13, so you may have seen the warning already.

A few other changes are less visible but can still surprise you:

- Text files that an earlier Python version wrote under a legacy locale are now read as UTF-8 unless you pass `encoding=` explicitly, as described in the UTF-8 section.
- `argparse` infers`dest` from a single-dash long option, so`.add_argument("-f", "-foo")` now stores its value under`foo` instead of`f` .
- `unittest` ’s`.assertWarns()` no longer swallows unrelated warnings, so previously hidden warnings may start showing up in your test output.
- `base64.urlsafe_b64decode()` no longer requires padding unless you pass`padded=True` .
- Generator functions decorated with `@contextmanager` keep their context open while they run, as described earlier.

If your test suite passes on the new version with warnings enabled, then you’ve most likely dodged all of these.

For anything you deploy, the usual advice holds. Wait for the ecosystem to catch up first. Compiled dependencies need wheels for the new version, and those tend to appear in the weeks after the final release. If you run Python in production at any scale, then waiting for the first maintenance release, 3.15.1, remains the boring and correct choice.

For your own tools, scripts, and new projects, there’s little reason to hold back. Lazy imports alone can make a sluggish CLI feel new again, and the profiler is the kind of tool you’ll wish you’d had on the day something gets slow.

In short, play with it today, upgrade your projects when your dependencies say yes, and let servers wait for 3.15.1.

Python 3.15 makes the language quicker to start, quicker to run, and easier to debug. Hundreds of people contributed to these improvements. Several of these features, like `frozendict`, sentinels, and lazy imports, were first proposed and rejected years ago, which says something about how carefully the language evolves.

**In this tutorial, you’ve seen highlights like:**

- **Lazy imports** that load modules only when you first use them
- **Unpacking in comprehensions** for flattening and merging data
- **`frozendict`** for mappings that can’t change and can be hashed
- **`sentinel`** for readable, pickle-safe “no value” markers
- **`TypeForm` and closed `TypedDict` types** for more precise type hints
- **A sampling profiler** that attaches to live processes with almost no overhead
- **Smarter error messages and a friendlier REPL** that meet you where you are
- **UTF-8** as the default encoding on every platform
- **An upgraded JIT compiler** with measurable speedups on the official benchmarks

This tutorial only scratched the surface of six of these features. Each of them has its own tutorial with the full story, including the history, benchmarks, and edge cases that didn’t fit here:

If you’d rather watch than read, then the What’s New in Python 3.15 video course walks you through the highlights. Either way, you can form your own opinion right now. One command, `uv run --python 3.15 python`, and you’re in.

**Try It Yourself:** Get the code examples you need to try lazy imports, frozendict, TypeForm, the sampling profiler, and more new features in Python 3.15.

Free Bonus: **What's New in Python 3.15 Cheat Sheet**

Get a **What's New in Python 3.15 Cheat Sheet (PDF)** that sums up every new feature from this tutorial on a single page:

Below are a few questions that come up every release cycle. Click the *Show/Hide* toggle beside each question to reveal the answer.

The biggest additions are explicit lazy imports with the `lazy` keyword, the `frozendict` and `sentinel` built-in types, unpacking in comprehensions, and the Tachyon sampling profiler in the new `profiling` package. The release also makes UTF-8 the default encoding, improves error messages and the REPL, adds `TypeForm` and closed `TypedDict` types to the type system, and upgrades the experimental JIT compiler.

Run `uv run --python 3.15 python`, and uv downloads a standalone interpreter that lives next to, not instead of, your current Python. You can also grab the official installers from python.org, which install side by side with other versions.

Often, but it depends on your platform and build. The official Windows 64-bit installers now use the tail-calling interpreter, which the docs report as 15 to 20 percent faster than the old switch-based interpreter.

Separately, the experimental JIT speeds up the official benchmark suite by 7 to 8 percent on x86-64 Linux and by 11 to 12 percent over the tail-calling interpreter on AArch64 macOS, though individual benchmarks vary widely. Lazy imports can also cut startup time for command-line tools.

No. They move import work from startup to first use, which helps short-lived programs like command-line tools that don’t touch most of their imports. A long-running program that uses everything it imports pays the same total cost, just later.

 **Test your knowledge with our interactive “Python 3.15: Cool New Features for You to Try” quiz. You’ll receive a score upon completion to help you track your learning progress:**

**Interactive Quiz**

Test your knowledge of Python 3.15's new features, from lazy imports, frozendict, and sentinels to TypeForm, Tachyon, and UTF-8 by default.
