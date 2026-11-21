---
id: collect-261121-veille-ia/langages-dev/python-3-15-is-here-lazy-imports-frozendict-and-the-new-jit
title: "Python 3.15 Is Here: Lazy Imports, frozendict, and the New JIT"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["lazy", "imports", "python", "module", "data", "frozendict", "application", "import"]
source: https://www.c-sharpcorner.com/article/python-3-15-is-here-lazy-imports-frozendict-and-the-new-jit
source_anchor: ""
source_lines: [1, 273]
section: "Langages_Dev"
---
Python 3.15 introduces several changes that affect how developers start applications, manage configuration data, and evaluate runtime performance. The stable release arrived on October 9, 2026, bringing explicit lazy imports, a built-in `frozendict` type, and significant improvements to the experimental Just-in-Time (JIT) compiler.

These features address different development problems. Lazy imports can reduce the work performed during application startup. `frozendict` provides an immutable mapping for data that should not change after creation. The upgraded JIT offers another path for improving execution speed, although actual results depend on the workload and platform.

For developers maintaining APIs, command-line utilities, data-processing applications, or Python libraries, the important question is not simply what is new. It is which features solve real problems and how to adopt them without introducing compatibility issues.

## What's New in Python 3.15?

Python 3.15 includes improvements to the language, interpreter, and standard library.

| Feature | What changes | Main benefit | 
|---|---|---|
| Explicit lazy imports | Defers importing a module until its name is used | Can reduce startup work | 
| Built-in `frozendict` | Provides an immutable mapping type | Supports immutable configuration and mapping keys | 
| Improved experimental JIT | Enhances the optimizing execution path | Potentially improves runtime performance | 
| UTF-8 as the default encoding | Changes the default text encoding behavior | More consistent text processing across environments | 
| Improved profiling tools | Adds a dedicated profiling package and Tachyon profiler | Helps investigate application performance | 

These features are not interchangeable performance optimizations. Lazy imports target initialization, `frozendict` addresses data immutability, and the JIT targets execution. Each should be evaluated against the specific problem an application has.

## 1. Explicit Lazy Imports Can Improve Startup Time

Python normally executes an imported module's top-level code when the module is imported. This includes loading dependencies, initializing objects, and executing other statements.

In a large application, the resulting dependency tree can become expensive to initialize. The problem is particularly noticeable in command-line tools that import many libraries but use only a small subset for a particular command.

Python 3.15 introduces explicit lazy imports through the `lazy` soft keyword.

### How Lazy Imports Work

Consider a utility that supports several operations, including displaying a simple help message and processing JSON data.

With a regular import, Python loads the imported module immediately:

```
import json
def show_help():
    print("Usage: app [command]")
def parse_data(text):
    return json.loads(text)
```

If the program only displays help, it may not need the JSON module at all.

Python 3.15 allows the import to be declared lazily:

```
lazy import json
def show_help():
    print("Usage: app [command]")
def parse_data(text):
    return json.loads(text)
```

The actual module loading is deferred until the imported name is first accessed. In this example, calling `show_help()` does not require the application to load `json`, while calling `parse_data()` triggers the import when needed.

The syntax is available in Python 3.15 and later. Earlier Python versions cannot parse the `lazy import` statement.

### When Should Developers Use Lazy Imports?

Lazy imports are worth evaluating when:

- A command-line application starts frequently but uses only a small part of its dependency tree.
- An application supports multiple optional integrations.
- A development tool has expensive modules used only for specific commands.
- A service imports substantial functionality that is not needed during initialization.

However, lazy imports do not make the underlying module execute faster. They defer the cost. If the module is eventually used, its loading and initialization work still needs to happen.

This means the first operation that accesses a lazy import may take longer than it did with an eager import.

### Limitations to Understand

The `lazy` keyword is supported only in permitted module-level import statements. It cannot be used indiscriminately inside functions, class bodies, or exception-handling blocks.

It also cannot be used for star imports or `__future__` imports.

Developers should test lazy imports carefully when module initialization has side effects, registers plugins, configures logging, or establishes global state. Deferring those actions can change when the application observes them.

## 2. Python 3.15 Adds a Built-In `frozendict`

Python dictionaries are mutable. After creating a dictionary, any code holding a reference to it can modify its contents.

That behavior is useful for ordinary application state, but it can be problematic when a mapping represents configuration, fixed metadata, or a value that must remain stable.

Python 3.15 introduces `frozendict`, a built-in immutable mapping type.

### Creating an Immutable Mapping

The following example illustrates its basic usage:

```
settings = frozendict(
    timeout=30,
    retries=3,
    environment="production"
)
print(settings["timeout"])
print(settings)
```

Attempting to change the mapping raises a `TypeError`:

`settings["timeout"] = 60`

This makes the mapping useful when an application needs to prevent accidental changes to its top-level key-value associations.

### Using `frozendict` for Configuration

Consider a service that receives a set of operational settings. The application should allow components to read the settings without allowing them to replace or remove entries.

```
DEFAULT_SETTINGS = frozendict({
    "timeout": 30,
    "max_retries": 3,
    "debug": False
})
def get_timeout(settings):
    return settings["timeout"]
print(get_timeout(DEFAULT_SETTINGS))
```

The mapping cannot be modified after creation. This can make configuration ownership easier to reason about, particularly when settings are shared across multiple components.

There is an important limitation: immutability is shallow. If a value inside the mapping is itself a mutable list or dictionary, that nested object can still change.

For example:

```
settings = frozendict({
    "allowed_hosts": ["api.example.com"]
})
settings["allowed_hosts"].append("internal.example.com")
```

The example demonstrates that the mapping is immutable while its list value remains mutable. Applications requiring deeply immutable configuration must also control the mutability of nested values.

### Important Differences From `dict`

| Property | `dict` | `frozendict` | 
|---|---|---|
| Modify entries | Supported | Not supported | 
| Inherits from `dict` | Yes | No | 
| Preserves insertion order | Yes | Yes | 
| Hashable | No | Yes, when its keys and values are hashable | 
| Suitable for changing application state | Yes | No | 
| Suitable for immutable mappings | Requires additional controls | Yes | 

Because `frozendict` is not a subclass of `dict`, existing code that checks `isinstance(value, dict)` may not recognize it.

When a function should accept different mapping implementations, consider checking against the `collections.abc.Mapping` interface instead.

## 3. Python 3.15's JIT Compiler: What Improved?

A Just-in-Time compiler can optimize program execution by compiling selected code paths into machine code at runtime.

Python 3.15 significantly upgrades its experimental JIT compiler. The published release information reports a 7–8% geometric-mean performance improvement on x86-64 Linux compared with the standard interpreter. On AArch64 macOS, it reports an 11–12% speedup compared with the tail-calling interpreter.

These results describe specific benchmark suites and interpreter configurations. They are not guarantees that every Python application will become 7–12% faster.

### When Can JIT Improvements Matter?

The JIT is most relevant to workloads where Python execution itself contributes substantially to runtime.

Examples include:

- CPU-intensive data transformations.
- Repeated calculations in Python loops.
- Algorithms that execute frequently.
- Applications with substantial interpreter overhead.

By contrast, an application that spends most of its time waiting for a database, remote API, or disk operation may see little improvement from faster Python execution.

For example, consider a CPU-bound function:

```
def calculate_total(values):
    total = 0
    for value in values:
        total += value * value
    return total
```

This function performs repeated Python-level calculations and can serve as a starting point for a performance test.

However, its actual performance depends on input size, interpreter configuration, processor architecture, and the rest of the application. The code alone does not establish that the JIT will improve its execution time.

### How to Evaluate JIT Performance

A useful benchmark should compare the standard interpreter with the supported JIT configuration using the same workload and environment.

Measure at least:

Run multiple trials and report the measurement method. Avoid drawing conclusions from a single execution or comparing results from different machines.

The JIT remains an experimental optimization, so production adoption should account for the maturity of the feature and the deployment environment.

## Comparing the Three Main Features

The following table summarizes when each feature is most useful.

| Requirement | Feature to evaluate | Main consideration | 
|---|---|---|
| Faster command startup | Lazy imports | Deferred initialization may increase first-use latency | 
| Prevent changes to configuration mappings | `frozendict` | Nested mutable values still require attention | 
| Improve CPU-bound execution | Experimental JIT | Actual gains depend on workload and platform | 
| Reduce database response time | Database and query optimization | A faster interpreter may not address the bottleneck | 
| Support older Python versions | Compatibility planning | New syntax and built-in types may require a migration strategy | 

The key is to choose the feature that addresses the measured problem rather than adopting all three as general performance improvements.

## Best Practices for Adopting Python 3.15

### 1. Measure Before Optimizing

Establish a baseline for startup time, memory usage, and execution performance. Without a baseline, it is difficult to determine whether an optimization provides a meaningful benefit.

### 2. Introduce Lazy Imports Incrementally

Start with optional or expensive dependencies that are not required by every execution path. Test initialization side effects and first-use latency before applying lazy imports broadly.

### 3. Use `frozendict` for Data With Clear Ownership

Immutable mappings work well for fixed settings and metadata. Avoid using them as a replacement for ordinary dictionaries when the application needs to update entries regularly.

### 4. Check Compatibility With Dependencies

Verify that frameworks, libraries, build tools, and deployment environments support Python 3.15. Test the application's startup, background tasks, and production entry points.

### 5. Treat JIT Performance as a Measurement Question

Benchmark the actual workload on the target platform. Do not assume that benchmark results from another architecture will apply to your service.

### 6. Keep a Rollback Path

For production systems, deploy changes through a staging environment, monitor errors and latency, and retain a practical way to return to the previous runtime if compatibility problems appear.

## Common Mistakes and Troubleshooting

| Problem | Likely cause | Recommended action | 
|---|---|---|
| `lazy import` raises a syntax error | The application is running on an older Python version or the statement is used in an unsupported location | Verify the interpreter version and import placement | 
| An imported module initializes later than expected | Lazy loading defers module execution | Review initialization side effects and first-use behavior | 
| `frozendict` cannot be modified | This is expected behavior | Create a new mapping when updated values are required | 
| A `dict` type check fails | `frozendict` is not a subclass of`dict` | Accept the appropriate mapping interface | 
| JIT benchmarks show little improvement | The workload may be I/O-bound or otherwise limited by another component | Profile the application before changing runtime settings | 
| A dependency fails after upgrading | The dependency may not support Python 3.15 | Check compatibility and test a supported dependency version | 

## Advantages and Disadvantages

### Advantages

- Lazy imports can reduce unnecessary initialization work.
`frozendict` provides a standard option for immutable mappings.
- JIT improvements create additional opportunities to optimize CPU-intensive Python workloads.
- New profiling capabilities help developers investigate performance systematically.

### Disadvantages

- Lazy imports can move latency to the first use of a module.
`frozendict` does not automatically make nested objects immutable.
- The JIT's benefits vary by workload and platform.
- Upgrading Python may require dependency changes and additional compatibility testing.

## Summary

Python 3.15 introduces three changes worth evaluating independently: explicit lazy imports for deferred initialization, `frozendict` for immutable mappings, and an improved experimental JIT compiler for runtime optimization.

Lazy imports are particularly relevant to command-line applications and tools with optional dependencies. `frozendict` is useful when mappings should remain unchanged after creation. The JIT deserves attention when profiling identifies Python execution as a significant performance bottleneck.

For production applications, the most reliable approach is to upgrade in a controlled environment, test compatibility, measure real workloads, and adopt only the features that solve a demonstrated problem.

Join the conversation! Your thoughts help the community grow.
