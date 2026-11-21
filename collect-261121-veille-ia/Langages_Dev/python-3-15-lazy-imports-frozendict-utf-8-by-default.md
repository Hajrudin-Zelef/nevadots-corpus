---
id: collect-261121-veille-ia/langages-dev/python-3-15-lazy-imports-frozendict-utf-8-by-default
title: "Python 3.15: Lazy Imports, frozendict & UTF-8 by Default"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-10-09"]
keywords: ["lazy", "import", "json", "frozendict", "utf-8", "python", "release", "code"]
source: https://dev.to/ali_82f3409ed625e8ba8515b/python-315-lazy-imports-frozendict-utf-8-by-default-1gf3
source_anchor: ""
source_lines: [1, 97]
section: "Langages_Dev"
---
Python 3.15.0 landed on 9 October 2026, a week later than planned after a surprise third release candidate fixed late lazy-import bugs. It is the product of 5,643 commits from 1,012 contributors, and it is one of the most interesting Python releases in years — not because it changes how you write everyday code, but because it changes how your code starts up, stores configuration, and reads text files.

This is not a "rewrite everything" release. It is a "adopt two features, audit one default" release. Here is what actually matters, with working examples and the honest caveats the release notes gloss over.

The headline feature is PEP 810, explicit lazy imports. Write lazy import json instead of import json, and the module is not loaded until the name is first used. The name binds to a lightweight proxy; ordinary import statements keep their existing behaviour — nothing changes unless you opt in.


```
lazy import json          # json is NOT loaded yet
lazy from json import dumps  # also fine
print("CLI responds instantly")  # json still unloaded
data = json.loads("{}")          # json loads here, on first use
```
The realistic use case is command-line tools, test runners, and serverless functions that drag in heavy dependency chains at startup:


```
# cli.py — starts fast, heavy deps load only when needed
lazy import pandas
lazy import matplotlib.pyplot as plt
def main(argv):
    if "--plot" not in argv:
        run_fast_path()        # pandas and matplotlib never load
    else:
        plt.plot(load_data())  # import happens here, not at startup
```
How big is the win? The accepted PEP reports startup-time reductions of roughly 50% to 70% on its own workloads — that is the PEP's claim on its workloads, not a promise about your application. Your gain depends entirely on how much of your import graph goes unused in a typical run. Tools with deep, rarely-touched dependency trees will feel it most.

Two things to watch before sprinkling lazy everywhere. First, it changes *when* module-level code runs: anything that configures logging, registers plugins, or validates the environment at import time now happens at first use. Second, lazy is a soft keyword, so existing code that uses lazy as a variable name keeps working — but double-check any metaprogramming or mocking setup that inspects modules at import time. The very fact that the release needed a surprise rc3 for lazy-import bugs is a reminder: try this on slow-starting tools first, not in your hottest path.

PEP 814 adds frozendict as a built-in type — a dictionary that cannot be changed after creation:


```
CONFIG = frozendict({"host": "db.internal", "port": 5432, "ssl": True})
CONFIG["port"] = 5433   # TypeError: 'frozendict' object is immutable
# Hashable when its contents are, so it works as a dict key or in a set
registry = {CONFIG: "primary"}
```
This retires the types.MappingProxyType workaround and the hand-rolled immutable config classes littering every codebase. Fixed settings, feature flags, and lookup tables become genuinely safe to share: no more defensive copies before handing a dict to untrusted code. If you have ever debugged a "config changed out from under me" bug, you already know why this matters.

PEP 686 makes UTF-8 the default text encoding everywhere, regardless of the platform locale. A bare open() call now reads and writes UTF-8 whether you run on Linux, macOS, or Windows. This is the one change in 3.15 that can alter existing behaviour, so it deserves the most attention.

On Windows this is a genuine quality-of-life fix. File operations previously followed the system locale — often cp1252 or similar — which mangled anything outside it. For European developers this was a weekly papercut: German umlauts, French accents, Polish ł, Nordic ø all broke the moment a script moved from a Linux server to a Windows laptop:


```
with open("customers.csv", "w") as f:   # UTF-8, on every platform now
    f.write("Müller,Schröder,Zürich\n")
```
The migration cost falls on code that *relied* on the old locale behaviour. If you read files written in a legacy encoding, you must now pass encoding= explicitly — the silent locale fallback is gone. The stopgap is the PYTHONUTF8=0 environment variable, which restores the old behaviour, but treat it as a bridge, not a destination: pin your encodings explicitly and your code becomes portable across platforms for free.

PEP 798 lets * and ** unpacking appear directly in comprehensions, at the top level of the expression:


```
chunks = [[1, 2], [3], [4, 5]]
flat = [*chunk for chunk in chunks]        # [1, 2, 3, 4, 5]
configs = [defaults, env_overrides, cli_overrides]
merged = {**c for c in configs}            # one dict; later keys win
```
Before 3.15 you wrote the nested loop — [x for chunk in chunks for x in chunk] — which the PEP notes is the version students and newcomers most often get backwards. The new form reads the way it behaves. Note the restriction: the star must sit at the top level of the comprehension expression, so you cannot nest it inside a larger expression.

PEP 661 finally standardises the sentinel pattern every library reinvents: a unique marker that distinguishes "not provided" from None. Instead of _MISSING = object() — which prints as the useless  in tracebacks — you get a named, identity-unique value:


```
MISSING = sentinel("MISSING")
def get_user(user_id, default=MISSING):
    user = db.lookup(user_id)
    if user is MISSING:
        if default is MISSING:
            raise KeyError(user_id)
        return default
    return user
```
Identity comparison with is is the rule, exactly as with None. Type checkers are already on board: mypy supports sentinel narrowing, so int | None | MISSING in a signature narrows correctly after an is check. This is a small feature with an outsized effect on API design — anywhere you currently overload None to mean both "missing" and "explicitly null", a sentinel is the honest fix.

The experimental JIT compiler keeps improving. The release benchmarks show a 7–8% geometric-mean gain on x86-64 Linux over the standard interpreter, and 11–12% on AArch64 macOS over the tail-calling interpreter. Note the different baselines, and note what "geometric mean" hides: individual benchmarks range from about 15% *slower* to more than twice as fast. The JIT is still experimental and still opt-in — treat these numbers as an average across a benchmark suite, not a promise about your workload.

The steadier speed story is on Windows: the official 64-bit installers now ship the tail-calling interpreter, worth a reported 15–20% on pure-Python code. The official macOS installers now include free-threading support by default, and frame pointers are enabled by default (PEP 831), which gives profilers and debuggers a much clearer view of a running programme.

PEP 799 adds a proper profiling package to the standard library, including Tachyon, a high-frequency statistical sampling profiler. More importantly for your backlog: the pure-Python profile module is deprecated and will be removed in Python 3.17. If any tooling still imports it, move to cProfile or the new profiling package now. Code-executing lines in .pth files are also deprecated in favour of the new package startup files from PEP 829.

A practical checklist:

- **Add 3.15 to CI early.** It landed in GitHub Actions' python-versions on 10 October, so a matrix entry is one line. Your test suite will find the encoding issues before your users do.
- **Audit text I/O, especially on Windows.** Any open() without an explicit encoding now means UTF-8. Run your file-handling tests on Windows; use PYTHONUTF8=0 only as a temporary stopgap.
- **Check .pth files.** Code-executing lines are deprecated — move to PEP 829 startup files.
- **Defer macOS 27.0 if you use tkinter or IDLE.** A Tk toolkit issue (tracked as issue #158053) can hang apps when a menu opens a dialog, requiring a force quit. This is an OS behaviour change, not a Python bug, but the timing is unfortunate.
- **Confirm key dependencies support 3.15** before touching production. The ABI is stable from rc3 onward, but extension wheels take weeks to catch up.

What remains unknown: the JIT is still experimental, so its trajectory is the thing to watch rather than its current numbers. Lazy imports only just survived a late bugfix round, so exotic metaprogramming — dynamic imports, import hooks, heavy patching in tests — deserves caution until the ecosystem has exercised it. And the release gets two years of bug fixes with security support until about October 2031, so there is no rush: 3.15 is a release you adopt deliberately, feature by feature.

Start with lazy import in your slowest CLI, reach for frozendict the next time you write a config, and grep your codebase for open( calls missing an encoding. That is the whole of Python 3.15 that matters this month — the rest can wait until you need it.

## Top comments (0)
