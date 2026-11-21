---
id: collect-261121-veille-ia/langages-dev/python-3-15-is-out-lazy-imports-the-tachyon-profiler-and-a-jit-that-runs-1-2x-faster-hamidun-news
title: "Python 3.15 Is Out: Lazy Imports, the Tachyon Profiler, and a JIT That Runs 1.2x Faster — Hamidun News"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["python", "lazy", "profiler", "tachyon", "imports", "runs", "faster", "unpacking"]
source: https://news.hamidun.com/en/news/13611/python-3-15-is-out-lazy-imports-the-tachyon-profiler-and-a-j
source_anchor: ""
source_lines: [1, 45]
section: "Langages_Dev"
---
# Python 3.15 Is Out: Lazy Imports, the Tachyon Profiler, and a JIT That Runs 1.2x Faster

Python 3.15 is out. The headline features are explicit lazy imports (PEP 810), unpacking inside comprehensions (PEP 798), and Tachyon, a built-in sampling profiler in the profiling.sampling module. Overall speed is nearly the same as 3.14, but the experimental JIT delivered 1.2x on a Fibonacci benchmark (measured by Miguel Grinberg on 3.15.0rc3). With the 3.15 release, Python 3.10 is officially end-of-life. For library authors, that means 3.11 can now be the minimum supported version.

AI-processed from Simon Willison; edited by Hamidun News
Python 3.15 has been released. For most developers, three things matter: explicit lazy imports, the Tachyon profiler in the standard library, and new unpacking syntax in comprehensions. Interpreter speed is almost unchanged from 3.14, but the experimental JIT has shown a noticeable gain for the first time: 1.2x. Python 3.10 (from 2021) has also officially lost support. For library maintainers, that matters more than any new syntax: the compatibility floor moves up to 3.11. This year's "What's new" overview was written by Hugo van Kemenade, the release manager for 3.14 and 3.15.

## Lazy imports, unpacking, and Tachyon

The most practical addition is PEP 810. You can now declare a module import as `lazy import json` or `lazy from pathlib import Path`. The actual import happens on first access to the symbol. Simon Willison, the developer behind the LLM CLI tool and its plugin system, has long struggled with this problem. Plugins with heavy dependencies such as `sentence_transformers` added several seconds to every `llm` command. In the documentation for plugin authors, he had to recommend manual lazy imports. Now the language handles it.

The second change is PEP 798 (by Adam Hartz and Erik Demaine). It adds unpacking inside comprehensions: `[` flattens a list of lists, *L for L in lists]*`{s for s in sets}` merges sets, and `{**d for d in dicts}` merges dictionaries. It extends PEP 448, which landed back in Python 3.5 in 2015.

The third is profilers. The familiar tracing `cProfile` has been renamed `profiling.tracing`. Alongside it sits a sampling profiler, `profiling.sampling`, also known as Tachyon. It periodically captures the stacks of a running process and calculates where time is spent. Overhead is low, and you don't need to instrument the code in advance. Until now, this required external tools such as py-spy. The capability is now built into the standard library.

In the example from the overview, the profiler collected 2 651 samples in 2.65 seconds at a rate of about 1000 per second. The clearest takeaway was that the process spent almost all its time idle, waiting on the asyncio event loop. Results can be rendered as a flame graph and a heat map, as well as in a format for the Firefox Profiler interface. On macOS, attaching to a process by PID required `sudo`.

## Speed: the JIT is finally noticeable

Don't count on a "free" speedup. Miguel Grinberg compared 3.15.0rc3 with 3.14 and found that performance mostly stayed the same. The exception is the experimental JIT.

- JIT vs. the standard 3.15 interpreter: 1.2x on the Fibonacci benchmark.
- This is the first time Grinberg has seen a meaningful gain from the JIT.
- For comparison: last year, 3.14 clearly beat 3.13 across significantly more categories.
- Tachyon profiler: about 1000 samples per second, with no prior instrumentation.

## How to upgrade and what dropping 3.10 gives you

If you use `uv`, the upgrade takes a few commands. First, update the tool itself so it knows about the new release: `uv self update`. Then install the interpreter: `uv python install 3.15`. To make it the default for any `uv run` where the project hasn't pinned a different version, run `uv python pin --global 3.15`. To check: `uv run python --version`. Tools you've already installed can be reinstalled with the right version, for example `uv tool upgrade datasette --python 3.15`.

The flip side is more interesting for maintainers. Willison's policy is to support all living versions of Python. Since 3.10 is no longer supported, you can rely on features from 3.11:

- `tomllib` in the standard library;
- `asyncio.TaskGroup` and`asyncio.timeout()` ;
- `datetime.UTC` as an alias for`datetime.timezone.utc` ;
- in `sqlite3` ,`setlimit()` /`getlimit()` ,`create_window_function()` ,`blobopen()` for partial reads of large BLOBs, and`set_authorizer(None)` for removing an authorizer;
- a significantly expanded type system.

In Datasette, the first PR (#2998) already removes the workaround for `set_authorizer(None)`. The cookiecutter templates for click-app, datasette-plugin, python-lib, and llm-plugin are now tested on versions 3.11 through 3.15. The practical takeaway for your project: check your CI matrix, drop 3.10, and add 3.15. Try lazy imports first in CLI tools with heavy dependencies. The startup-time gain is visible right away there.

### The AI world, distilled — once a week

Seven stories that actually mattered, hand-picked. No noise, no reposts, no press releases.

Done! Check your inbox for a confirmation.
