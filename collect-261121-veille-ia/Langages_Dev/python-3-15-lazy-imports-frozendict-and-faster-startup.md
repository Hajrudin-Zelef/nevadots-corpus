---
id: collect-261121-veille-ia/langages-dev/python-3-15-lazy-imports-frozendict-and-faster-startup
title: "Python 3.15: Lazy Imports, frozendict, and Faster Startup"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-10-12"]
keywords: ["imports", "lazy", "python", "import", "startup", "json", "release", "module"]
source: https://www.danilchenko.dev/posts/python-3-15/
source_anchor: ""
source_lines: [1, 206]
section: "Langages_Dev"
---
## TL;DR

Python 3.15.0 shipped on October 9, 2026, and it’s a stable release you can install today. The headline additions are a `lazy import` statement that defers module loading until first use, a real `frozendict` builtin, `*`/`**` unpacking inside comprehensions, and a statistical sampling profiler (Tachyon) that can attach to a running process at up to a million samples per second. The official release notes report a 7–8% geometric-mean JIT speedup on x86-64 Linux and 11–12% on AArch64 macOS, and the garbage collector stays on the generational design after the 3.14 incremental GC was reverted for eating memory in production. None of the new syntax is on by default, so upgrading won’t break your code, but a few of these features are worth rewriting for.

*Release date and JIT figures last verified against python.org: October 12, 2026.*

I read through the 3.15 what’s-new and ran every new snippet against my own 3.12.3 install to see exactly what changes and what stays a workaround. The 3.12 side of each contrast below is real output from that run; the 3.15 side comes from the official docs.

## Why I care about a point release

Most Python releases give you three headline features and forty stdlib footnotes you’ll never notice. The usual routine is to skim the what’s-new page, bump the CI matrix, and move on. Python 3.15 is worth reading twice, because two of its features fix problems most of us have hand-coded around for years: slow startup from eager imports, and the absence of a hashable read-only dict.

Instead of summarizing the doc, I did what I’d tell anyone to do before an upgrade. I opened a REPL on my current interpreter, typed each new snippet, and captured what happened. Here’s my starting point:

```
$ python3 --version
Python 3.12.3
```
Everything marked “3.12” below is genuine output from that shell. Everything marked “3.15” is from the official what’s-new document, since this server has no 3.15 interpreter and I’m not going to fake a run and call it hands-on.

## Lazy imports finally land (PEP 810)

This is the one I’d been waiting for. Big applications pay a startup tax for imports they might never touch on a given code path. A CLI that imports `xml.etree.ElementTree` at the top of a module pays for it even when the user runs `--help`.

Measured on this blog’s build server (Python 3.12.3) with `-X importtime`:

```
$ python3 -X importtime -c "import json, argparse, decimal, xml.etree.ElementTree"
...
import time:      1763 |       7397 | xml.etree.ElementTree
```
That’s 7.4 milliseconds of cumulative import time (7-13 ms across repeated runs) for a single tree module you might not use. Multiply across a dependency graph and startup creeps past a second for tools that should feel instant.

The old fix was to shove imports inside functions, which works but scatters your dependencies and confuses every linter and type checker. Python 3.15 gives you a real statement instead:

```
lazy import json
lazy from pathlib import Path
print("Starting up...")   # neither json nor pathlib is loaded yet
data = json.loads('{"key": "value"}')   # json is imported right here, on first use
```
On 3.12 that syntax doesn’t exist. My REPL is blunt about it:

```
--- lazy import keyword? ---
SyntaxError: invalid syntax
```
Two things kept me from getting over-excited. First, `lazy` is a soft keyword and the feature is opt-in per import; it won’t silently change how existing code loads. Second, there’s a global switch (`-X lazy_imports` or the `PYTHON_LAZY_IMPORTS` env var) plus `sys.set_lazy_imports()` for turning it on process-wide, but PEP 810 restricts lazy imports to module scope, with no star imports and no `__future__` imports. That’s a sane boundary. If you maintain a CLI or a plugin host, this is the single feature most likely to earn its keep. If you’re writing a web service that imports everything at boot anyway, you’ll feel it less. I put real numbers on that tradeoff in a dedicated lazy-imports benchmark, measuring `boto3`, `requests`, and a small CLI before and after.

## frozendict: a real immutable mapping (PEP 814)

For years the “read-only dict” answer in Python was `types.MappingProxyType`. It’s a live view onto another dict, and it isn’t hashable, so you can’t use it as a dict key or drop it in a set. Here’s the wall I hit on 3.12:

```
--- current read-only dict idiom (MappingProxyType) ---
proxy: {'x': 1}
hashable? no -> unhashable type: 'dict'
```
And plain `frozendict` simply doesn’t exist yet:

```
--- frozendict builtin? ---
NameError: name 'frozendict' is not defined
```
Python 3.15 promotes `frozendict` to a builtin (PEP 814). It’s immutable and, as long as its keys and values are hashable, it’s hashable too. The what’s-new doc shows the shape:

```
>>> a = frozendict(x=1, y=2)
>>> a['z'] = 3
TypeError: 'frozendict' object does not support item assignment
>>> b = frozendict(y=2, x=1)
>>> hash(a) == hash(b)
True
>>> a == b
True
```
Note the last two lines: equality and hashing ignore insertion order, which is what you want for a value type. Several stdlib modules learned to speak it too, including `json`, `pickle`, `copy`, and `pprint`, and `json.loads` gains an `array_hook` so you can decode a payload straight into nested `frozendict` plus `tuple` and get a genuinely immutable structure back. When raw decode speed on that path is what you care about, a typed library still wins — see msgspec vs Pydantic. If you’ve ever built a cache keyed on a config dict and reached for `frozenset(d.items())`, this replaces that hack.

## Unpacking inside comprehensions (PEP 798)

This one is small, but it fixes a real annoyance. Per PEP 798 you can now splat with `*` and `**` inside list, set, and dict comprehensions and generator expressions. Flattening a list of lists stops needing the double-`for` incantation that everyone has to re-read twice.

My 3.12 shell rejects it outright:

```
--- unpacking in comprehension? ---
SyntaxError: iterable unpacking cannot be used in comprehension
```
In 3.15 the same line runs, per the doc:

```
>>> lists = [[1, 2], [3, 4], [5]]
>>> [*L for L in lists]
[1, 2, 3, 4, 5]
>>> dicts = [{'a': 1}, {'b': 2}, {'a': 3}]
>>> {**d for d in dicts}
{'a': 3, 'b': 2}
```
It reads the way you’d say it out loud: for each list, spread its items. Expect it to quietly delete a lot of `itertools.chain(*lists)` calls.

## The Tachyon sampling profiler (PEP 799)

If you’ve only ever reached for `cProfile`, you know its problem: it instruments every call, which slows the target down enough to distort the very numbers you’re chasing, and you can’t point it at a process that’s already running and misbehaving in production.

Python 3.15 reorganizes profiling into a dedicated `profiling` package (PEP 799). `profiling.tracing` is the deterministic profiler you already know (`cProfile` stays as an alias), and `profiling.sampling` is new. It’s a statistical sampler, nicknamed Tachyon, that pokes the interpreter at intervals instead of wrapping every call. The what’s-new doc puts the ceiling at up to 1,000,000 Hz, and because it attaches to a live process, you can profile something that’s stuck without restarting it.

On 3.12 the module isn’t there at all:

```
--- sampling profiler module (profiling.sampling)? ---
ModuleNotFoundError: No module named 'profiling'
```
What sold me reading the changelog was the output modes. You get `--mode wall`, `--mode cpu`, and `--mode gil`, plus exporters for flamegraphs, Firefox-compatible Gecko traces, a heatmap, and a live TUI. There’s an `--async-aware` flag for coroutine-heavy code and `--opcodes` for bytecode-level detail. PEP 831 also turns on frame pointers by default in the official builds, which is the unglamorous plumbing that makes native stack unwinding reliable for exactly this kind of tool. For anyone who has tried to profile a wedged asyncio service, “attach to the running PID and get a flamegraph” is the whole pitch. I put the sampler through its paces in a separate hands-on Tachyon tutorial, including attaching to a live PID and reading the GIL view.

## Speed: the JIT and the GC U-turn

The two performance changes in 3.15 pull in opposite directions.

The JIT compiler, still experimental and off by default, gets a real upgrade in 3.15: a new tracing front end and register allocation for the generated machine code. The final what’s-new reports a 7–8% geometric-mean improvement on the pyperformance suite over a fully optimized standard interpreter on x86-64 Linux, and 11–12% over the tail-calling interpreter on AArch64 macOS. InfoWorld cites a slightly higher 8–13%. The per-benchmark spread is much wider than the headline: against a no-JIT build, results run from roughly 15% slower to more than twice as fast. I’d treat all of it as “reported until you benchmark your own code,” because JIT gains are famously workload-shaped, but the direction is right.

The garbage collector is the quieter and more interesting one. Python 3.14.0 through 3.14.4 shipped an incremental GC. Then enough people reported significant memory pressure in production that the core team reverted it, first in the 3.14.5 bugfix release and then in 3.15, which both use the generational collector from 3.13 and earlier. It’s a rare public U-turn, and I read it as a healthy sign: a change went out, real workloads pushed back with data, and the default flipped back. If you saw memory creep on an early 3.14 release, 3.14.5 or later already fixes it, and 3.15 keeps the fix.

## Quieter wins worth a mention

A few more that didn’t need their own section but will show up in real code.

The new `sentinel` builtin (PEP 661) retires a pattern everyone has hand-rolled: a unique “no value here” object that distinguishes “argument omitted” from “argument was `None`.” Here’s the boilerplate that’s still standard today, running fine on 3.12:

```
--- hand-rolled sentinel idiom (current) ---
MISSING repr: <MISSING> | identity kept: True
```
The 3.15 builtin produces a unique, picklable, copy-stable value with a clean repr, so that whole singleton class goes away.

On the typing side, `TypeForm` (PEP 747) lets a function annotate that it takes a *type expression* as a value, which is what libraries like `cast()` and validation frameworks have wanted forever. `TypedDict` also picks up `closed` and `extra_items` (PEP 728) so you can finally say “no keys beyond these” or “extra keys, but they must be `int`.” If you lint your code, pair this with a modern checker; I compared the current options in Pyrefly vs mypy vs ty.

The no-GIL build also gets its own stable ABI, `abi3t` (PEP 803), so C extensions can ship one wheel that keeps working across future free-threaded releases instead of recompiling every version. It’s boring infrastructure, and it’s the part that decides whether no-GIL Python is actually usable at scale. I dug into the runtime side of that story in Python 3.14 free-threading, and 3.15 is the packaging half of the same effort.

asyncio gets one addition: `TaskGroup.cancel()`, which ends a task group early without the old trick of raising a private exception and suppressing it on the way out. If you’re still deciding between task groups and `gather` in the first place, I ran both against the same failures in asyncio TaskGroup vs gather.

One default worth knowing about: Python now uses UTF-8 for I/O without an explicit encoding, independent of the system locale. You can opt out with `PYTHONUTF8=0`, but the long-running “why is this file mojibake on Windows” class of bug is finally addressed where it should be.

## How the releases stack up

Here’s the short version of where each recent feature lives, so you know what you’re actually getting when you bump the interpreter.

| Feature | Version | Default? | Who benefits most | 
|---|---|---|---|
| Free-threading (no-GIL) build | 3.13 experimental → 3.14 supported | Off (separate build) | CPU-bound multithreaded code | 
| Incremental GC | 3.14.0 (added) → 3.14.5 and 3.15 (reverted) | Back to generational | Anyone who saw memory creep | 
| `lazy import` statement | 3.15 | Off (opt-in per import) | CLIs, plugin hosts, slow-start apps | 
| `frozendict` builtin | 3.15 | Always available | Cache keys, config values | 
| Unpacking in comprehensions | 3.15 | Always available | Everyone flattening iterables | 
| Tachyon sampling profiler | 3.15 | Available in stdlib | Profiling live/production processes | 
| Upgraded JIT | 3.15 | Off (experimental, opt-in) | CPU-bound hot loops | 

## How to try Python 3.15 today

You don’t have to touch your system Python to kick the tires. The cleanest path is a version manager that installs an isolated build. With uv, grabbing 3.15 and opening a REPL is two commands (update uv first so it knows about the new release):

```
uv python install 3.15
uv run --python 3.15 python
```
From there, run the snippets above yourself: try `lazy import json`, build a `frozendict`, and flatten a list with `[*L for L in lists]`. If you’d rather stay on a package-manager-native flow, `pyenv install 3.15.0` does the same job once you’ve updated pyenv to pick up the new build definition. Either way, add 3.15 to one project’s CI matrix so the deprecation warnings surface before you move anything bigger. The full list of what’s changing across the stdlib is long, and if you’re modernizing a codebase it pairs well with the rest of the toolchain I walk through in the modern Python tooling guide.

One caution: 3.15.0 is a stable release, but the JIT is still experimental, and a .0 release is where the first round of regressions gets found. Let CI run on it for a while before moving production, or wait for the first bugfix release. Do read the deprecations, because 3.15 soft-deprecates `re.match()` in favor of the clearer `re.prefixmatch()` and schedules the old `profile` module for removal in 3.17.

## FAQ

### When was Python 3.15 released?

Python 3.15.0 was released on October 9, 2026, about a week after the October 1 date reported when the first beta came out. Beta 1 shipped on May 7, 2026, and the PEP 790 schedule puts bugfix releases on a roughly two-month cadence after that.

### Is Python 3.15 faster than 3.14?

For most code, startup is the visible win, thanks to lazy imports deferring module loading. The experimental JIT reports a 7–8% geometric-mean improvement on x86-64 Linux and 11–12% on AArch64 macOS, but it’s off by default, and gains depend heavily on your workload. The GC revert helps if you saw memory growth on 3.14.0 through 3.14.4, though 3.14.5 already carries the same fix.

### What is the new profiler in Python 3.15?

It’s Tachyon, exposed as `profiling.sampling`. Unlike `cProfile`, which instruments every function call, Tachyon takes statistical samples at up to a million hertz and can attach to a process that’s already running, then export flamegraphs, Gecko traces, or a live terminal view.

### Are lazy imports enabled by default in Python 3.15?

No. `lazy import` is opt-in per statement, and the process-wide switches (`-X lazy_imports`, `PYTHON_LAZY_IMPORTS`, `sys.set_lazy_imports()`) are also opt-in. Existing code keeps loading modules eagerly unless you ask otherwise, so upgrading won’t change your import behavior.

### Should I upgrade to Python 3.15 now?

Test on it first. 3.15.0 is a stable release, so running a project’s CI against it now is the cheap way to surface deprecation warnings and C-extension wheels that haven’t caught up yet. For production, waiting for the first bugfix release is the conservative call, and the JIT stays experimental either way.

## Sources

- What’s New in Python 3.15 — official documentation — the primary reference for every feature and code sample above, plus the JIT and GC figures
- Python 3.15.0 release page — python.org — release date (October 9, 2026) and downloads
- PEP 790 – Python 3.15 Release Schedule — beta, final, and bugfix release dates
- PEP 810 – Explicit Lazy Imports — the lazy import design and restrictions
- PEP 814 – Add frozendict built-in type — the frozendict spec
- PEP 798 – Unpacking in Comprehensions — the `*` /`**` comprehension syntax
- PEP 799 – A dedicated profiling package — the profiling reorg and Tachyon sampler
- PEP 803 – Stable ABI for free-threaded builds — abi3t for no-GIL C extensions
- The best new features in Python 3.15 — InfoWorld — independent coverage of the JIT and GC changes
- Feature freeze for Python 3.15 — The Register — beta 1 and feature-freeze context (it reported the then-planned October 1 release date)

## The bottom line

Python 3.15 is a quietly practical release. Lazy imports and `frozendict` alone justify reading the changelog, the profiler reorg finally makes production profiling first-class, and the GC revert shows the core team is willing to undo a shipped default when the data says so. Nothing here forces a rewrite, and nothing here breaks on upgrade. And for any CLI or plugin host, `lazy import` alone will shave real startup time, which is more than most point releases can say. 3.15.0 has been out since October 9, so add it to your CI matrix and let the deprecation warnings tell you what to fix first.
