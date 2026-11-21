---
id: collect-261121-veille-ia/langages-dev/python-3-15-0-ships-with-lazy-imports-and-frozendict
title: "Python 3.15.0 ships with lazy imports and frozendict"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["python", "release", "imports", "code", "lazy", "frozendict", "default", "linux"]
source: https://dev.to/techaiwire/python-3150-ships-with-lazy-imports-and-frozendict-18c6
source_anchor: ""
source_lines: [1, 82]
section: "Langages_Dev"
---
Python 3.15.0 was released on October 9, 2026, the language's yearly feature release. It brings explicit lazy imports, a built-in frozendict type and UTF-8 as the default text encoding. "Python 3.15.0 is the newest major release of the Python programming language," release manager Hugo van Kemenade wrote on Python Insider. The release took 5,643 commits from 1,012 contributors. It arrives a week later than first planned, after a surprise third release candidate fixed late lazy-import bugs.

Each big change comes from a PEP, a Python Enhancement Proposal, the design document for a feature. The release notes list these:

| PEP | Feature | What it does | 
|---|---|---|
| 810 | Explicit lazy imports | Lets code delay loading a module until it is first used, for faster startup | 
| 814 | `frozendict` | A built-in dictionary that cannot be changed after it is created | 
| 661 | Sentinel type | A built-in type for unique marker values | 
| 686 | UTF-8 by default | Python reads and writes text as UTF-8 unless told otherwise | 
| 798 | Unpacking in comprehensions | Allows unpacking inside comprehensions | 
| 829 | Package startup files | New startup configuration files for packages | 
| 799 | Profiling package | Adds the Tachyon sampling profiler | 
| 803, 820, 793 | Free-threaded stable ABI | One compiled extension can work across free-threaded builds | 
| 831 | Frame pointers | Turned on by default in builds | 

Typing gets three changes too: PEPs 728, 747 and 800. The experimental JIT compiler, error messages and command-line colors were also improved, the What's New page says.

The UTF-8 change matters most on Windows, Linux Compatible reports. Code that relied on the old default encoding can switch back with `PYTHONUTF8=0`.

The JIT, a compiler that turns hot Python code into machine code while it runs, is now faster. Python Insider gives these geometric-mean gains:

| Platform | JIT gain | 
|---|---|
| x86-64 Linux | 7-8% over the standard interpreter | 
| AArch64 macOS | 11-12% over the tail-calling interpreter | 

The results vary widely by workload. Individual benchmarks range from about 15% slower to more than 100% faster, Linux Compatible reports.

The official installers changed too. Windows 64-bit builds now use the tail-calling interpreter, a faster way to run Python's main loop. Linux Compatible puts that gain at 15-20%, and up to 40% for small, long-running pure-Python scripts. The official macOS installers now include free-threading support by default. Free threading lets Python run code on several CPU cores at once without the global interpreter lock.

There is one warning for Mac users. On macOS 27.0, IDLE and other tkinter apps "may hang when using an application's menu command that opens a dialog," Python Insider says. The app then has to be force quit.

The Python team blames a change in the operating system and believes it affects all versions of Tk, the toolkit tkinter uses. It is tracked as issue #158053. The release page suggests waiting on macOS 27.0 if you depend on Tk apps.

Three things are on their way out, according to the release notes and Linux Compatible:

- The pure-Python `profile` module is deprecated and will be removed in Python 3.17.`cProfile` stays.
- Code-executing lines in `.pth` files are deprecated in favor of the new startup files from PEP 829.
- The `PyGILState_*` C functions are soft-deprecated, but not removed.

Python 3.15 gets two years of bug fixes, then security fixes until about October 2031, Linux Compatible reports, following the schedule in PEP 602.

- 
**Try lazy imports on slow-starting tools.** Command-line tools and apps with heavy imports benefit most. Test any module that relies on code running at import time, since deferring the import changes when that code runs.
- 
**Check text handling on Windows.** UTF-8 is now the default. Run your file input and output tests on Windows, and use`PYTHONUTF8=0` only as a stopgap.
- 
**Use frozendict for fixed settings.** An unchangeable dictionary is safer for configuration and can be used where a hashable value is needed.
- 
**Benchmark before you count on the JIT.** Average gains are modest, and some workloads get slower. Measure your own code.
- 
**Hold off on macOS 27.0 if you use tkinter.** IDLE and Tk apps can hang until the bug is fixed.
- 
**Move off `profile` now.** It disappears in Python 3.17, so switch to`cProfile` or the new profiling package.

Before you upgrade production systems, check that your key dependencies support 3.15.

*This article was first published on Tech AI Wire.*

Deutsch · 日本語 · Français · Español · Português

- Python 3.15.0 final is here! - Python Insider
- Python Release Python 3.15.0 - Python.org
- Python 3.15.0 (final) is here! - Discussions on Python.org
- What's new in Python 3.15 - Python documentation
- Python 3.15.0 Is Out: Lazy Imports, frozendict, and Faster Speeds - Linux Compatible

## Top comments (1)

Lazy imports là thay đổi lớn nhất — cuối cùng cũng có PEP 690 sau bao năm tranh luận. Thử chạy

`python -X importtime`trên codebase lớn, thấy startup time giảm 30-40% là bình thường, đặc biệt khi import heavy libs kiểu pandas/torch chỉ khi thực sự cần.
frozendict built-in thì tiện, nhưng thực tế vẫn hay dùng

`types.MappingProxyType`cho backward compat. Cái hay là giờ có`__hash__`sẵn, dùng làm key trong dict/set không phải wrap thêm.
UTF-8 default thì... muộn màng nhưng chắc chắn. Đã hết thời

`locale.getpreferredencoding()`trả về cp1252 trên Windows.
JIT faster — benchmark micro thấy nhanh 10-15%, nhưng real-world workload (web server, data pipeline) chưa thấy breakthrough. Cần theo dõi 3.15.1/3.15.2 để các regression fix kịp.

Ai đang maintain lib public, nhớ test matrix thêm 3.15 — lazy imports có thể break code phụ thuộc side-effect tại import time (ví dụ: register plugin, patch monkey) (site: labagent .tech)
