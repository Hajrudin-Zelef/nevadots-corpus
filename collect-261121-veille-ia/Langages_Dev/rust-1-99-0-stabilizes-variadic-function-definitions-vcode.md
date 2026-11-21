---
id: collect-261121-veille-ia/langages-dev/rust-1-99-0-stabilizes-variadic-function-definitions-vcode
title: "Rust 1.99.0 stabilizes variadic function definitions — VCode"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["rust", "functions", "variadic", "stabilizes", "release", "function", "list", "announcement"]
source: https://vcode.zeleznalady.cz/en/blog/rust-1-99-0-stabilizuje-definice-variadickych-funkci-db2c9073/
source_anchor: ""
source_lines: [1, 19]
section: "Langages_Dev"
---
## Variadic functions in Rust

Announced by the Rust team on October 1, 2026, Rust 1.99.0 stabilizes defining variadic functions with the “C” and “C-unwind” ABIs. Such a function accepts a variable number of arguments, represented by an ellipsis, and its body can work with them through a variadic argument list. Rust could already call variadic functions defined outside the language, such as libc::printf; the new capability is to define these functions in Rust itself. This is an extension on the definition side, not a claim that the behavior of all existing external functions has changed. The release note describes the newly stabilized capability rather than a broader redesign of Rust’s foreign-function interface.

## Working with the argument list

The type representing a variable argument list is called VaList. The announcement says it is ABI-compatible with C’s va_list type across targets; this concerns the interface between programs and their components. It does not mean that any type can be read from the list without restriction: the VaArgSafe trait governs which types may be read. The release example reads two i32 values and adds them, and its comment explicitly requires the caller to provide at least two i32 arguments. That example makes the calling assumption visible, but it does not describe a general runtime check of the number or types of arguments. The release note points readers to the Rust Reference for further details.

## Safety assumptions and limits

The example function is marked unsafe extern, and the operations that read arguments appear in unsafe blocks. This matches its stated assumption that the call supplies at least two i32 values; the example does not provide a general check of argument count or types at runtime. The announcement refers readers to the Rust Reference for more information about C-variadic functions. The available description therefore supports a limited conclusion: the release stabilizes a way to define these functions, but it does not establish every condition for their use, the effect on particular projects, or whether their security properties change. Those questions are outside the details given in the release announcement.

## Another change and how to update

The release also stabilizes support for defining naked variadic functions with ABIs other than “C”. According to the announcement, these functions must be written using inline assembly; it gives no further account of their use. The Rust team says that users with an installation managed by rustup can move to the stable version with rustup update stable. It also suggests the beta or nightly channel for people who want to test future releases and asks users to report bugs they encounter. The announcement thus describes both language changes and an update route, but does not report how the features have performed in deployment or how widely they will be adopted.

## Comments

No comments yet.
