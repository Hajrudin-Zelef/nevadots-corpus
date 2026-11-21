---
id: collect-261121-veille-ia/python-php/python-lazy-imports-speed-up-startup-with-the-lazy-keyword-real-python
title: "Python Lazy Imports: Speed Up Startup With the lazy Keyword – Real Python"
domain: python-php
role: reference
task: article
actors: []
dates: []
keywords: ["python", "lazy", "youll", "imports", "import", "tutorial", "keyword", "first"]
source: https://realpython.com/python315-lazy-imports/
source_anchor: ""
source_lines: [1, 773]
section: "Python_PHP"
---
Python 3.15 adds lazy imports: a `lazy` keyword that puts off loading a module until the first time you use it. The import statement stays where it belongs, at the top of your file, but the work of running it moves to the moment something touches the module’s name. In this tutorial, you’ll make a command-line tool start more than twice as fast without moving a single import.

**By the end of this tutorial, you’ll understand that:**

- A **lazy import** binds its name immediately and**loads the module** the first time something reads that name.
- **Deferring the imports** that a code path never touches can**cut startup time** sharply, and every import still sits at the top of the file.
- **`lazy` is rejected inside functions, class bodies, and `try` blocks** , so a deferred import can’t slip past an`except ImportError` fallback.
- **`__lazy_modules__` lets one file defer on Python 3.15** and stay eager on older versions because they ignore it silently instead of refusing to parse it.
- A module that does its work at **import time** , such as**registering a plugin** , never runs at all if nothing reads its name.

First, you’ll install Python 3.15. Then you’ll measure what your imports cost today, see why the old workarounds are unsatisfying, write your first lazy import, and put the keyword to work on a real command-line tool. Along the way, you’ll retire the `if TYPE_CHECKING` guard, run into the imports that have to stay eager, and learn how to adopt deferral in a codebase that still has to run on older Pythons.

To get the most out of this tutorial, you should be comfortable running Python scripts from a terminal. Some familiarity with type hints will help in one section, though you’ll get the background you need as you go.

**Note:** The examples in this tutorial were tested with Python 3.15.0rc1, so you may see minor differences in the output on the final 3.15 release.

You’ll work through this tutorial with a small command-line tool and a few demo modules. You can download all of them here:

**Get Your Code:** Click here to download the free sample code you’ll use to defer heavy modules and speed up your CLI startup with Python 3.15.

 **Test your knowledge with our interactive “Python 3.15 Preview: Lazy Imports” quiz. You’ll receive a score upon completion to help you track your learning progress:**

You’ll need Python 3.15 or later. The quickest path is `uv`, which downloads a prebuilt interpreter for your platform. If you don’t have it yet, then Real Python’s guide covers installing `uv` on every platform. Once `uv` is in place, you can run Python 3.15 straight away:

```
$ uv run --python 3.15 python -VV
Python 3.15.0rc1 (main, Aug 25 2026, 13:50:37) [Clang 22.1.3 ]
```
Your version string will name the current 3.15 release rather than the release candidate shown here. A single `-V` would print just the version number. Doubling it to `-VV` adds the build date and the compiler, which is worth having while you’re juggling more than one interpreter and want to be certain which one you landed on.

Running `pyenv install 3.15` gets you to the same place by a different route. It compiles CPython from source, so you’ll need a build toolchain first: a C compiler, the Python headers, and a handful of system libraries. That takes longer, but it’s the option to reach for when you want to configure the build yourself.

You can also run Python 3.15 in Docker and keep it off your machine entirely, or use one of the official installers covered in How to Install Python on Your System.

Next, unpack the sample code you downloaded and change into the folder it creates. That folder is home base for the rest of this tutorial, and every `cd` after this one is relative to wherever the last one left you:

```
$ cd materials-python315-lazy-imports/
```
Now create a virtual environment on Python 3.15 and activate it, so that a bare `python` means 3.15 in every folder you visit. Pinning with `uv python pin` doesn’t do that, because the `.python-version` file it writes only applies to `uv run`:

```
$ uv venv --python 3.15
Using CPython 3.15.0rc1
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
```
That creates a `.venv/` folder inside the sample code folder. Activating it is the step that puts Python 3.15 on your path:

With the environment active, a bare `python` is the 3.15 build:

```
$ python -VV
Python 3.15.0rc1 (main, Aug 25 2026, 13:50:37) [Clang 22.1.3 ]
```
That’s the interpreter that every bare `python` command from here on will use.

**Note:** If you already had an early 3.15 pre-release on your machine, then `uv` may reuse it rather than fetching anything newer, and the `lazy` keyword didn’t arrive until the betas. The `-VV` output above names the exact build. If it names an alpha, then ask for a newer build explicitly:

```
$ uv venv --python '>=3.15.0'
```
That specifier skips the pre-releases and resolves to the stable release.

If you took the `pyenv` route instead, then the `pyenv shell 3.15` command selects the version for the current session rather than creating an environment.

That’s the whole setup.

The quickest way to see what the keyword does to a parser is to hand it to each version in turn:

```
$ uv run --python 3.14 python -c "lazy import json"
  File "<string>", line 1
    lazy import json
         ^^^^^^
SyntaxError: invalid syntax
$ uv run --python 3.15 python -c "lazy import json"
```
On 3.14, the keyword doesn’t exist, so the parser rejects the line before it runs anything. On 3.15, the same command succeeds and prints nothing, which is exactly what you’d want from an import that hasn’t been used yet.

If the 3.15 line raises a `SyntaxError` too, then you’ve been handed an early 3.15 alpha from before the keyword landed, and the note above shows how to ask for a newer build.

That difference matters beyond checking your build. A `lazy` keyword is a hard `SyntaxError` on every Python released so far, which shapes how libraries can adopt this at all. You’ll come back to that near the end of the tutorial.

With the environment in place, you can find out what your imports are costing you today.

Every `import` statement at the top of a module runs when that module loads. That’s true whether or not the code path you’re on needs what it imports. A command-line tool that imports a web server, an async runtime, and a GUI toolkit pays for all three before it prints a single character of `--help` output.

For a long-running server, that cost is paid once and forgotten. For a script that runs and exits, or a tool a person launches dozens of times a day, it’s the bulk of the runtime.

Python nearly fixed this in 2022. PEP 690 proposed making imports lazy implicitly and globally, controlled by a single switch. The Steering Council rejected it largely because it would have split the community into two Pythons and forced library authors to test their code both ways.

PEP 810 is the answer to that objection. Laziness is explicit—the keyword applies to one import at a time and doesn’t cascade into the modules you import.

Before you can shrink a number, you need to know what it is. Python ships with a flag that reports the cost of every import in a run, and you can point it at any script.

You’ll work with a small tool called `report`, which comes with the sample code. It summarizes a CSV file using `statistics` for the number-crunching and has four optional modes: it can serve the report over HTTP, open a desktop viewer, fetch rows from a remote source, and export to XML. The fetching is stubbed out so that you can run everything offline.

Each of those optional modes brings an import with it, and so does the summarizing:

`cli_eager.py`
    ```
 8import argparse
 9import csv
10
11import asyncio
12import http.server
13import statistics
14import tkinter
15import xml.etree.ElementTree as ET
16
17import handlers
18import handlers.csv_out
19import handlers.json_out
```
Ten imports, and printing a page of help text needs almost none of them.

The tool reads its CSV file and loads its plug-in package relative to the working directory, so it needs to run from its own folder:

```
$ cd report_cli/
```
Every command in this section runs from there. Sorting the tool’s import log by cumulative cost shows where the time goes before the tool prints any help text:

```
$ python -X importtime cli_eager.py --help 2>&1 >/dev/null \
    | grep -E '\| [^ ]' | sort -t'|' -k2 -rn | head -6
import time:       186 |      26739 | asyncio
import time:      1007 |      11776 | http.server
import time:       976 |       7984 | site
import time:      7544 |       7544 | _colorize
import time:      1563 |       5478 | tkinter
import time:       986 |       4488 | argparse
```
Your own figures won’t match these, and they’ll shift from run to run on the same machine. A cold run pays the cost of pulling shared libraries off disk, which hits `tkinter` hardest because it loads the native Tk toolkit underneath. The numbers above come from a warm run, so run the command a couple of times before you compare. What stays stable is the shape of the list, not the milliseconds.

The `grep` keeps only the top-level imports and drops the modules they pull in underneath. Without it, the modules that `asyncio` pulls in would fill most of the list and crowd out the imports you can actually do something about.

The filters also hide anything that isn’t an import line. If your own run comes back with only a few entries and no `asyncio`, then drop the pipe and read the raw output because an error message from the script is likely sitting at the bottom of it.

The middle column is cumulative microseconds, so `asyncio` alone accounts for about 27 milliseconds and `http.server` for another 12. Neither of them does anything on the `--help` path, and neither does `tkinter` further down.

The other three are a useful contrast. The `site` entry is interpreter startup rather than anything your file imports, `_colorize` is what paints Python’s help text and error messages, and `argparse` does the work that every single run needs. None of those three is a candidate for deferral.

Counting the modules gives you the other half of the picture:

```
$ python -X importtime cli_eager.py --help 2>&1 >/dev/null \
    | grep -c '^import time: *[0-9]'
222
```
That’s 222 modules loaded so the tool can print a paragraph of help text. It’s the number you’ll shrink over the course of this tutorial. Expect your own total to land a module or two either side of that because the total includes the modules Python loads before your code runs, and those depend on how your environment was set up.

Python programmers have been deferring imports for years, and the standard library does it too. PEP 810 counted roughly 17 percent of the standard library’s non-test imports sitting inside a function body rather than at the top of a module.

That’s the first of four workarounds, and you may have used it in your code. Moving the import inside the function that needs it defers the cost until the first call:

```
def to_xml(rows):
    import xml.etree.ElementTree as ET
    ...
```
A module-level `__getattr__` does the same for a whole module, at the price of a hook that runs on every attribute lookup that fails:

```
def __getattr__(name):
    if name == "ET":
        global ET
        import xml.etree.ElementTree as ET
        return ET
    raise AttributeError(name)
```
The other two are narrower. An `if TYPE_CHECKING` guard, which you’ll come back to later in this tutorial, skips imports that exist only to satisfy annotations, and `importlib.util.LazyLoader` wires up a module proxy by hand.

All four work. The trouble is what each one costs you:

| Approach | Import stays at the top? | Defers the cost? | Works for any import? | 
|---|---|---|---|
| Import inside the function | No | Yes | Yes | 
| Module-level `__getattr__` shim | No | Yes | Yes | 
| `if TYPE_CHECKING` guard | Yes | Yes | Type hints only | 
| `importlib.util.LazyLoader` | No | Yes | Whole modules only | 

The first two rows move the import out of the one place a reader looks for it. Tuck `import pandas` inside a function, and nobody scanning the top of the file knows the module is a dependency. The `__getattr__` shim is worse because now there’s machinery to understand as well.

The fourth row is the closest existing option. The `importlib.util.LazyLoader` class has been in the standard library for years and still works on 3.15, but you have to build the loader and module spec yourself, and it operates on whole modules only. Few people use it.

Not one of those rows manages a yes in all three columns. That’s the gap PEP 810 fills:

| Approach | Import stays at the top? | Defers the cost? | Works for any import? | 
|---|---|---|---|
| `lazy import` | Yes | Yes | Yes | 

The import stays where a reader looks for it, the cost is still deferred, and it works for any import, not just the ones that exist to satisfy a type checker.

The syntax is one word in front of an import you’d write anyway. That’s all there is to it, and that’s deliberate: PEP 810 went through several rounds of design specifically to avoid introducing new machinery that readers would have to learn.

Two forms take the keyword, matching the two forms of import you already write. There’s also a short list of places where Python won’t accept it, and each of those restrictions blocks a way deferral could go wrong.

You’ll work through the two forms first, then the restrictions.

To watch deferral happen, you need a module that announces itself when Python runs it:

`noisy_module.py`
    ```
print("noisy_module is loading now")
VALUE = 42
```
Now import it lazily and check whether Python has loaded it:

`probe.py`
    ```
 1import sys
 2
 3lazy import noisy_module
 4
 5print("The lazy import statement has run.")
 6print("Loaded?", "noisy_module" in sys.modules)
 7
 8print(noisy_module.VALUE)
 9print("Loaded?", "noisy_module" in sys.modules)
```
Before you run that, go back up to the top-level sample code folder, where `probe.py` and `noisy_module.py` live:

```
$ cd ..
```
The interesting part is the ordering of the output:

```
$ python probe.py
The lazy import statement has run.
Loaded? False
noisy_module is loading now
42
Loaded? True
```
The `lazy import` line ran, and afterward, `noisy_module` still wasn’t in `sys.modules`. The module’s own `print()` call didn’t appear until line 8, where you read `noisy_module.VALUE`. That’s the whole mental model: the name is bound immediately, and the module loads on first use.

The moment a lazy import resolves and the real module loads is called **reification**, which is the term PEP 810 uses throughout.

**Note:** Reification fires when something reads the *name*, which has a consequence worth knowing early: looking at a lazy import loads it. Printing `noisy_module`, passing it to `type()`, or hovering over it in a debugger resolves the very thing you were trying to inspect. The same goes for any other module-level name holding a placeholder, so stashing it in a second global doesn’t help.

To reach a pending import without resolving it, index the namespace dictionary directly and don’t bind the result. The expression `globals()["noisy_module"]` hands back the placeholder object, whose `repr()` reads `<lazy_import 'noisy_module'>`. Calling `type(globals()["noisy_module"])` is safe for the same reason.

The placeholder doesn’t forward attribute access either, so `globals()["noisy_module"].VALUE` raises an `AttributeError` instead of resolving the import behind your back.

Dotted imports take the keyword too, and they resolve in two stages. Writing `lazy import xml.etree.ElementTree` binds `xml` in your namespace, and reading `xml` loads only the top-level package. The submodule waits until you touch it as an attribute. You’ll see why that second stage matters when you get to import-time side effects.

The `from` form takes the keyword too, and so does an aliased import:

```
lazy from http.server import HTTPServer
lazy import xml.etree.ElementTree as ET
```
Both behave the way you’d hope. The name lands in your namespace immediately, and the module behind it waits.

Something subtle happens when you pull several names out of one module. Python can’t fetch one name without running the whole module, so touching any of them loads it. What it can do is track each name separately afterward.

To watch that happen, you need a module with two names in it. This one announces itself the same way `noisy_module` did:

`shapes.py`
    ```
print("shapes is loading now")
CIRCLE = "circle"
SQUARE = "square"
```
Now import both names lazily and read only one of them:

`partial.py`
    ```
 1lazy from shapes import CIRCLE, SQUARE
 2
 3print(CIRCLE)
 4print(type(globals()["CIRCLE"]))
 5print(type(globals()["SQUARE"]))
```
Reading `CIRCLE` on line 3 has to load `shapes`, since that’s where the value lives. Comparing the two names afterward is the interesting part:

```
$ python partial.py
shapes is loading now
circle
<class 'str'>
<class 'lazy_import'>
```
The module loaded, and `CIRCLE` resolved to an ordinary string, but `SQUARE` was still a placeholder object of type `lazy_import`. It resolves as soon as something reads it.

The keyword is a module-level feature. Python rejects it anywhere else:

| Context | `lazy` allowed? | 
|---|---|
| Function body | No | 
| Class body | No | 
| `try` ,`except` ,`else` ,`finally` | No | 
| `lazy from module import *` | No | 
| `lazy from __future__ import ...` | No | 
| Module-level `if` ,`for` ,`while` ,`with` ,`match` | Yes | 

Each rejection comes with its own message, so the parser tells you which rule you broke rather than leaving you to guess. This file breaks the first rule in the table:

`badfunc.py`
    ```
 1def load():
 2    lazy import json
```
Running it names the rule:

```
$ python badfunc.py
  File "badfunc.py", line 2
    lazy import json
    ^^^^^^^^^^^^^^^^
SyntaxError: lazy import not allowed inside functions
```
Every other row in the table names its restriction just as plainly.

The last row is the one that looks like an exception, but isn’t. A `lazy import` inside a module-level `if` block is fine because that block still runs at the module level.

The `try` restriction deserves a closer look, and you’ll see why it makes sense once you’ve watched a deferred import fail. The restriction is on where the `lazy import` statement lives, not on where you use the name, so referring to a lazily imported module from inside a function is fine—that’s where reification usually happens.

Existing code will keep working, by the way. `lazy` is a **soft keyword**, so a variable named `lazy` still works, even in a file that also uses the keyword.

You’ve seen the syntax on modules built to demonstrate it. Now you’ll reproduce the speedup from the top of this tutorial on something closer to code you’d actually ship.

The imports you measured earlier are `asyncio`, `http.server`, `statistics`, `tkinter`, and `xml.etree.ElementTree`, plus two plug-in modules for the tool’s output formats.

None of that is unreasonable, and none of it is needed to print help text.

The whole change is five keywords. Here’s that import block again, exactly as you saw it earlier:

`cli_eager.py`
    ```
 8import argparse
 9import csv
10
11import asyncio
12import http.server
13import statistics
14import tkinter
15import xml.etree.ElementTree as ET
16
17import handlers
18import handlers.csv_out
19import handlers.json_out
```
The lazy version changes five of those lines:

`cli_lazy.py`
    ```
 8import argparse
 9import csv
10
11lazy import asyncio
12lazy import http.server
13lazy import statistics
14lazy import tkinter
15lazy import xml.etree.ElementTree as ET
16
17import handlers
18import handlers.csv_out
19import handlers.json_out
```
The highlighted lines are the change. Nothing else in the file moved.

That’s the difference between this and every workaround that came before. No import migrated into a function body, no module grew a `__getattr__`, and anyone opening the file still sees the complete dependency list before the first function definition, where they’d think to look for it.

Notice which imports stayed eager. Every run parses arguments, so deferring `argparse` would buy nothing and cost a little clarity, and `csv` takes under a millisecond to import, which is below the level where deferring it is worth the thought.

The two `handlers` plug-in modules stayed eager for a more interesting reason, which you’ll come to in the section on traps. Going the other way, `statistics` went lazy even though summarizing is the tool’s whole job, because `--help` never summarizes anything.

Back in `report_cli/`, you’ll find `bench.py` alongside the two versions of the tool. It runs a script ten times and reports the fastest run, so point it at each version in turn:

```
$ cd report_cli/
$ python bench.py cli_eager.py --help
cli_eager.py: 68 ms (best of 10)
$ python bench.py cli_lazy.py --help
cli_lazy.py: 32 ms (best of 10)
```
Same code, same machine, same interpreter. The only difference is five keywords, and `--help` is now more than twice as fast.

Counting the modules again shows where that time went. This is the command you ran on the eager version earlier, pointed at the lazy one:

```
$ python -X importtime cli_lazy.py --help 2>&1 >/dev/null \
    | grep -c '^import time: *[0-9]'
76
```
Your own numbers will differ, since they depend on your hardware and on how much of your dependency graph goes unused, but the two measurements tell the same story:

| Metric | Eager | Lazy | 
|---|---|---|
| `--help` startup | 68 ms | 32 ms | 
| Modules imported | 222 | 76 | 

The two rows don’t move by the same proportion, and the gap between them is worth a moment. Two-thirds of the modules dropped away, but only about half the startup time went with them because the imports that stay eager include some of the more expensive ones.

**Note:** This is a favorable case by construction. The `--help` path uses none of the deferred modules, which is exactly when laziness pays best. A tool whose common path needs most of its dependencies will see far less.

The obvious worry is that deferral costs something once the code actually uses those modules. It doesn’t.

The tool has a `--load-all` flag that exists for this measurement: it reads all five deferred names, so every one of them reifies, and nothing is skipped. It doesn’t print the help text, though, so compare these two figures against each other rather than against the ones above. On that path, the two versions finish in the same time:

```
$ python bench.py cli_eager.py --load-all
cli_eager.py: 60 ms (best of 10)
$ python bench.py cli_lazy.py --load-all
cli_lazy.py: 59 ms (best of 10)
```
The gap is smaller than the run-to-run variation you’ll see by repeating either command. Once a lazy import resolves, the binding is indistinguishable from one that was never lazy.

If you write type hints, you’ve probably got a `TYPE_CHECKING` guard somewhere in your codebase:

```
from typing import TYPE_CHECKING
if TYPE_CHECKING:
    from decimal import Decimal
```
The goal is reasonable. You want the name available to your type checker without paying for the import at runtime. The trouble is that the name really doesn’t exist at runtime, and plenty of tools want to read your annotations while your program is running.

Before Python 3.14, you had to write that hint as the string `"Decimal"` so Python wouldn’t try to look the name up when it defined the function. Since 3.14, annotations aren’t evaluated until something asks for them, so the unquoted version imports cleanly too, as Python 3.14: Lazy Annotations covers in depth.

The catch shows up when something asks what the annotation actually means. Type checkers read annotations statically, so they never notice the name is missing. Anything that reads them at runtime does:

`type_checking_guard.py`
    ```
 1from typing import TYPE_CHECKING, get_type_hints
 2
 3if TYPE_CHECKING:
 4    from decimal import Decimal
 5
 6def to_pennies(amount: Decimal) -> int:
 7    return int(amount * 100)
 8
 9print(get_type_hints(to_pennies))
```
Defining the function works, and so does calling it. Asking for its type hints is what breaks:

```
$ cd ..
$ python type_checking_guard.py
Traceback (most recent call last):
  ...
  File "type_checking_guard.py", line 6, in __annotate__
    def to_pennies(amount: Decimal) -> int:
                           ^^^^^^^
NameError: name 'Decimal' is not defined
```
The function was defined without complaint and would have run without complaint. The failure waited until `get_type_hints()` asked Python to evaluate the annotation, at which point there was no `Decimal` to evaluate. That’s the `__annotate__` frame in the traceback: since 3.14, the annotation is stored as a function that computes it on demand, and this is the moment it finally runs.

Plenty of tools do exactly that. Data validation libraries build their schemas from annotations, web frameworks derive request models from function signatures, and `dataclasses` inspects them to work out field types. All of them hit this `NameError`.

A lazy import gives you the runtime savings and a name that still resolves. Drop the guard and the quoting:

`lazy_annotation.py`
    ```
 1import sys
 2from typing import get_type_hints
 3
 4lazy from decimal import Decimal
 5
 6def to_pennies(amount: Decimal) -> int:
 7    return int(amount * 100)
 8
 9print("decimal loaded?", "decimal" in sys.modules)
10print(get_type_hints(to_pennies))
11print("decimal loaded?", "decimal" in sys.modules)
```
Watch when `decimal` shows up:

```
$ python lazy_annotation.py
decimal loaded? False
{'amount': <class 'decimal.Decimal'>, 'return': <class 'int'>}
decimal loaded? True
```
Defining the function didn’t load `decimal`, and neither would calling it. The module loaded at the moment something evaluated the annotations, and the hint resolved to the real class rather than raising.

That’s both halves of what you wanted. The import costs nothing on the paths that don’t inspect annotations, which is almost all of them, and the name is there for the paths that do.

The other thing you gain is that the annotation now matches reality. Under a `TYPE_CHECKING` guard, the hint claims a name exists at runtime when it doesn’t, and you find out only when something asks. With a lazy import, the name exists, and the module load is what’s deferred.

**Note:** The tooling hasn’t caught up yet. As of this writing, `mypy` and Pyright both report the `lazy` keyword as a syntax error, and `isort` and `ruff` don’t recognize it either. That’s an awkward trade-off when the whole point here is to serve your type checker. Until they ship support, you can get the same runtime behavior without the parse problem by using `__lazy_modules__`, a per-module opt-in that has a section of its own further down.

Deferred imports change two things you might not expect: where an import failure shows up, and whether some modules run at all. This section covers both, plus the problem the keyword looks like it should solve outright, which it only partly does.

Don’t let any of this put you off the feature. Most imports defer safely, and the risky ones fall into a few recognizable shapes. Once you can spot those shapes, you can use the keyword confidently.

Deferring an import moves *when* it can fail, which means it also moves where you see the failure. That’s the part most likely to surprise you at three in the morning.

This script imports a module that doesn’t exist, then keeps going:

`fail.py`
    ```
 1lazy import missing_mod
 2
 3print("Still running.")
 4print(missing_mod.value)
```
On any earlier version of Python, line 1 raises immediately, and the script never reaches the print. On 3.15, the run gets further:

```
$ python fail.py
Still running.
Traceback (most recent call last):
  File "fail.py", line 1, in <module>
    lazy import missing_mod
ImportError: lazy import of 'missing_mod' raised an exception
⮑ during resolution
The above exception was the direct cause of the following exception:
Traceback (most recent call last):
  File "fail.py", line 4, in <module>
    print(missing_mod.value)
          ^^^^^^^^^^^
ModuleNotFoundError: No module named 'missing_mod'
```
The print on line 3 ran, which it never would have before. Then the failure surfaced on line 4, where you touched the name.

The traceback names both places. The first half points at the `lazy import` statement that set up the deferral, and the second half points at the access that triggered it. That chaining is deliberate, and it’s what keeps deferred failures debuggable.

Stepping through the file one line at a time makes the shift concrete. Switch the import style to compare the deferred run against the eager one, where line 1 raises and the print never happens:

This is also where the `try` block restriction earns its keep. The whole point of wrapping an import in `try` and `except ImportError` is to catch the failure right there and fall back to something else. A deferred import would sail straight past that handler and blow up somewhere unrelated, so Python refuses to compile it.

Some modules exist to be imported. A plugin that registers itself with a decorator does its entire job at import time, and nothing ever reads its name afterward.

The `report` tool has exactly this shape. Its `handlers` package holds a registry, and each format plugin registers itself when it loads:

`handlers/csv_out.py`
    ```
from handlers import register
@register("csv")
def emit(rows):
    return ",".join(str(row) for row in rows)
```
The main module imports both plugins purely for that effect, then lists whatever ended up in the registry. The sample code includes `cli_too_lazy.py`, which is `cli_lazy.py` with those two plug-in imports deferred as well. That one extra step changes what `--list-formats` prints:

```
$ cd report_cli/
$ python cli_lazy.py --list-formats
csv, json
$ python cli_too_lazy.py --list-formats
```
That second command really did print an empty line rather than nothing at all. The over-lazy version found no formats to list, and you got no exception and no warning along the way. The tool has simply lost the ability to write CSV.

The reason follows directly from the mental model you built earlier. A deferred import fires when something reads the bound name, and nothing ever touches the `csv_out` attribute. Reading `handlers` reifies only the package itself. The import was only ever there for its side effect, so deferring it means the module never runs at all.

Earlier, `noisy_module` printed its line late rather than never because something eventually read the name. Here, nothing ever does.

The same reasoning applies to any module whose value lies in the act of importing it. Before you mark an import lazy, check whether the module does any of these at import time:

- Registers something with a decorator, as the plugins here do
- Configures logging or installs an exception hook
- Calls `warnings.simplefilter()` to set up filters
- Installs a signal handler
- Hooks `__init_subclass__` or a metaclass that records subclasses

If any of those apply, then the import needs to stay eager. That’s why the two plug-in imports in the tool stayed eager back when you marked the heavy imports lazy, while the five heavy standard library imports went lazy.

**Warning:** This is the failure mode to watch for, because it stays silent. Every other mistake in this tutorial announces itself with a `SyntaxError` or a traceback. This one gives you a working program that does less than it used to, without saying so.

Deferred imports are recommended as a circular-import cure often enough that it’s worth being precise about what they do and don’t fix.

Take two modules that each import a name from the other, which is the shape that usually breaks:

`circular/eager/a.py`
    ```
from b import B
class A:
    def make_b(self):
        return B()
```
With `b.py` doing the mirror image, importing `a` fails the way you’d expect:

```
$ cd ../circular/eager/
$ python main.py
Traceback (most recent call last):
  ...
  File "b.py", line 1, in <module>
    from a import A
ImportError: cannot import name 'A' from 'a' (consider renaming 'a.py'
⮑ if it has the same name as a library you intended to import)
```
It breaks partway through `a.py`: Python went to `b.py`, and `b.py` asked for a name that `a.py` hadn’t defined yet.

Now mark the import in `a.py` as `lazy from b import B`, which is what you’ll find in `circular/lazy/`. The cycle resolves, and the program runs. Better still, marking *either* module is enough, and it works whichever module you import first. Deferring one side is all it takes to break the deadlock.

That’s a real fix for a real problem, but it has a boundary, and you should see where it lies instead of taking it on trust. The cycle above needed `B` only once somebody called `make_b()`, long after both modules had finished loading. Deferral works there because the deadline moved.

Now consider a cycle where each module needs a value from the other *while both are still initializing*:

`circular/init_lazy/pricing.py`
    ```
 1lazy from tax import RATE
 2
 3TOTAL = 100 * (1 + RATE)
```
The other half of the cycle asks for `TOTAL` on line 1, before it has defined `RATE` on line 3:

`circular/init_lazy/tax.py`
    ```
 1from pricing import TOTAL
 2
 3RATE = 0.2
 4BUDGET = TOTAL / 2
```
The keyword is there, but it buys nothing. Line 3 of `pricing.py` reads `RATE` immediately, which reifies `tax`, which asks `pricing` for a `TOTAL` that line 3 is still in the middle of computing:

```
$ cd ../init_lazy/
$ python main.py
Traceback (most recent call last):
  File "pricing.py", line 1, in <module>
    lazy from tax import RATE
ImportError: lazy import of 'tax.RATE' raised an exception during
⮑ resolution
The above exception was the direct cause of the following exception:
Traceback (most recent call last):
  File "main.py", line 1, in <module>
    import pricing
  File "pricing.py", line 3, in <module>
    TOTAL = 100 * (1 + RATE)
                       ^^^^
  File "tax.py", line 1, in <module>
    from pricing import TOTAL
ImportError: cannot import name 'TOTAL' from 'pricing' (consider
⮑ renaming 'pricing.py' if it has the same name as a library you
⮑ intended to import)
```
Drop the keyword, and you get the same `ImportError` from the same line of `tax.py`, just without the deferred-resolution frames wrapped around it. The sample code includes both versions, in `circular/init_eager/` and `circular/init_lazy/`, so you can run them back to back. Deferral moved when the value was needed, but it was still needed too early.

The exception type follows the import form rather than the laziness. A cycle built on `import module` and `module.attribute` raises `AttributeError` on a partially initialized module, while one built on `from module import name` raises `ImportError`. Adding `lazy` changes neither.

PEP 810 puts the boundary plainly: making the imports lazy might help only if the circular reference isn’t accessed during module initialization. Deferral only buys you time. It doesn’t change your dependency graph. If you have a true cycle, the fix is still to restructure the code, usually by moving the shared piece into a third module that both sides import.

The keyword is the headline feature, but most real codebases will get there another way.

The problem with a new keyword is that it’s a hard `SyntaxError` on every Python version that came before it. If your code needs to run on 3.14 as well as 3.15, then you can’t put `lazy` in a file that both versions have to parse. That rules out the keyword for most library authors for the next few years.

Python 3.15 offers two answers to that, and they suit different situations.

Assign a `__lazy_modules__` collection at the top of a module, and Python defers the matching plain imports with no keyword anywhere:

`bridge.py`
    ```
 1import sys
 2
 3__lazy_modules__ = {"json"}
 4
 5import json
 6
 7major, minor = sys.version_info[:2]
 8deferred = "json" in getattr(sys, "lazy_modules", ())
 9print(f"Python {major}.{minor}: json deferred? {deferred}")
10print(json.dumps({"ok": True}))
```
Line 8 asks `sys.lazy_modules` whether `json` is still pending, falling back to an empty tuple on the versions that don’t have that attribute. A name leaves that set as soon as the module loads, which is why the check has to come before line 10. Running the same file on two interpreters shows what the assignment buys you:

```
$ cd ../..
$ uv run --python 3.14 python bridge.py
Python 3.14: json deferred? False
{"ok": true}
$ uv run --python 3.15 python bridge.py
Python 3.15: json deferred? True
{"ok": true}
```
Both runs work, and only one of them defers. On 3.14, `__lazy_modules__` is a module-level variable that nothing reads, so the assignment is silently ignored, and the import is eager. On 3.15, it’s honored, and `json` stays pending until line 10 serializes something. The `lazy` keyword, by contrast, is a hard `SyntaxError` on 3.14. That difference is what makes `__lazy_modules__` the practical option for a library that still has to run on older Pythons.

Names in the collection must be fully qualified, and for `from` imports, it’s the module after `from` that counts, not the names you’re importing. The assignment also has to come before the imports it covers because Python reads `__lazy_modules__` as it executes each import statement. An assignment further down the file is ignored without complaint.

The standard library’s own `tomllib` uses this, and so does Google’s Python client generator. When they implemented PEP 810 lazy loading, `google-cloud-compute` went from importing 1,407 modules to importing 15. Cold start dropped from about 23 seconds to roughly 1 second, and peak memory fell by 96 percent.

Google’s stated reason for choosing `__lazy_modules__` over the keyword was precisely that older Python versions ignore it.

The `-X lazy_imports` flag is the global switch PEP 690 proposed, with the one difference that decided that PEP’s fate. It’s off by default and opted into per process by whoever owns the application, so it can’t split the ecosystem the way an ambient default would have.

You can turn laziness on for every top-level import in a process using the `-X lazy_imports=all` flag, the `PYTHON_LAZY_IMPORTS` environment variable, or `sys.set_lazy_imports()` at runtime. When several of those are set, the function call wins, then the flag, then the environment variable.

A short script is enough to see the difference. This one imports `json` without any keyword and asks whether it’s still pending:

`allmode.py`
    ```
import sys
import json
print("json deferred?", "json" in sys.lazy_modules)
print(json.dumps({"ok": True}))
```
Running it both ways shows the mode taking effect:

```
$ python allmode.py
json deferred? False
{"ok": true}
$ python -X lazy_imports=all allmode.py
json deferred? True
{"ok": true}
```
In the default `normal` mode, only imports you’ve marked are lazy. Under `all`, every module-level import becomes a candidate. You can carve out exceptions with `sys.set_lazy_imports_filter()`, which takes a callable receiving the importing module’s name, the resolved name of the imported module, and the from-list. That last argument is `None` for a plain `import`. Return `True` to keep an import lazy or `False` to force it eager.

**Note:** PEP 810 documents a third mode, `none`, which would disable laziness entirely. It didn’t ship, and asking for it raises:

```
$ python -c "import sys; sys.set_lazy_imports('none')"
Traceback (most recent call last):
  ...
ValueError: mode must be 'normal' or 'all'
```
PEP 810 is now marked as a historical document, with the language reference as the authoritative source. When the two disagree, trust the Python 3.15 documentation, not the PEP.

You’ve now seen what deferral buys and what it breaks. This table summarizes what to do with your own code:

| Import | Mark it lazy? | Why | 
|---|---|---|
| Heavy dependency used on one code path | Yes | This is the case the feature exists for | 
| Type-only import behind `if TYPE_CHECKING` | Yes | Your annotations resolve once your type checker supports the keyword | 
| Optional dependency in a `try` block | No | Python rejects it as a `SyntaxError` , by design | 
| Module with import-time side effects | No | Its side effect never runs at all | 
| Small module you use immediately | No | There’s nothing to defer | 

One rule of thumb covers most of these: if the import exists so that a name is available later, deferring it is safe. If the import exists so that something happens now, deferring it is a bug.

How you adopt this depends on whether you’re writing an application or a library, and the advice pulls in opposite directions.

If you own the application, then you also own the risk, which means you can afford to be aggressive.

Start by profiling your entry point with `-X importtime` and looking at what loads before your program does anything. Mark the worst offenders, run your test suite, and confirm the behavior didn’t change. Measure startup before and after, instead of trusting the module count.

If going through every import sounds tedious, then `all` mode is a good way to find out what you’d save before committing to anything. Run your test suite under `-X lazy_imports=all` and see what breaks. The failures point straight at your side-effecting modules.

Treat that as a diagnostic rather than a shipping configuration, though. The mode applies to every module in the process, including third-party packages whose import-time behavior you’ve never audited, and it will empty a plug-in registry like the `report` tool’s just as silently as the keyword would.

If you do keep it on, remember that `sys.set_lazy_imports_filter()` only affects imports that run after you install it, so it belongs in a wrapper script or `sitecustomize`, not at the top of your entry point.

If you’re writing a library, then the calculation changes because your decisions land in other people’s programs.

Mark only the imports you’ve verified individually. A module that’s safe to defer in your test suite might not be safe in an application that imports your package for a side effect you didn’t think about. So the bar for confidence is higher than it is in your own code.

Prefer `__lazy_modules__` for as long as you support Python 3.14 and earlier. Your users on older versions get the eager behavior they’ve always had, and your users on 3.15 get faster imports, from the same source file.

Above all, don’t call `sys.set_lazy_imports()` on your users’ behalf. It’s process-wide state, and reaching for it from library code is the same category of rudeness as calling `sys.setrecursionlimit()`.

The `lazy` keyword closes a gap that Python programmers have been working around for years. You can now defer an expensive import without hiding it inside a function, and the deferral stays explicit, local, and straightforward to reason about. For command-line tools and short-lived scripts, the difference in startup time is something your users will notice.

**In this tutorial, you’ve learned that:**

- A **lazy import** binds its name right away and loads the module the first time something reads that name.
- Five `lazy` keywords took a command-line tool’s`--help` from 68 to 32 milliseconds and**cost nothing on the paths** that do use the modules.
- The **restrictions on `lazy` protect you** , and the`try` block ban keeps deferred failures from slipping past an`except ImportError` fallback.
- **`__lazy_modules__`** lets you ship the same file to Python 3.14 and 3.15 because**older versions** ignore it instead of refusing to parse it.
- Modules that **register, configure, or hook something** at import time have to**stay eager** , since a lazy import that nobody reads never runs.

The fastest way to find out whether this matters for your code is to measure it. Run your own entry point under `-X importtime`, look at what loads before your program does anything, and ask how much of it the current code path actually needs.

Real Python’s guide to profiling in Python covers the tooling for the rest of your runtime, and the Python import system tutorial goes deeper into how imports work under the hood, building on the basics in Python Modules and Packages.

Now that you have some experience with deferred imports in Python 3.15, you can use the questions and answers below to check your understanding and recap what you’ve learned.

These FAQs are related to the most important concepts you’ve covered in this tutorial. Click the *Show/Hide* toggle beside each question to reveal the answer.

It’s an import that binds the name right away but doesn’t load the module until something uses that name. Python 3.15 adds a `lazy` keyword that you put in front of a normal import statement to get this behavior.

They cut startup time, sometimes dramatically, by skipping imports that your current code path never uses. They don’t speed up your program once it’s running, because a resolved lazy import behaves exactly like a normal one.

Because some modules do their work at import time. A plugin that registers itself, a module that configures logging, or one that installs a hook will never run if nothing reads its name. That’s fine for an application you control and risky for a library.

Sometimes. Marking either side of a two-module cycle lazy is often enough to break it. If each module needs a value the other computes while it’s still initializing, then deferral doesn’t help, and you need to restructure the code.

At the top of your module, assign a `__lazy_modules__` collection that lists the fully qualified names you want deferred. Python 3.15 honors it and defers those imports, while older versions ignore the assignment and import eagerly, so the same file runs everywhere.

 **Test your knowledge with our interactive “Python 3.15 Preview: Lazy Imports” quiz. You’ll receive a score upon completion to help you track your learning progress:**
