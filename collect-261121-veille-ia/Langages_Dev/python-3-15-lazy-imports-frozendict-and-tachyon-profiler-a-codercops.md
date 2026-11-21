---
id: collect-261121-veille-ia/langages-dev/python-3-15-lazy-imports-frozendict-and-tachyon-profiler-a-codercops
title: "Python 3.15: Lazy Imports, frozendict, and Tachyon Profiler â CODERCOPS"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-10-09"]
keywords: ["lazy", "imports", "python", "frozendict", "utf-8", "default", "encoding", "import"]
source: https://blog.codercops.com/blog/python-3-15-lazy-imports-frozendict-tachyon-profiler-2026
source_anchor: ""
source_lines: [1, 124]
section: "Langages_Dev"
---
Sponsored

Python 3.15 was released on October 9, 2026 (a week later than planned, after a surprise third release candidate), and the three changes worth your attention are explicit lazy imports, UTF-8 as the default file encoding, and a new sampling profiler that attaches to a live process. The rest is a long tail of standard library additions. If you maintain a CLI tool, a Lambda function or anything with a slow cold start, `lazy import` is the headline. If you maintain code that touches files on Windows, the encoding change is the one to test first.

This post covers what changed, with the code, and what we would do about each item. It is based on the Whatâs New in Python 3.15 documentation and the release announcements. For the encoding change in depth, see our follow-up on auditing your open() calls for the UTF-8 default.

## Lazy imports: the startup-time fix

Every Python developer has watched a tool take 400 ms to print `--help` because it imported pandas at the top of the file. The usual workaround is an import inside the function. It works, but it hides dependencies and linters complain.

PEP 810 gives you a statement for it:

```
lazy import json
lazy from pathlib import Path
print("Starting up...")        # json and pathlib are not loaded yet
data = json.loads('{"key": "value"}')   # json loads here, on first use
```
The name is bound immediately, but the module loads the first time you touch it. The draft docs also add `sys.set_lazy_imports()` and `sys.get_lazy_imports()` so you can inspect and control the behavior at runtime.

Where this pays off:

- **CLIs with many subcommands.** Only the subcommand you run pays for its imports.
- **Serverless handlers.** Cold start time is billed time.
- **Large apps with optional features.** Admin tooling, report exporters and the like.

Where it bites: an import that fails will now fail later, at first use, and not at startup. A missing optional dependency used to crash the process at boot. Now it crashes on the request that needs it. If you rely on import-time side effects (registering plugins, patching libraries), keep those imports eager.

## frozendict and sentinel are now built in

`frozendict` (PEP 814) is an immutable, hashable dictionary:

```
>>> a = frozendict(x=1, y=2)
>>> a['z'] = 3
TypeError: 'frozendict' object does not support item assignment
>>> hash(a)   # works if all keys and values are hashable
```
Because it hashes, it can be a dictionary key or a set member, and it can sit safely inside a cached functionâs arguments. That ends a small but constant annoyance: converting a config dict to a tuple of items just to memoize on it.

PEP 661 adds a `sentinel` type for unique marker values. If you have ever written `_MISSING = object()` and then wondered why it prints as `<object object at 0x7f...>` in a traceback, this is for you. It also gives `None`-versus-not-provided checks a standard spelling.

PEP 798 extends unpacking into comprehensions, which flattens nested data without the double `for`:

```
>>> lists = [[1, 2], [3, 4], [5]]
>>> [*L for L in lists]
[1, 2, 3, 4, 5]
>>> dicts = [{'a': 1}, {'b': 2}, {'a': 3}]
>>> {**d for d in dicts}
{'a': 3, 'b': 2}
```
Later keys win in the dict case, which matches what `{**a, **b}` already does.

## UTF-8 by default: read this one twice

PEP 686 makes Python use UTF-8 for text I/O regardless of the system locale. On Linux and macOS, where the locale is almost always UTF-8 already, you will notice nothing. On Windows, where `open('file.txt')` historically used the legacy ANSI code page, it changes behavior.

`open('file.txt')   # now UTF-8 by default`
Two groups benefit and one gets hurt. Cross-platform code stops producing different bytes on different machines. People who never passed `encoding=` stop seeing mojibake. The group hurt is anyone with existing files written in a legacy encoding and read back with the implicit default. Those reads can now raise `UnicodeDecodeError`.

Our advice: grep for `open(` calls without an `encoding` argument, decide which files are really UTF-8, and pass the encoding explicitly in the ones that are not. If you need a quick escape hatch during migration, `PYTHONUTF8=0` or `-X utf8=0` turns the new default off.

## Tachyon: a sampling profiler you can attach to production

PEP 799 adds a `profiling` package. `profiling.tracing` is the old deterministic `cProfile`, moved. `profiling.sampling` is new, and it is called Tachyon:

```
python -m profiling.sampling --mode wall --pstats script.py
python -m profiling.sampling --mode cpu script.py
python -m profiling.sampling --mode gil script.py
python -m profiling.sampling --flamegraph script.py
python -m profiling.sampling attach --pid=12345
```
The docs describe sampling rates up to 1,000,000 Hz, four useful modes (wall clock, CPU time, GIL holding time, and flamegraph output as interactive HTML), and an `attach` command for a process that is already running. A sampling profiler does not instrument every function call, so overhead stays low enough to point at a live service. A tracing profiler distorts the very timings you are trying to measure.

The GIL mode is the interesting one if you have been following Pythonâs free-threaded work. It shows how long threads hold the lock, which tells you whether threading helps your workload before you move to a free-threaded build. If your service is async rather than threaded, pair it with the failure modes in our asyncio production pitfalls piece.

Frame pointers are now enabled by default (PEP 831), which lets system-level tools such as `perf` walk Python stacks with less guesswork.

## The JIT, free-threading and the smaller stuff

The 3.15.0rc1 announcement reports an 8-9% geometric mean speedup from the upgraded experimental JIT on x86-64 Linux, with larger gains on macOS ARM64. Treat that as a geometric mean over a benchmark suite. Your workload will differ, and the JIT is still experimental. Official Windows and macOS binaries now ship with free-threading support by default, and C extensions can target free-threaded builds through a new stable ABI (`abi3t`).

A few smaller additions you may actually use:

| Change | Why it matters | 
|---|---|
| `asyncio.TaskGroup.cancel()` | Stop a whole group early without raising a sentinel exception | 
| `tomllib` TOML 1.1.0 | Newlines allowed inside inline tables | 
| `re.prefixmatch()` | An explicit name for what `re.match()` does;`re.match()` is soft-deprecated | 
| Better `AttributeError` hints | `[].push(4)` now suggests`.append` ;`'x'.toUpperCase()` suggests`.upper` | 
| `sqlite3` CLI | Keyword, table and column completion, colored output | 

Several long-deprecated APIs are gone, including the `sre_*` modules and `platform.java_ver()`. Run your suite with `-W error::DeprecationWarning` on 3.14 first and you will see most of the breakage ahead of time.

## What we would do this week

1. Install 3.15.0 in a throwaway environment and run your test suite. Do not touch production.
2. Search for `open(` without`encoding=` . That is the most likely source of real breakage.
3. Pick one slow-starting CLI and try `lazy import` on its heaviest dependencies. Measure with`python -X importtime` before and after.
4. Point Tachyon at a staging service under load before you need it in an incident.
5. Check that your key dependencies publish 3.15 wheels, then schedule the production upgrade after the first bugfix release.

If your team is weighing a Python upgrade alongside a larger backend project, our engineering services cover migrations like this one. The cheapest first step is still the one above: run the tests on the release candidate and read what fails.

## Frequently asked questions

- When is Python 3.15 released?
- Python 3.15.0 final was released on October 9, 2026. It was originally planned for October 1, but a third release candidate (rc3, October 2) was added after lazy-import release blockers were found. Treat the first weeks as early adopter territory and wait for bugfix releases before moving production.
- What is a lazy import in Python 3.15?
- It is a new statement, `lazy import json` or `lazy from pathlib import Path`, that binds the name but delays loading the module until you first use it. It is defined in PEP 810 and exists to cut startup time in programs that import many modules they may never touch.
- Will UTF-8 by default break my code?
- It can, in a narrow case. Code that opens files without an `encoding` argument on a system whose locale was not UTF-8 will now read and write UTF-8 instead of the locale encoding. Files you wrote earlier in that legacy encoding may fail to decode. You can restore the old behavior with `PYTHONUTF8=0` or `-X utf8=0` while you migrate.
- Should I upgrade production to Python 3.15 right away?
- Not on day one. Run your test suite against the release candidate now, wait for your dependencies to publish 3.15 wheels, and plan the production move for after the first bugfix releases. The same advice applied to 3.14, which we covered in [what actually mattered in Python 3.14](/blog/python-3-14-whats-new-2026).

Sources

Sponsored

Sponsored

Sponsored
