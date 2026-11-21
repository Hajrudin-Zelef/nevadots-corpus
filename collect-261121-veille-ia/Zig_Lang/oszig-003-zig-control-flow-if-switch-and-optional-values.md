---
id: collect-261121-veille-ia/zig-lang/oszig-003-zig-control-flow-if-switch-and-optional-values
title: "OSZig.003: Zig Control Flow — If, Switch, and Optional Values"
domain: zig-lang
role: reference
task: article
actors: []
dates: ["2026-11-08"]
keywords: ["else", "oszig", "switch", "optional", "watts", "boolean", "missing", "value"]
source: https://bitcoinversus.tech/2026/11/08/oszig-003-zig-control-flow-if-switch-optional-values/
source_anchor: ""
source_lines: [1, 235]
section: "Zig_Lang"
---
**OSZig.003** explains how the Zig programming language chooses between alternative code paths. A Boolean `if` handles comparisons and thresholds; `switch` matches known states or integer cases; and optional capture, using a type such as `?u32`, safely distinguishes a missing measurement from the number zero. The lesson follows OSZig.002 on const, var, and value types and uses the official **Zig 0.17.1** reference. A complete local practice program tests simulated device states, without accessing actual hardware or modifying system configuration.

## Learning Objectives and Prerequisites

- Construct Boolean conditions with `if` ,`else if` , and`else` .
- Explain why an `if` expression can produce a value without a separate ternary operator.
- Write an exhaustive `switch` expression over an enum and describe compiler-checked missing cases.
- Use an optional `?u32` and`|value|` capture to handle a missing reading.
- Trace inclusive threshold boundaries and distinguish strict `>` from`>=` .
- Build and run a standalone Zig source file, then execute its embedded `test` declarations.
- Identify type, integer range, invalid sensor data, and operational safety limitations.

Prerequisites: OSZig.001 on toolchains and source files and the previous lesson on Zig variables and types. For the executable lab, install Zig 0.17.1 from the official Zig download page for your operating system. Use a text editor and a nonprivileged account in a temporary study directory; do not connect the educational classifier to actual electrical controls.

## Elementary Overview

A program often needs to decide what to do when it sees a number. For example, a fictional device might be labeled NORMAL below 70 watts, WATCH from 70 through 89 watts, and ALERT at 90 watts or higher. A conditional asks a yes-or-no question and runs the matching instructions. More than one alternative can be tested in sequence, but the program should select the appropriate outcome consistently.

Zig also makes the difference between a missing number and zero explicit. If no reading arrives, the program should report MISSING rather than pretending the measurement was zero. A clear decision tree helps technicians and software developers see exactly how data moves from input through comparisons to an output label.

## The Version and Technical Background

Zig is a compiled, statically typed systems programming language designed to make control flow explicit and avoid hidden memory allocations. The current stable release checked for the lesson is **0.17.1**, announced in early November 2026 as a bug-fix update after 0.17.0. Its official language reference provides executable specification examples for `if`, `switch`, optional types, and error handling. The exact API behavior of the standard library can evolve between releases; examples here avoid advanced I/O and allocator APIs for that reason.

Zig deliberately separates types and assignments from flow-control constructs. A value declared `const` cannot be reassigned, but can still be tested by an `if`. A value declared `var` may change, yet the conditions used to choose a branch must still have appropriate types. Zig does not implicitly accept an integer wherever a Boolean is required.

## Boolean If, Else If, and Else

A Zig `if` uses a Boolean condition in parentheses followed by one expression or block. An `else if` is checked only after earlier conditions fail; an `else` handles all remaining cases. Condition order matters. Placing the threshold 70 before 90 would incorrectly select WATCH for a reading of 95 if the 70 case returned immediately.

A classifier returning a string slice can be written as one `if` expression, with the more severe threshold first. For a cross-language comparison, review C# branching and loops:

```
fn classify(watts: u32) []const u8 {
    return if (watts >= 90)
        "ALERT"
    else if (watts >= 70)
        "WATCH"
    else
        "NORMAL";
}
```
The return type `[]const u8` is a read-only byte slice. The string literals are backed by static immutable bytes, and Zig can coerce each branch to the declared slice type. With 69 watts the result is NORMAL; at 70 it is WATCH; at 89 it remains WATCH; at 90 it becomes ALERT. These toy watt limits are not manufacturer thresholds, electrical safety settings, or validated equipment telemetry.

## If Is an Expression

Unlike languages where conditional blocks are only statements, Zig allows `if` to produce a result. All reachable value-producing branches must be compatible with the expected type. As the official reference explains, a value can be assigned directly from a condition, without a separate `?:` ternary operator.

For example, this complete function returns the larger of two unsigned integers:

```
fn larger(a: u32, b: u32) u32 {
    return if (a >= b) a else b;
}
```
For `larger(12, 9)`, the result is 12. Equal values return either matching number, with no special case needed. A missing `else` is valid for a normal conditional action that does not need to yield a value, but a value-producing `if` expression needs a consistent result on all possible paths.

## Using Switch for Discrete States

When a variable represents one of several well-defined states, `switch` is often clearer than a long chain of comparisons. Each *prong* associates a pattern with an expression. Prongs do not fall through as in some C-language switch statements. If all enum possibilities are covered, Zig can prove the switch is exhaustive; omitting an enum case without an appropriate fallback causes a compile-time error.

Define a typed enum for three fictional operating states, then return its label:

```
const PowerState = enum {
    running,
    standby,
    offline,
};
fn powerStateName(state: PowerState) []const u8 {
    return switch (state) {
        .running => "RUNNING",
        .standby => "STANDBY",
        .offline => "OFFLINE",
    };
}
```
Enum literals such as `.running` gain their type from the surrounding `PowerState` context. The final comma is conventional and valid. If the enum later gains `maintenance`, the switch should be updated to handle the new state explicitly. For a small, closed enum, a catch-all `else` is often less useful because it can hide the need for deliberate handling of newly introduced values.

## Switching on Integer Cases and Ranges

A `switch` can also match integer values, groups, and inclusive ranges. The range syntax `3...10` matches every integer from 3 through 10, including both endpoints. An `else` prong is needed when other integer values remain possible.

Here, numeric status codes are examples only; no standard device protocol is implied:

```
fn statusCodeName(code: u8) []const u8 {
    return switch (code) {
        0 => "IDLE",
        1, 2 => "ACTIVE",
        3...10 => "BUSY",
        else => "UNKNOWN",
    };
}
```
The mapping gives 0 as IDLE; codes 1 and 2 as ACTIVE; codes 3 through 10 as BUSY; and all other `u8` values as UNKNOWN. A `u8` can hold integers from 0 through 255. If an external message contains values outside its range, validating them before conversion is essential; a successful switch does not make untrusted input safe.

## Optional Values Are Not Zero

A type written `?u32` means either a 32-bit unsigned integer or `null`. It is not the same as a Boolean and is not shorthand for a numeric value of zero. Some systems use missing values when a sensor times out or a network message does not contain the expected field; software must not silently substitute a valid reading.

The optional capture form `if (reading) |watts| ... else ...` binds the non-null payload to the name `watts`. The captured value is a plain `u32` inside the successful branch, while the fallback handles null:

```
fn classifyReading(reading: ?u32) []const u8 {
    return if (reading) |watts| classify(watts) else "MISSING";
}
```
For `classifyReading(null)`, the result is MISSING. For `classifyReading(0)`, the result is NORMAL, demonstrating that null and zero are distinct. The official optional-if examples also cover pointer captures, which can permit modification of mutable payloads when such changes are deliberately required.

## Boolean Logic and Evaluation

For plain Boolean conditions Zig uses `and`, `or`, and `!`. Logical AND requires both operands to be true, while OR requires at least one true. Short-circuit evaluation skips evaluating a second operand when the first already determines the result. These operations differ from integer bitwise operators.

A conditional checking whether a known reading lies between 70 inclusive and 90 exclusive could be written as:

```
fn isWatchRange(watts: u32) bool {
    return watts >= 70 and watts < 90;
}
```
The function returns true for 70 and 89, false for 69 and 90. Parenthesize expressions when doing so helps readers understand precedence. Safe real-world software also validates the provenance, units, and permitted range of each measurement before applying decision thresholds.

## Complete Local Practice Program

Create a file named `control_flow.zig` in a disposable study directory. The following complete program defines a reading classifier, an exhaustive enum switch, an optional-value handler, three diagnostic output lines, and eleven unit assertions. All sample numbers and power states are fictional. The code does not read from real sensors or interact with hardware.

```
const std = @import("std");
const PowerState = enum {
    running,
    standby,
    offline,
};
fn classify(watts: u32) []const u8 {
    return if (watts >= 90)
        "ALERT"
    else if (watts >= 70)
        "WATCH"
    else
        "NORMAL";
}
fn powerStateName(state: PowerState) []const u8 {
    return switch (state) {
        .running => "RUNNING",
        .standby => "STANDBY",
        .offline => "OFFLINE",
    };
}
fn classifyReading(reading: ?u32) []const u8 {
    return if (reading) |watts| classify(watts) else "MISSING";
}
pub fn main() void {
    std.debug.print("80 watts: {s}\n", .{classifyReading(80)});
    std.debug.print("State: {s}\n", .{powerStateName(.running)});
    std.debug.print("No reading: {s}\n", .{classifyReading(null)});
}
test "classification boundaries" {
    try std.testing.expectEqualStrings("NORMAL", classify(0));
    try std.testing.expectEqualStrings("NORMAL", classify(69));
    try std.testing.expectEqualStrings("WATCH", classify(70));
    try std.testing.expectEqualStrings("WATCH", classify(89));
    try std.testing.expectEqualStrings("ALERT", classify(90));
    try std.testing.expectEqualStrings("ALERT", classify(120));
}
test "every enum case" {
    try std.testing.expectEqualStrings("RUNNING", powerStateName(.running));
    try std.testing.expectEqualStrings("STANDBY", powerStateName(.standby));
    try std.testing.expectEqualStrings("OFFLINE", powerStateName(.offline));
}
test "missing and zero readings differ" {
    try std.testing.expectEqualStrings("MISSING", classifyReading(null));
    try std.testing.expectEqualStrings("NORMAL", classifyReading(0));
}
```
The `pub fn main() void` function is the executable entry point. It uses `std.debug.print`, which writes diagnostic text to **standard error**, not standard output. The `{s}` format specifier prints a byte slice as text. Test declarations are evaluated when running `zig test`; they are not automatically executed by `zig run`.

## How to Run and Test the Example

Verify that the official Zig 0.17.1 compiler is on your shell’s path, then use the built-in runner and test command. Only local source code is executed; no special administrator privileges are required:

```
zig version
zig run control_flow.zig
zig test control_flow.zig
```
If compilation and execution succeed, the expected diagnostic messages from `zig run` are:

```
80 watts: WATCH
State: RUNNING
No reading: MISSING
```
The test block labeled `classification boundaries` checks six input values; the enum test checks three cases; the optional test checks two cases. Eleven assertions are therefore expected across three named test blocks. The exact test runner progress text can vary by platform or compiler version. **No Zig compiler execution is claimed in the publication environment**; the compiler download attempt failed, so examples are source-reviewed expectations rather than an observed successful build.

## Reasoning Through Every Boundary

The thresholds can be checked without a compiler by applying the comparison rules. With 69, neither `watts >= 90` nor `watts >= 70` is true, so NORMAL results. At 70, the second comparison is true and WATCH results. At 89, the same branch remains selected. At 90, the first comparison becomes true and ALERT results. With missing data, the optional handler returns MISSING without attempting a numeric comparison.

That explicit distinction is important in operational code. A missing measurement should not be interpreted as a healthy zero-power reading. A production monitoring system would additionally need authenticated inputs, timestamp validation, range limits, sensor-fault handling, alarming requirements, and independently approved engineering thresholds.

## Troubleshooting and Safety

- **Compiler version mismatch:** compare`zig version` to the official 0.17.1 language reference. Older Zig examples can differ, particularly in their standard-library I/O usage.
- **Nonexhaustive switch:** ensure every enum value has an explicit prong, or add a deliberately justified fallback for an open set of values.
- **Unexpected threshold category:** check the order of comparisons and distinguish`>` from`>=` .
- **Optional capture fails:** a`|watts|` binding after`if (reading)` requires an optional payload; ordinary integers are not optional types.
- **Output appears on stderr:**`std.debug.print` emits diagnostic text, so shell redirects that capture only stdout can miss it.
- **Wrong type or overflow:** ensure input values can be represented as`u32` ; external negative or oversized numbers must be validated before conversion.
- **Code blocks overflow on mobile:** present long code with preserved line breaks and wrapping, not boxed fixed-width components.
- **Safety:** example thresholds are invented; never deploy them as mining hardware, server power, industrial or electrical protection limits.

## Practice and Assessment

- Predict the output of `classify(69)` ,`classify(70)` ,`classify(89)` , and`classify(90)` . Explain the two boundary transitions.
- Add a `maintenance` value to`PowerState` without changing`powerStateName` . What should the exhaustive switch check report?
- Add the missing enum prong and a new test for the maintenance label.
- Change `classifyReading(null)` to`classifyReading(0)` . Explain the different returned strings.
- Predict `statusCodeName(2)` ,`statusCodeName(8)` , and`statusCodeName(200)` .
- Explain why diagnostic output from `std.debug.print` may not appear in a pipeline reading stdout only.
- Run the tests in Zig 0.17.1, capture real exit statuses, and compare the observed outcomes with the expected results.

**Answer key:** The boundary outputs are NORMAL, WATCH, WATCH, and ALERT. A newly added enum value makes a formerly exhaustive switch incomplete until its handler is added. Null yields MISSING while zero is an actual value classified NORMAL. Status code 2 is ACTIVE, 8 is BUSY, and 200 is UNKNOWN. `std.debug.print` writes to stderr. Actual test execution must be documented separately; correct predictions are not equivalent to compiler verification.

## Authoritative References

## Elementary Conclusion

Zig makes code decisions visible. An `if` expression checks a Boolean condition, a `switch` matches a known set of values, and an optional type distinguishes a missing value from a real number. By studying exact boundaries and using tests, a learner can predict what a program should do before trusting it to manage any information. The practice example classifies made-up readings safely without changing real systems.

**Editor’s Note:** OSZig.003 follows the published OSZig.002 lesson. Examples follow the official Zig 0.17.1 language reference and use fictional monitoring values. The source program and eleven assertions were written and examined against the documentation; the Zig compiler could not be installed in the current authoring environment, so actual compilation, runtime output, and test execution remain unverified. The original 1200 × 630 branded cover is conceptual editorial art rather than a screenshot of tested Zig code. Video identities and educational relevance were checked through published video listings; successful public playback and viewport screenshots must be verified separately.

*We volunteer daily to ensure the information on this platform is Verifiably True. If you would like to support the research, please scan the QR code. Verify the destination and amount independently before donating.*

## Leave a Reply
