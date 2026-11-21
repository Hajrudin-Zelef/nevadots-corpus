---
id: collect-261121-veille-ia/python-php/python-free-threaded-mode-no-gil-explained
title: "Python Free-Threaded Mode (No-GIL) Explained"
domain: python-php
role: reference
task: article
actors: []
dates: ["2025-06-16"]
keywords: ["python", "threads", "thread", "free-threaded", "build", "four", "time", "about"]
source: https://pythonresources.com/guides/python-free-threading-explained/
source_anchor: ""
source_lines: [1, 87]
section: "Python_PHP"
---
# Python Free-Threaded Mode (No-GIL) Explained

What free-threaded Python actually is, why it’s opt-in rather than the default, and when disabling the GIL is worth trying

**Free-threaded Python removes the Global Interpreter Lock (GIL), letting threads run Python code truly in parallel — but it’s an opt-in build, not the default, and it doesn’t speed up the code you’re running today.** Here’s what actually changed, and who it’s for right now.

## The problem it solves: the GIL

CPython’s Global Interpreter Lock has always meant that only one thread can execute Python bytecode at a time, even on a multi-core machine — threads take turns rather than running simultaneously. This is why `threading` in Python has traditionally helped with I/O-bound waiting (a thread can release the GIL while waiting on a network call) but never with CPU-bound parallelism the way it does in languages without a GIL. For genuine CPU parallelism, Python developers have reached for `multiprocessing` (separate processes, no shared memory) or offloaded the work to a C extension like NumPy instead.

Free-threaded builds remove that lock, so multiple threads can genuinely execute Python bytecode on separate cores at the same time.

## Officially supported, still opt-in

PEP 779, accepted on 16 June 2025, moved the free-threaded build from experimental (introduced in Python 3.13) to officially supported starting in Python 3.14, a real commitment from the core team, not a permanent side experiment. But “supported” doesn’t mean “default”: it is a separate build, distinguished by a `t` suffix (`python3.15t` alongside the regular `python3.15`). Python 3.15’s release notes add that the official macOS binaries now install free-threading support by default, so it is easier to get than before, but you still choose to run the `t` interpreter. The decision to make it the default is a separate, later step (PEP 779 calls it phase III) that has not been taken.

## The real trade-off: overhead, not a free lunch

Removing the GIL requires making the interpreter thread-safe in other ways, which adds overhead; reference counting in particular gets more expensive without a single global lock protecting it. PEP 779 set the limits for official support at a 15% single-threaded slowdown and 20% higher memory use (geometric means on the pyperformance suite), and reported the build at about 10% slower on Linux and Windows and about 3% on macOS when it was accepted, with roughly 15-20% more memory. The Python 3.15 documentation now puts the average slowdown at about 1% (macOS aarch64) to 8% (x86-64 Linux). Running free-threaded Python on single-threaded code doesn’t make it faster; it can make it marginally slower, for no benefit.

## What it looks like in practice

The point of the build is that threads doing CPU-bound work stop taking turns. This script times the same pure-Python loop on one thread and then on four threads at once, and prints how much longer four threads take. Run it with the standard `python3.15` and with `python3.15t` to compare:

```
import sys
import threading
import time
def work(n=3_000_000):
    total = 0
    for i in range(n):
        total += i * i % 7
    return total
def run(thread_count):
    threads = [threading.Thread(target=work) for _ in range(thread_count)]
    start = time.perf_counter()
    for thread in threads:
        thread.start()
    for thread in threads:
        thread.join()
    return time.perf_counter() - start
one = min(run(1) for _ in range(5))
four = min(run(4) for _ in range(5))
print(f"GIL enabled: {sys._is_gil_enabled()}")
print(f"four threads took {four / one:.1f}x as long as one")
```
On the machine used for this check (Apple M1, macOS, 8 cores, Python 3.15.2, best of five runs), four threads took about 4.0 times as long as one on the standard build, which means no parallel speedup at all, and about 1.1 times as long on the free-threaded build, which means the four threads ran in parallel. One thread took about the same time on both builds (roughly 0.16 s and 0.15 s), so the single-threaded cost was too small to see on this machine and workload. Your numbers will differ with the CPU, the operating system and the workload; the shape of the result is what to expect, not the figures.

## Where it actually helps

Free-threaded mode is for genuinely CPU-bound, multi-threaded Python — numerical code, data processing, or custom algorithms spread across `threading.Thread` workers where you’d previously have needed `multiprocessing` (with its separate-memory-space overhead) to get real parallelism. If your bottleneck is I/O (waiting on a database, an API, a file), you already get concurrency benefits from `threading` or `asyncio` under the standard GIL build — free-threading doesn’t add anything there.

## What 3.15 adds: a stable ABI for free-threaded builds

Python 3.15 adds PEP 803, a Stable ABI for free-threaded builds (nicknamed `abi3t`). It matters to the people who build C extensions rather than the people who use them: an extension targeting the Stable ABI can now be compiled once for free-threaded CPython, rather than needing a separate version-specific build for every Python release. The official notes are candid that moving over usually takes non-trivial source changes, such as switching to newer APIs and a new module export hook. Extensions that can’t switch continue to ship separate `abi3` and `cp315t` builds. Build tools with `abi3t` support include meson-python, scikit-build-core and Maturin.

The practical effect for you is indirect: it lowers the cost for library authors to publish free-threaded wheels, which is the thing the ecosystem section below says is holding adoption back.

## The ecosystem isn’t fully there yet

A large share of Python’s performance-critical libraries are C extensions, and not all of them are thread-safe under a free-threaded build yet — some still rely on the GIL implicitly for correctness, not just performance. The Python Free-Threading Guide tracks ecosystem compatibility if you’re considering testing it against real dependencies rather than a toy benchmark.

## Should you try it

Not by default. For nearly everyone, the standard GIL-enabled build remains the right choice — it’s what your dependencies are tested against, what tutorials assume, and what has zero single-threaded overhead cost. Test the free-threaded build specifically if you have CPU-bound multi-threaded code today and want to measure a real before and after on your own workload. Treat the PEP’s figures as a limit on overhead, not a promise of speedup, since the gain depends on how much of your workload can actually run in parallel.

## Frequently Asked Questions

### What is free-threaded Python?

A build of CPython, installed alongside the normal interpreter (e.g. python3.15t), that disables the Global Interpreter Lock (GIL). Without the GIL, multiple threads can run Python bytecode truly in parallel across CPU cores instead of taking turns.

### Is the GIL gone in Python 3.14 or 3.15?

No. The free-threaded build has been officially supported (not experimental) since Python 3.14 under PEP 779, but it’s still opt-in. Python 3.15’s release notes say the official macOS binaries now install free-threading support by default, and it remains a separate build with a `t` suffix rather than a replacement for the standard interpreter.

### Does free-threaded Python make single-threaded code faster?

No, and it can be slightly slower, because removing the GIL adds overhead to make the interpreter thread-safe without it. PEP 779 made a 15% limit on that slowdown (and 20% on extra memory) the bar for official support, and the Python 3.15 documentation puts the average overhead at about 1% on macOS aarch64 up to about 8% on x86-64 Linux. It is a cost, not a benefit, for single-threaded code.

### What does Python 3.15 add for free-threading?

A stable ABI for free-threaded builds (PEP 803, called abi3t). C extensions that target the Stable ABI can now build a single wheel compatible with free-threaded CPython, instead of shipping a separate version-specific build. It usually needs non-trivial source changes, and extensions that cannot switch keep building abi3 and cp315t wheels separately.

### Should you switch to free-threaded Python now?

Only if you have genuinely CPU-bound, multi-threaded Python code today — most people don’t. Most of Python’s C-extension ecosystem is still catching up to full thread-safety under the free-threaded build, and the standard interpreter remains the right default for nearly everyone.
