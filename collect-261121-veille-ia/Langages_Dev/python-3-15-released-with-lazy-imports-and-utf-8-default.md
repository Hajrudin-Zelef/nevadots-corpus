---
id: collect-261121-veille-ia/langages-dev/python-3-15-released-with-lazy-imports-and-utf-8-default
title: "Python 3.15 released with lazy imports and UTF-8 default"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-10-09"]
keywords: ["python", "changes", "default", "release", "utf-8", "lazy", "imports", "such"]
source: https://betanews.com/article/python-3-15-release/
source_anchor: ""
source_lines: [1, 31]
section: "Langages_Dev"
---
Python 3.15 reached stable status on Oct. 9, 2026, according to its official release page. Its headline changes put UTF-8 in charge of text encoding by default and introduce an opt-in lazy keyword for imports.

Version 3.15 follows Python 3.14 by twelve months and also upgrades the experimental just-in-time (JIT) compiler. Installers are available from Python.org, with every change catalogued in the What’s New in Python 3.15 guide.

Until now, Python loaded every imported module at the moment of import, whether or not the program went on to use it. Programs that pull in long chains of dependencies can lose seconds to that work at launch, the release documentation explains.

Under PEP 810, a line such as lazy import json binds the name to a lightweight proxy, and the module itself loads on first access. Existing code behaves as before, since the proposal states that “nothing changes unless a developer explicitly opts in.” Test suites, command-line utilities and dependency-heavy applications benefit most, its authors wrote.

Text handling changes under PEP 686, which treats UTF-8 as the standard encoding no matter how a computer is configured. File operations that omit an encoding argument, such as a bare open() call, once followed whatever locale the host operating system used.

Performance numbers for the JIT, which remains experimental, depend on the platform. Benchmarks published with the release put the gain at 7 to 8%, measured as a geometric mean, for x86-64 Linux compared with the standard interpreter.

AArch64 macOS machines saw an 11 to 12% speedup, measured against the faster tail-calling interpreter. Those two baselines differ, so the figures do not line up directly. Windows users on 64-bit hardware get that tail-calling build in the official installers as well.

A new built-in frozendict type cannot be modified once created, and it keeps keys in insertion order. Whenever every key and value inside is hashable, the whole object is hashable as well, the release notes add.

Alongside it sits sentinel, a built-in that produces distinct placeholder values. Such placeholders matter when None counts as a legitimate value, for example to distinguish a skipped argument from one deliberately set to None, as PEP 661 describes. Developers previously relied on homemade idioms such as object().

Profiling tools now sit together in a new profiling package defined by PEP 799. Inside it is Tachyon, a statistical profiler based on sampling that hooks into a live process “without requiring code modification or process restart,” in the words of the 3.15 docs. Tachyon reports wall-clock time, CPU time or how long each thread holds the Global Interpreter Lock (GIL), and it can produce an interactive flame graph.

Frame pointers are now on by default in CPython builds wherever a platform permits them, under PEP 831. Debuggers, profilers and eBPF-based observability tools can walk native call stacks faster and with fewer failures, per the documentation.

Free-threaded builds, which operate without the GIL, now have a stable ABI of their own, called abi3t, along with companion C API changes. Extension authors can compile against it, though the docs warn that switching usually calls for non-trivial source changes. The official macOS installers now set up free-threading by default.

Smaller changes round out the release, starting with * and ** unpacking inside comprehensions. Error messages now give clearer hints, and terminal output picks up extra color. Typing gains new features, and PEP 829 brings package startup configuration files to go with a long run of standard library updates.

For a hands-on tour, Barry Warsaw built whatsnewt, a text adventure that runs in the terminal and holds 18 puzzles. Each one centers on “a real 3.15 feature you have to actually use,” Warsaw wrote. Players can start it by running uvx --python 3.15 whatsnewt.

Marcus Pruitt covers Windows, desktop software and the open-source tools that help people get more from their computers. His work focuses on solving practical problems, from failed updates and confusing system settings to clean installations, bootable media and everyday performance issues. Marcus approaches technical advice from the reader’s point of view: explain what is happening, identify the safest fix and make every step easy to follow. He is especially interested in useful software that gives users greater control over their devices. When he is not troubleshooting Windows, he keeps an eye on the tools and changes shaping the wider PC ecosystem.

## Discussion
