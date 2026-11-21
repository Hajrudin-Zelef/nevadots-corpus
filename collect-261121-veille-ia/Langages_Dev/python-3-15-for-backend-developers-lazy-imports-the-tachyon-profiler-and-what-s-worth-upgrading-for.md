---
id: collect-261121-veille-ia/langages-dev/python-3-15-for-backend-developers-lazy-imports-the-tachyon-profiler-and-what-s-worth-upgrading-for
title: "Python 3.15 for Backend Developers: Lazy Imports, the Tachyon Profiler and What's Worth Upgrading For"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["lazy", "import", "imports", "python", "tachyon", "first", "does", "module"]
source: https://zyvop.com/python-3-15-for-backend-developers-lazy-imports-the-tachyon-profiler-and-what-s-worth-upgrading-for-cc5dq
source_anchor: ""
source_lines: [1, 645]
section: "Langages_Dev"
---
Python 3.15.0 was released on October 9, 2026. The headline features are lazy imports, a built-in sampling profiler called Tachyon, UTF-8 as the default text encoding, and two new built-in types, `frozendict` and `sentinel`.

A list of features doesn't tell you whether to upgrade a web service. So this post does two practical jobs and then gives a verdict.

First, you'll **measure your app's startup time** and see how much `lazy import` could save. Second, you'll **profile a slow endpoint** with Tachyon, with no code changes and no restart.

**About the numbers:** The helper scripts in this post run on any recent Python. The sample outputs come from Python 3.12 running them. The two 3.15-only steps, `lazy import` and Tachyon, are commands to run on your own app, because startup time and profiles depend on your dependencies.


## 3.15 at a glance

| Feature | PEP | What it does | Why a backend developer cares | 
|---|---|---|---|
| **Lazy imports** | 810 | `lazy import x` waits until`x` is first used | Faster startup and cold starts | 
| **Tachyon profiler** | 799 | `python -m profiling.sampling` samples a running process | Profile production without touching the code | 
| `frozendict` | 814 | A built-in immutable, hashable mapping | Config objects and cache keys | 
| `sentinel` | 661 | Unique placeholder values with a clean repr | "Argument not provided" defaults | 
| **UTF-8 by default** | 686 | Text files open as UTF-8 on every platform | Same behavior on Windows and odd-locale containers | 
| **Frame pointers on** | 831 | Better native stack traces for `perf` and eBPF | Profiling tools see through the interpreter | 
| **Unpacking in comprehensions** | 798 | `[*group for group in groups]` | Less boilerplate for flattening | 
| **Experimental JIT** | n/a | python.org reports 7% to 8% on a geometric-mean benchmark mix, on x86-64 Linux | Interesting, but experimental | 

We'll spend most of the time on the first two. Then we'll cover the smaller changes, and finish with what to check before you upgrade.

## Lazy imports

### The problem

An import statement does real work when it runs. It finds the module, executes it, and everything that module imports in turn. In a web service that happens once per worker process, before the first request is served.

For a FastAPI app, a handful of heavy libraries can dominate that time. Many of them are used by only one or two endpoints, such as a data library for an export route or a cloud SDK for an admin route. Yet every worker pays for them at startup.

### What `lazy import` does

PEP 810 adds a keyword for deferring that work:

```
lazy import json
lazy from pathlib import Path
```
The name is bound right away, but to a placeholder. The real import runs the first time you use the name. After that it behaves like any other import.

```
flowchart LR
    subgraph Eager["Regular import"]
        A1["Start worker"] --> A2["Import every library<br/>(slow)"] --> A3["Ready to serve"]
    end
    subgraph Lazy["lazy import"]
        B1["Start worker"] --> B2["Bind placeholders<br/>(fast)"] --> B3["Ready to serve"]
        B3 --> B4["First use of a lazy name<br/>loads the module"]
    end
```
The rules that matter:

- **Module level only.** A`lazy import` inside a function, a class body or a`try` block is a`SyntaxError` .
- **No star imports.**`lazy from x import *` isn't allowed either.
- **Errors move.** A missing module or a broken import now fails at first use, not at startup. The traceback points to both places.
- **Side effects move too.** Anything a module does when it's imported, like registering itself with a framework, happens later.

There are also ways to turn it on without editing code, and a way to opt libraries in:

| How | What it does | 
|---|---|
| `lazy import x` | Lazy for this one import | 
| `-X lazy_imports=all` or`PYTHON_LAZY_IMPORTS=all` | Makes imports lazy by default across the program. The other value, `normal` , is the default, and only honors the`lazy` keyword | 
| `__lazy_modules__ = ["x", "y"]` | A module-level list. Plain `import x` statements for those names become lazy. Older Pythons simply ignore it, so libraries can use it without dropping support for them | 
| `sys.set_lazy_imports()` | Changes the mode at runtime | 

### Step 1: See where startup time goes

You can't decide what to make lazy without knowing what's slow. Python has had `-X importtime` for years, but its raw output is hard to read. This script turns it into a ranking:

```
"""Summarise `python -X importtime`: which imports cost the most at startup?
    python importtime_report.py app                         # imports app, shows the top 15
    python importtime_report.py app -X lazy_imports=all     # Python 3.15: make every import lazy
"""
import argparse
import re
import subprocess
import sys
from collections import defaultdict
from dataclasses import dataclass
LINE = re.compile(r"^import time:\s+(\d+)\s+\|\s+(\d+)\s+\|( +)(\S.*)$")
@dataclass(frozen=True)
class Import:
    name: str
    self_us: int        # time spent in this module's own code
    cumulative_us: int  # ...plus everything it imported
    depth: int          # 0 = imported directly by your code
def parse(text: str) -> list[Import]:
    imports = []
    for line in text.splitlines():
        match = LINE.match(line)
        if match:
            self_us, cumulative_us, indent, name = match.groups()
            imports.append(Import(name, int(self_us), int(cumulative_us), (len(indent) - 1) // 2))
    return imports
def total_us(imports: list[Import]) -> int:
    return sum(i.cumulative_us for i in imports if i.depth == 0)
def by_package(imports: list[Import]) -> dict:
    """Self time per top-level package. Self times add up to the total, so nothing is counted twice."""
    totals = defaultdict(int)
    for i in imports:
        totals[i.name.split(".")[0]] += i.self_us
    return dict(sorted(totals.items(), key=lambda item: item[1], reverse=True))
def saved(before: list[Import], after: list[Import]) -> dict:
    """Packages whose startup cost went down, and by how many microseconds."""
    old, new = by_package(before), by_package(after)
    gains = {name: old[name] - new.get(name, 0) for name in old}
    return dict(sorted(((n, g) for n, g in gains.items() if g > 0), key=lambda item: item[1], reverse=True))
def run_importtime(module: str, python: str = sys.executable, xoptions=(), cwd=None) -> list[Import]:
    # __import__ is a function call, not an import statement, so it stays eager even when you test
    # with -X lazy_imports=all. Only the imports INSIDE your app become lazy. (importlib.import_module
    # would also be eager, but -X importtime wouldn't time the app itself.)
    code = f"__import__({module!r})"
    command = [python, "-X", "importtime", *[arg for x in xoptions for arg in ("-X", x)], "-c", code]
    result = subprocess.run(command, capture_output=True, text=True, check=True, cwd=cwd)
    return parse(result.stderr)  # importtime writes to stderr
def main(argv=None):
    parser = argparse.ArgumentParser(description=__doc__, formatter_class=argparse.RawDescriptionHelpFormatter)
    parser.add_argument("module")
    parser.add_argument("--top", type=int, default=15)
    parser.add_argument("-X", dest="xoptions", action="append", default=[], help="extra -X option, e.g. lazy_imports=all")
    args = parser.parse_args(argv)
    imports = run_importtime(args.module, xoptions=args.xoptions)
    print(f"{args.module}: {total_us(imports) / 1000:.1f} ms in {len(imports)} imports\n")
    print("Self time by package:")
    for name, us in list(by_package(imports).items())[: args.top]:
        print(f"  {name:<24} {us / 1000:8.1f} ms")
    print("\nSlowest single imports (cumulative):")
    for i in sorted(imports, key=lambda i: i.cumulative_us, reverse=True)[: args.top]:
        print(f"  {i.name:<32} {i.cumulative_us / 1000:8.1f} ms")
if __name__ == "__main__":
    main()
```
Run it against your app module:

`python importtime_report.py app`
Here's what the report looks like. This example imports `asyncio` from the standard library, on Python 3.12. Your app will look different, and the timings vary from run to run:

```
asyncio: 80.9 ms in 128 imports
Self time by package:
  asyncio                      19.8 ms
  ssl                           7.7 ms
  _ssl                          6.5 ms
  typing                        6.0 ms
  inspect                       3.6 ms
Slowest single imports (cumulative):
  asyncio                              75.7 ms
  asyncio.base_events                  68.0 ms
  concurrent.futures                   16.9 ms
  ssl                                  15.3 ms
  concurrent.futures._base             15.1 ms
```
The first list ranks packages by **self time**, which is the time a module spends in its own code. Self times add up to the total, so nothing is counted twice. The second list shows the slowest single imports, including everything each one pulled in.

Look for libraries near the top that only a few endpoints use. Those are your candidates.

**A detail that matters.** The script imports your module with `__import__('app')`, not `importlib.import_module('app')`. With `import_module`, `-X importtime` doesn't time the app module itself, and everything it imports shows up as top-level. And a plain `import app` in a `-c` string would be a module-level import, so under `-X lazy_imports=all` it would be deferred too, and nothing would load. A call to `__import__` avoids both problems.


### Step 2: Time it

Startup time is noisy. This helper runs a command several times, skips the warm-up runs, and reports the minimum and median:

```
"""Time a command several times. Run it twice to compare before and after.
    python startup_bench.py -- python -c "__import__('app')"
    PYTHON_LAZY_IMPORTS=all python startup_bench.py -- python -c "__import__('app')"
"""
import argparse
import statistics
import subprocess
import time
def summarize(samples: list[float]) -> dict:
    return {"runs": len(samples), "min": min(samples), "median": statistics.median(samples), "max": max(samples)}
def bench(command: list[str], runs: int = 15, warmup: int = 2) -> dict:
    samples = []
    for i in range(warmup + runs):
        start = time.perf_counter()
        subprocess.run(command, check=True, capture_output=True)
        elapsed = time.perf_counter() - start
        if i >= warmup:  # the first runs warm the disk cache and .pyc files
            samples.append(elapsed)
    return summarize(samples)
def main(argv=None):
    parser = argparse.ArgumentParser(description=__doc__, formatter_class=argparse.RawDescriptionHelpFormatter)
    parser.add_argument("--runs", type=int, default=15)
    parser.add_argument("command", nargs="+")
    args = parser.parse_args(argv)
    stats = bench(args.command, args.runs)
    print(f"{stats['runs']} runs: min {stats['min'] * 1000:.0f} ms, median {stats['median'] * 1000:.0f} ms, max {stats['max'] * 1000:.0f} ms")
if __name__ == "__main__":
    main()
```
`python startup_bench.py -- python -c "__import__('app')"`
On Python 3.12, the same command for `asyncio` reports:

`15 runs: min 59 ms, median 60 ms, max 79 ms`
Trust the median for comparisons and the minimum as the best case. The first runs warm the disk cache and the `.pyc` files, which is why they're skipped.

### Step 3: Find the upper bound without changing code

Before you edit anything, ask Python 3.15 how much laziness could possibly save:

`PYTHON_LAZY_IMPORTS=all python startup_bench.py -- python -c "__import__('app')"`
That makes every module-level import lazy by default. It's an experiment, not a deployment setting, because it changes when side effects run. But it gives you a ceiling. If the median barely moves, `lazy import` won't do much for you, and you can stop here.

The same flag works with the report, and a few lines show which packages disappeared from startup:

```
from importtime_report import run_importtime, saved
before = run_importtime("app")
after = run_importtime("app", xoptions=["lazy_imports=all"])
for name, us in list(saved(before, after).items())[:10]:
    print(f"{name:<20} {us / 1000:6.1f} ms")
```
The tests prove the mechanism. `test_a_module_that_is_never_imported_costs_nothing_and_shows_up_in_saved` builds a module that sleeps for 50 ms on import. One app imports it eagerly, and another defers it. The report shows the 50 ms saved, because a module that never loads never costs anything.

### Step 4: Make the right imports lazy

Now apply it where it pays. In a FastAPI app that's usually a heavy library that only one route uses. This is Python 3.15 syntax:

```
from fastapi import FastAPI
lazy import pandas as pd  # only /export uses it, so don't load it at startup
app = FastAPI()
@app.get("/export")
def export():
    return pd.DataFrame(...).to_dict()  # pandas loads here, the first time
```
### What lazy imports won't speed up

- **Anything your route signatures touch.** FastAPI inspects each route function's signature when the decorator runs. Classes used in type hints are touched at import time, so they load then, lazy or not.
- **Libraries you need at startup anyway.** FastAPI itself, your settings, your database setup.
- **Steady-state speed.** Lazy imports move cost, they don't remove it. A server that stays up for weeks pays the cost on the first use instead of at startup.

That last point matters for latency. The first request that touches a lazy library will be slower. If that's a problem, load it during startup by touching the name:

```
from contextlib import asynccontextmanager
@asynccontextmanager
async def lifespan(app):
    pd.DataFrame  # using the name loads pandas now, not on the first request
    yield
app = FastAPI(lifespan=lifespan)
```
Warm-up makes sense for libraries on a hot path. Skip it for ones that really are rarely used.

### Where it goes wrong

| Risk | What to do | 
|---|---|
| A broken import now fails at first use, maybe in production | Run your tests with `-X lazy_imports=all` and exercise every route | 
| A module registers something when imported, like ORM models, plugins or signal handlers | Keep those imports regular. Use `lazy` for leaf libraries | 
| `try: import x` /`except ImportError` patterns | `lazy` isn't allowed in`try` , so keep these as they are | 
| Circular imports behave differently | Test after every change. Don't make lazy imports a fix for a cycle | 

## The Tachyon sampling profiler

### Why sampling

Python has long had a built-in profiler, `cProfile`. It's a *tracing* profiler. It hooks every function call and return, so it knows exact call counts. The price is overhead on every call.

A *sampling* profiler works differently. It looks at the program's call stack many times a second and counts where it finds the program. It's less exact but costs far less, because it isn't hooked into your code.

Tachyon is Python 3.15's sampling profiler, and it's in the standard library. Its docs say that because it samples from outside the target process, overhead is virtually zero. It also attaches to a running process by PID, so you can profile a live server without editing or restarting it.

How much does tracing cost? This script times a function three ways:

```
import cProfile
import time
from report import build_report_fast, build_report_slow, make_orders
orders = make_orders(2000)
def timed(fn, *args):
    best = float("inf")
    for _ in range(3):
        start = time.perf_counter()
        fn(*args)
        best = min(best, time.perf_counter() - start)
    return best
plain = timed(build_report_slow, orders)
profiler = cProfile.Profile()
under_cprofile = timed(lambda: profiler.runcall(build_report_slow, orders))
fixed = timed(build_report_fast, orders)
print(f"slow report, no profiler:    {plain * 1000:7.1f} ms")
print(f"slow report, under cProfile: {under_cprofile * 1000:7.1f} ms  ({under_cprofile / plain:.1f}x slower)")
print(f"fast report:                 {fixed * 1000:7.1f} ms  ({plain / fixed:.0f}x faster than the slow one)")
```
```
slow report, no profiler:       43.3 ms
slow report, under cProfile:   180.3 ms  (4.2x slower)
fast report:                     0.3 ms  (161x faster than the slow one)
```
On Python 3.12, `cProfile` made this function about four times slower. The function calls two tiny helpers inside its hot loop, which is typical of real code, and a tracing profiler charges for every call. That also distorts the picture, because the tiny helpers look more expensive than they are.

### The slow endpoint

Here's an endpoint with two classic problems. This is the logic:

```
import random
import time
def make_orders(n: int, seed: int = 7) -> list[dict]:
    rng = random.Random(seed)
    customers = [f"cust_{i}" for i in range(max(1, n // 4))]
    return [{"id": i, "customer": rng.choice(customers), "amount": rng.randrange(100, 20_000)} for i in range(n)]
def fetch_orders(n: int, latency: float = 0.05) -> list[dict]:
    """Stands in for a database call: it mostly waits."""
    time.sleep(latency)
    return make_orders(n)
def belongs_to(order: dict, customer: str) -> bool:
    return order["customer"] == customer
def amount_of(order: dict) -> int:
    return order["amount"]
def build_report_slow(orders: list[dict]) -> dict:
    customers = []
    for order in orders:
        if order["customer"] not in customers:  # hotspot 1: scans the whole list for every order
            customers.append(order["customer"])
    revenue = {}
    for customer in customers:  # hotspot 2: reads every order again for every customer
        revenue[customer] = sum(amount_of(o) for o in orders if belongs_to(o, customer))
    top = sorted(revenue.items(), key=lambda item: item[1], reverse=True)[:5]
    return {"customers": len(customers), "orders": len(orders), "top": top}
def build_report_fast(orders: list[dict]) -> dict:
    revenue = {}
    for order in orders:  # one pass, and dict lookups instead of list scans
        revenue[order["customer"]] = revenue.get(order["customer"], 0) + order["amount"]
    top = sorted(revenue.items(), key=lambda item: item[1], reverse=True)[:5]
    return {"customers": len(revenue), "orders": len(orders), "top": top}
```
And the FastAPI app around it:

```
from fastapi import FastAPI
from report import build_report_fast, build_report_slow, fetch_orders
app = FastAPI()
@app.get("/health")
def health():
    return {"status": "ok"}
@app.get("/report")
def report(n: int = 2000):
    return build_report_slow(fetch_orders(n))
@app.get("/report-fast")
def report_fast(n: int = 2000):
    return build_report_fast(fetch_orders(n))
```
`build_report_slow` has two hotspots:

1. `not in customers` **on a list.** It scans the whole list for every order.
2. **A second loop that reads every order again for every customer.** That's the real cost, at about orders times customers.

`fetch_orders` waits 50 ms, standing in for a database call.

The fix is one pass with a dictionary, in `build_report_fast`. The tests check that the two versions return identical results for several sizes, including empty input, and that the slow one really is more than 20 times slower on 1,500 orders. On Python 3.12, the 2,000-order report takes about 43 ms in the slow version, and about 0.3 ms in the fast one.

### Profile it

Start with a script, because it's the simplest case:

```
from report import build_report_slow, make_orders
orders = make_orders(2000)
for _ in range(5):
    build_report_slow(orders)
```
`python -m profiling.sampling run slow_report.py`
By default that prints a text table in the style of `pstats`. For a picture, ask for a flame graph, which is an HTML file:

`python -m profiling.sampling run --flamegraph -o flame.html slow_report.py`
Then try the real server. Start it as usual, find its PID, and attach while you send traffic:

`uvicorn app:app --port 8000````
PID=$(pgrep -f "uvicorn app:app" | head -n 1)
python -m profiling.sampling attach --all-threads --duration 20 --flamegraph -o report.html "$PID"
```
While it samples, hit the endpoint from another terminal:

`for i in $(seq 50); do curl -s "localhost:8000/report?n=2000" > /dev/null; done`
Note `--all-threads`. Tachyon samples only the main thread by default. A plain `def` endpoint in FastAPI runs in a worker thread, so without that flag you'd see the event loop idling and miss your code.

Here are the options you'll use most:

| Option | What it does | 
|---|---|
| `run script.py` or`run -m module` | Start a program under the profiler | 
| `attach PID` | Profile a process that's already running | 
| `dump PID` | Take a single snapshot of what every thread is doing. Handy for a hung process | 
| `-d` ,`--duration` | How long to sample | 
| `-r` ,`--sampling-rate` | Samples per second. The default is 1 kHz | 
| `-a` ,`--all-threads` | Sample every thread, not just the main one | 
| `--mode wall` ,`cpu` ,`gil` ,`exception` | What to count. See below | 
| `--async-aware` | Follow `await` chains in async code | 
| `--live` | A live, top-style view | 
| `--flamegraph` ,`--heatmap` ,`--gecko` ,`--collapsed` ,`--pstats` | Output format. `--pstats` is the default, and`--gecko` opens in the Firefox Profiler | 
| `-o` | Where to write the output | 

Two practical notes before you try it on a server:

- **Permissions.** Attaching reads another process's memory. On Linux that can require root or a relaxed`kernel.yama.ptrace_scope` . macOS needs elevated privileges, and Windows needs an administrator. Containers often restrict this too, so check your platform's policy.
- **Matching Python.** The profiler and the target should be the same Python minor version and build type. To attach to a 3.15 service, you need a 3.15 interpreter to run Tachyon.

### Reading the result

In a flame graph, each box is a function. Its width is the share of samples where that function was on the stack, so the widest boxes near the top are where time goes. Depth just shows who called whom.

For this endpoint, you should see:

- A wide stack under `build_report_slow` , in the generator expression that calls`belongs_to` and`amount_of` .
- A thinner part for the `not in` scan.
- A separate stack under `fetch_orders` , ending in`time.sleep` .

That last one introduces the **mode** option, which decides what gets counted:

| Mode | Counts | Use it to find | 
|---|---|---|
| `wall` (the default) | Real elapsed time, including time spent waiting | Where requests actually spend their time, I/O included | 
| `cpu` | Only time the thread is running Python code | CPU hotspots, without the waiting | 
| `gil` | Time holding the global interpreter lock | Threads fighting over the GIL | 
| `exception` | Samples where an exception is being handled | Hot error paths | 

In `wall` mode, the 50 ms wait in `fetch_orders` shows up as a block of its own. In `cpu` mode it disappears. Each answers a different question, so try both.

```
flowchart LR
    A["Measure the endpoint"] --> B["Sample it with Tachyon"]
    B --> C["Find the widest boxes"]
    C --> D["Fix one thing"]
    D --> E["Measure again"]
    E --> B
```
### Fix it, then check

Switch the route to `build_report_fast` and sample again. The wide stack under `build_report_slow` is gone. In `wall` mode, what's left is the 50 ms wait, which is now the floor for this endpoint. The slow report cost about 43 ms of CPU on top of it. And no amount of CPU tuning shrinks that wait. It's a database or a network problem now.

That's the value of sampling a real process. A `cpu` profile would have called the endpoint fixed. The `wall` profile shows what users still wait for.

Tachyon can also compare two runs. You record a baseline in its binary format, then ask for a differential flame graph against it. That makes "did my fix help?" a picture. See the `--binary` and `--diff-flamegraph` options in the docs.

## Smaller changes worth knowing

### UTF-8 is the default encoding

Before 3.15, `open("file.txt")` without an `encoding` used the system's locale encoding. That meant `cp1252` on many Windows machines, or ASCII in a container with the `C` locale. Now it's UTF-8 everywhere, whatever the environment.

For most services that removes a class of "works on my machine" bugs. But if anything reads files in a legacy encoding without saying so, it will change behavior. Find those calls first, on any Python since 3.10:

`python -X warn_default_encoding -m pytest`
You can opt out with `PYTHONUTF8=0` or `-X utf8=0`. For a single call, `encoding="locale"` asks for the old behavior. The best fix is to always pass `encoding=` explicitly.

### `frozendict`

A built-in immutable mapping. It keeps insertion order, and it's hashable when its keys and values are. It isn't a subclass of `dict`, so `isinstance(x, dict)` checks won't match it.

```
limits = frozendict({"free": 100, "pro": 1000})
cache_key = (user_id, limits)  # a hashable key, so it can go in a dict or an lru_cache
settings = json.loads(text, object_pairs_hook=frozendict, array_hook=tuple)  # immutable all the way down
```
The `array_hook` argument to `json.loads` is new in 3.15. Together with `object_pairs_hook`, it gives you a deeply immutable config tree.

### `sentinel`

You've probably written `_MISSING = object()`. This replaces it with something that has a readable repr, keeps its identity through `copy`, and works in type hints:

```
MISSING = sentinel("MISSING")
def get_setting(name: str, default: str | MISSING = MISSING) -> str:
    if default is MISSING:
        raise KeyError(name)
    ...
```
### The JIT

The experimental JIT has improved again. The python.org release notes report a 7% to 8% geometric-mean speedup on x86-64 Linux compared with the standard interpreter, and 11% to 12% on AArch64 macOS compared with the tail-calling interpreter. It's still experimental, and an average hides big differences between workloads. Benchmark your own service before you enable it, and don't make it your reason to upgrade.

### Frame pointers

CPython now builds with frame pointers by default (PEP 831). That helps `perf` and eBPF tools unwind native stacks through the interpreter. If you build Python or C extensions yourself, make sure your build flags keep them.

### Housekeeping

- `profile` **is deprecated.** It's scheduled for removal in 3.17.`cProfile` lives on as part of the new`profiling` package.
- `.pth` **import lines.** PEP 829 introduces`.start` files for package startup. When a matching`.start` file exists,`import` lines in`.pth` files are ignored. Check any tooling that relies on those lines.

## Should you upgrade?

| If you run... | What 3.15 offers | Be careful of | 
|---|---|---|
| **CLI tools, serverless functions, short-lived workers** | Lazy imports can cut startup noticeably | Errors and side effects moving to first use | 
| **A long-running FastAPI service** | Tachyon, to profile production safely. UTF-8 consistency | Little startup benefit once it's running. Dependency wheels | 
| **Windows servers or odd-locale containers** | UTF-8 by default | Implicit reads of legacy-encoded files | 
| **CPU-bound code** | An experimental JIT | Not production-ready. Benchmark before trusting it | 
| **A library others depend on** | `__lazy_modules__` , free-threaded stable ABI | Your support matrix | 

My suggestion for most services is to upgrade soon for the profiler and the encoding consistency, use lazy imports on a handful of heavy leaf libraries, and treat the JIT as something to watch.

### Before you flip the switch

1. **Check your dependencies.** C extensions need wheels built for 3.15. In a fresh 3.15 virtualenv, this fails fast if any dependency has no wheel:```
python3.15 -m venv .venv-315 && . .venv-315/bin/activate
pip install --only-binary :all: -r requirements.txt
```
2. **Run your tests with the warnings on.** Start without`-W error` to see the list, then tighten it:`python -X dev -X warn_default_encoding -W error::DeprecationWarning -m pytest`
3. **Read the notes.** Skim the Removed, Deprecated and Porting sections of the official What's New page for anything your code or tools touch.
4. **Add 3.15 to CI** as a non-blocking job first.
5. **Update your Docker image.** Official`python:3.15` images exist, including`-slim` and`-alpine` . A slim image has no compiler, so a missing wheel fails the build.
6. **Stage it,** then roll out, with the old image ready to roll back to.
7. **Adopt features one at a time.** Profile with Tachyon first, since it needs no code changes. Then lazy imports, one library at a time, measuring as you go.

## The tests

Here's the test file for the helper scripts. Several tests run real subprocesses, so they check the actual `-X importtime` format rather than a copy of it:

```
import contextlib
import io
import os
import subprocess
import sys
import tempfile
import textwrap
import time
import importtime_report as it
from report import build_report_fast, build_report_slow, fetch_orders, make_orders
from startup_bench import bench, summarize
SAMPLE = textwrap.dedent("""\
    import time: self [us] | cumulative | imported package
    import time:       100 |        100 |   _io
    import time:       200 |        900 | json
    import time:       300 |        300 |     json.scanner
    import time:       400 |        700 |   json.decoder
    import time:       500 |       2100 | app
    import time:       250 |        250 |   pydantic.fields
    import time:       750 |       1000 | pydantic
    some other stderr line
    """)
# ---- reading `-X importtime` -----------------------------------------------------------------------
def test_parse_reads_times_names_and_nesting():
    imports = it.parse(SAMPLE)
    assert [(i.name, i.self_us, i.cumulative_us, i.depth) for i in imports] == [
        ("_io", 100, 100, 1), ("json", 200, 900, 0), ("json.scanner", 300, 300, 2),
        ("json.decoder", 400, 700, 1), ("app", 500, 2100, 0), ("pydantic.fields", 250, 250, 1), ("pydantic", 750, 1000, 0),
    ]  # the header and the stray stderr line are ignored
def test_the_total_is_the_sum_of_what_your_code_imported_directly():
    assert it.total_us(it.parse(SAMPLE)) == 900 + 2100 + 1000
def test_packages_are_ranked_by_self_time_so_nothing_is_counted_twice():
    ranked = it.by_package(it.parse(SAMPLE))
    assert ranked == {"pydantic": 1000, "json": 900, "app": 500, "_io": 100}
    assert list(ranked)[0] == "pydantic"
def test_saved_lists_only_the_packages_that_got_cheaper():
    before = it.parse(SAMPLE)
    after = [i for i in before if not i.name.startswith("pydantic")]  # as if lazy import had skipped it
    assert it.saved(before, after) == {"pydantic": 1000}
    assert it.saved(after, before) == {}
def test_a_real_run_parses_and_the_numbers_add_up():
    imports = it.run_importtime("json")
    names = {i.name: i.depth for i in imports}
    assert names["json"] == 0 and names["json.decoder"] > 0
    total, summed = it.total_us(imports), sum(it.by_package(imports).values())
    assert abs(total - summed) <= 0.05 * total  # microsecond rounding only
def test_a_module_that_is_never_imported_costs_nothing_and_shows_up_in_saved():
    folder = tempfile.mkdtemp()
    for name, source in {
        "heavy_mod.py": "import time\ntime.sleep(0.05)\n",
        "app_eager.py": "import heavy_mod\n",
        "app_deferred.py": "def handler():\n    import heavy_mod\n",  # what `lazy import` does for a module you never touch
    }.items():
        with open(os.path.join(folder, name), "w") as f:
            f.write(source)
    eager = it.run_importtime("app_eager", cwd=folder)
    deferred = it.run_importtime("app_deferred", cwd=folder)
    assert "heavy_mod" in {i.name for i in eager} and "heavy_mod" not in {i.name for i in deferred}
    assert it.saved(eager, deferred)["heavy_mod"] >= 45_000  # 50 ms of sleeping, in microseconds
def test_extra_x_options_reach_the_interpreter():
    seen = []
    def fake_run(command, **kwargs):
        seen.append(command)
        return subprocess.CompletedProcess(command, 0, stdout="", stderr=SAMPLE)
    real, subprocess.run = subprocess.run, fake_run
    try:
        imports = it.run_importtime("app", python="py", xoptions=["lazy_imports=all", "dev"])
    finally:
        subprocess.run = real
    code = "__import__('app')"  # a call, so lazy_imports=all can't defer it
    assert seen[0] == ["py", "-X", "importtime", "-X", "lazy_imports=all", "-X", "dev", "-c", code]
    assert len(imports) == 7
def test_the_command_line_report():
    out = io.StringIO()
    with contextlib.redirect_stdout(out):
        it.main(["json", "--top", "3"])
    text = out.getvalue()
    assert text.startswith("json: ") and "Self time by package:" in text and "Slowest single imports" in text
# ---- the startup benchmark -----------------------------------------------------------------------------
def test_summarize():
    assert summarize([3.0, 1.0, 2.0]) == {"runs": 3, "min": 1.0, "median": 2.0, "max": 3.0}
    assert summarize([1.0, 2.0, 3.0, 10.0])["median"] == 2.5
def test_bench_skips_the_warmup_runs():
    stats = bench([sys.executable, "-c", "pass"], runs=3, warmup=2)
    assert stats["runs"] == 3 and 0 < stats["min"] <= stats["median"] <= stats["max"]
def test_bench_measures_a_real_delay():
    stats = bench([sys.executable, "-c", "import time; time.sleep(0.2)"], runs=2, warmup=0)
    assert 0.2 <= stats["min"] < 5
def test_bench_fails_loudly_when_the_command_fails():
    try:
        bench([sys.executable, "-c", "raise SystemExit(3)"], runs=1, warmup=0)
        raise AssertionError("should have raised")
    except subprocess.CalledProcessError as error:
        assert error.returncode == 3
# ---- the slow endpoint --------------------------------------------------------------------------------------
def test_orders_are_reproducible():
    assert make_orders(50) == make_orders(50) and make_orders(50, seed=1) != make_orders(50, seed=2)
def test_fast_and_slow_reports_agree():
    for n, seed in [(0, 1), (1, 1), (10, 3), (300, 4), (700, 5)]:
        orders = make_orders(n, seed)
        assert build_report_fast(orders) == build_report_slow(orders), (n, seed)
def test_the_slow_report_really_is_much_slower():
    orders = make_orders(1_500)
    start = time.perf_counter()
    build_report_slow(orders)
    slow = time.perf_counter() - start
    start = time.perf_counter()
    build_report_fast(orders)
    fast = time.perf_counter() - start
    assert slow > 20 * fast
def test_fetching_orders_waits_like_a_database_call():
    start = time.perf_counter()
    orders = fetch_orders(10, latency=0.05)
    assert time.perf_counter() - start >= 0.05 and len(orders) == 10
```
Run them with `pytest -q`. They don't need Python 3.15.

*Checked against the Python 3.15 documentation, the python.org release page, the PEPs and the Docker Hub* `python` *image page in November 2026. Version-specific details can change in patch releases, so check the docs for the exact 3.15.x you run.*

**Sources**

## Comments(0)

Join the discussion by logging into your account.

No comments yet. Be the first to comment!
