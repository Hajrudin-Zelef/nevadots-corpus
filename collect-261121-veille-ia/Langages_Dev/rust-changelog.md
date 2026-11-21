---
id: collect-261121-veille-ia/langages-dev/rust-changelog
title: "Rust changelog"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["language", "stabilize", "lint", "rustc", "rust", "miscompilation", "changelog", "allow"]
source: https://traceary.com/rust
source_anchor: ""
source_lines: [1, 140]
section: "Langages_Dev"
---
library: fix missing symbol wcslen on UEFI and embedded targets rustc: fix miscompilation of FFI bool return values on x86_64 rust-lld: add the rpath for libLLVM on macOS

Catalog / Languages

# Rust changelog

Rust is at 1.99.1, published 16 Oct 2026, 13 days ago.

Systems language with a six week release train and published release notes per version.

- Collected
- 62 releases back to 20 Jan 2022

Read on 29 of the 29 days on record, last today. Collection status

## Version history

## 1.x62 releases

Language Add allow-by-default raw_borrows_via_references lint that checks for references that decay immediately into raw borrows Extend unconditional_panic lint to function calls that panic when the chunks/windows size is zero Stabilize C-v

rustc: fix miscompilation in generating vtables

Language Allow shortening lifetime of &mut when unsize-coercing, even in an invariant position. For example, you can now coerce a Cell<&'long mut i32> to a Cell<&'short mut dyn Send>. Such shortenings were already previously allowed when co

rustc: Fix miscompilation in LLVM optimization This backports an LLVM submodule bump to include the LLVM-side fix and a revert of the rustc change that is one known trigger for the bug. The rustc side revert should not be strictly necessary

Language Consider Result<T, Uninhabited> and ControlFlow<Uninhabited, T> to be equivalent to T for must use lint Add allow-by-default dead_code_pub_in_binary lint for unused pub items in binary crates Stabilize the div32, lam-bh, lamcas, ld

Cargo: fix timeout/retry behavior Cargo: apply patches for CVE-2025-15661, CVE-2026-55199, and CVE-2026-55200 to libssh2 rustc: fix miscompilation in MIR optimization

Language Allow passing expr metavariable to cfg Always coerce never types in tuple expressions Avoid incorrect inference guidance of function arguments in rare cases Support s390x vector registers in inline assembly Allow using constants of

Language Stabilize if let guards on match arms irrefutable_let_patterns lint no longer lints on let chains Support importing path-segment keywords with renaming Stabilize inline assembly for PowerPC and PowerPC64 const-eval: be more consist

Fix std::thread::spawn on wasm32-wasip1-threads Remove new methods added to std::os::windows::fs::OpenOptionsExt The new methods were unstable, but the trait itself is not sealed and so cannot be extended with non-default methods. Clippy: f

Language Impls and impl items inherit dead_code lint level of the corresponding traits and trait items Stabilize additional 29 RISC-V target features including large portions of the RVA22U64 / RVA23U64 profiles Add warn-by-default unused_vi

Don't try to recover keyword as non-keyword identifier, fixing an ICE that especially affected rustfmt. Fix clippy::panicking_unwrap false-positive on field access with implicit deref. Revert "Update wasm-related dependencies in CI", fixing

Language Stabilize several s390x vector-related target features and the is_s390x_feature_detected! macro Stabilize declaration of C-style variadic functions for the system ABI Emit error when using some keyword as a cfg predicate Stabilize

Language Document MaybeUninit representation and validity Allow &raw [mut | const] for union field in safe code Prefer item bounds of associated types over where-bounds for auto-traits and Sized Do not materialize X in [X; 0] when X is unsi

Enable file locking support in illumos. This fixes Cargo not locking the build directory on illumos. Fix wasm_import_module attribute cross-crate. This fixes linker errors on WASM targets.

Language Lower pattern bindings in the order they're written and base drop order on primary bindings' order Stabilize declaration of C-style variadic functions for sysv64, win64, efiapi, and aapcs ABIs. This brings these ABIs in line with t

Language Split up the unknown_or_malformed_diagnostic_attributes lint. This lint has been split up into four finer-grained lints, with unknown_or_malformed_diagnostic_attributes now being the lint group that contains these lints: unknown_di

Language Stabilize explicitly inferred const arguments (feature(generic_arg_infer)) Add a warn-by-default mismatched_lifetime_syntaxes lint. This lint detects when the same lifetime is referred to by different syntax categories between func

Language Stabilize #![feature(let_chains)] in the 2024 edition. This feature allows &&-chaining let statements inside if and while, allowing intermixture with boolean expressions. The patterns inside the let sub-expressions can be irrefutab

Language Stabilize asm_goto feature Allow parsing open beginning ranges (..EXPR) after unary operators !, -, and *. Don't require method impls for methods with Self: Sized bounds in impls for unsized types Stabilize feature(precise_capturin

Language Stabilize upcasting trait objects to supertraits. Allow safe functions to be marked with the #[target_feature] attribute. The missing_abi lint now warns-by-default. Rust now lints about double negations, to catch cases that might h

Fix the doctest-merging feature of the 2024 Edition. Relax some target_feature checks when generating docs. Fix errors in std::fs::rename on Windows 10, version 1607. Downgrade bootstrap cc to fix custom targets. Skip submodule updates when

Language The 2024 Edition is now stable. See the edition guide for more details. Stabilize async closures See RFC 3668 for more details. Stabilize #[diagnostic::do_not_recommend] Add unpredictable_function_pointer_comparisons lint to warn a

Fix ICE 132920 in duplicate-crate diagnostics. Fix errors for overlapping impls in incremental rebuilds. Fix slow compilation related to the next-generation trait solver. Fix debuginfo when LLVM's location discriminator value limit is excee

Language Allow #[deny] inside #[forbid] as a no-op Show a warning when -Ctarget-feature is used to toggle features that can lead to unsoundness due to ABI mismatches Use the next-generation trait solver in coherence Allow coercions to drop

Language Stabilize &mut, *mut, &Cell, and *const Cell in const. Allow creating references to statics in const initializers. Implement raw lifetimes and labels ('r#ident). Define behavior when atomic and non-atomic reads race. Non-exhaustive

Language Don't make statement nonterminals match pattern nonterminals Patterns matching empty types can now be omitted in common cases Enforce supertrait outlives obligations when using trait impls addr_of(_mut)! macros and the newly stabil

Language Abort on uncaught panics in extern "C" functions. Fix ambiguous cases of multiple & in elided self lifetimes. Stabilize #[expect] for lints (RFC 2383), like #[allow] with a warning if the lint is not fulfilled. Change method resolu

Fix miscompilation in the jump threading MIR optimization when comparing floats Revert changes to the dead_code lint from 1.80.0

Language Document maximum allocation size Allow zero-byte offsets and ZST read/writes on arbitrary pointers Support C23's variadics without a named parameter Stabilize exclusive_range_pattern feature Guarantee layout and ABI of Result in so

Language Stabilize inline const {} expressions. Prevent opaque types being instantiated twice with different regions within the same function. Stabilize WebAssembly target features that are in phase 4 and 5. Add the redundant_lifetimes lint

Language Stabilize #[cfg(target_abi = ...)] Stabilize the #[diagnostic] namespace and #[diagnostic::on_unimplemented] attribute Make async-fn-in-trait implementable with concrete signatures Make matching on NaN a hard error, and remove the

CVE-2024-24576: fix escaping of Windows batch file arguments in std::process::Command

Revert stripping debuginfo by default for Windows This fixes a regression in 1.77 by reverting to the previous default. Platforms other than Windows are not affected. Internal: Fix heading anchor rendering in doc pages

Language Reveal opaque types within the defining body for exhaustiveness checking. Stabilize C-string literals. Stabilize THIR unsafeck. Add lint static_mut_refs to warn on references to mutable statics. Support async recursive calls (as lo

Language Document Rust ABI compatibility between various types Also: guarantee that char and u32 are ABI-compatible Add lint ambiguous_wide_pointer_comparisons that supersedes clippy::vtable_address_comparisons Compiler Lint pinned #[must_u

Language Stabilize async fn and return-position impl Trait in traits. Allow function pointer signatures containing &mut T in const contexts. Match usize/isize exhaustively with half-open ranges. Guarantee that char has the same size and ali

Resolved spurious STATUS_ACCESS_VIOLATIONs in LLVM Clarify guarantees for std::mem::discriminant Fix some subtyping-related regressions

Language Codify that std::mem::Discriminant<T> does not depend on any lifetimes in T Replace private_in_public lint with private_interfaces and private_bounds per RFC 2145. Read more in RFC 2145. Allow explicit #[repr(Rust)] closure field c

Language Uplift clippy::fn_null_check lint as useless_ptr_null_checks. Make noop_method_call warn by default. Support interpolated block for try and async in macros. Make unconditional_recursion lint detect recursive drops. Future compatibi

Adjust codegen change to improve LLVM codegen rustdoc: Fix self ty params in objects with lifetimes Fix regression in compile times Resolve some ICE regressions in the compiler: #115215 #115559

Fix CVE-2023-38497: Cargo did not respect the umask when extracting dependencies Fix bash completion for users of Rustup Do not show suspicious_double_ref_op lint when calling borrow() Fix ICE: substitute types before checking inlining comp

Language Replace const eval limit by a lint and add an exponential backoff warning expand: Change how #![cfg(FALSE)] behaves on crate root Stabilize inline asm for LoongArch64 Uplift clippy::undropped_manually_drops lint Uplift clippy::inva

Language Stabilize raw-dylib, link_ordinal, import_name_type and -Cdlltool. Uplift clippy::{drop,forget}_{ref,copy} lints. Type inference is more conservative around constrained vars. Use fulfillment to check Drop impl compatibility Compile

Language Relax ordering rules for asm! operands Properly allow macro expanded format_args invocations to uses captures Lint ambiguous glob re-exports Perform const and unsafe checking for expressions in let _ = expr position. Compiler Exten

Language Deriving built-in traits on packed structs works with Copy fields. Stabilize the cmpxchg16b target feature on x86 and x86_64. Improve analysis of trait bounds for associated types. Allow associated types to be used as union fields.

Update the GitHub RSA host key bundled within Cargo. The key was rotated by GitHub on 2023-03-24 after the old one leaked. Mark the old GitHub RSA host key as revoked. This will prevent Cargo from accepting the leaked key even when trusted

Fix miscompilation in produced Windows MSVC artifacts This was introduced by enabling ThinLTO for the distributed rustc which led to miscompilations in the resulting binary. Currently this is believed to be limited to the -Zdylib-lto flag u

Language Stabilize default_alloc_error_handler This allows usage of alloc on stable without requiring the definition of a handler for allocation failure. Defining custom handlers is still unstable. Stabilize efiapi calling convention. Remov

Fix interoperability with thin archives. Fix an internal error in the compiler build process. Downgrade clippy::uninlined_format_args to pedantic.

Language Make Sized predicates coinductive, allowing cycles. #[must_use] annotations on async fn also affect the Future::Output. Elaborate supertrait obligations when deducing closure signatures. Invalid literals are no longer an error unde

Added validation of SSH host keys for git URLs in Cargo (CVE-2022-46176)

Language Permit specifying explicit discriminants on all repr(Int) enums#[repr(u8)] enum Foo { A(u8) = 0, B(i8) = 1, C(bool) = 42, } Allow transmutes between the same type differing only in lifetimes Change constant evaluation errors from a

Language Error on as casts of enums with #[non_exhaustive] variants Stabilize let else Stabilize generic associated types (GATs) Add lints let_underscore_drop and let_underscore_lock from Clippy Stabilize breaking from arbitrary labeled blo

Language Unions with mutable references or tuples of allowed types are now allowed It is now considered valid to deallocate memory pointed to by a shared reference &T if every byte in T is inside an UnsafeCell Unused tuple struct fields are

Language Remove migrate borrowck mode for pre-NLL errors. Modify MIR building to drop repeat expressions with length zero. Remove label/lifetime shadowing warnings. Allow explicit generic arguments in the presence of impl Trait args. Make c

Rust 1.62.1 addresses a few recent regressions in the compiler and standard library, and also mitigates a CPU vulnerability on Intel SGX. The compiler fixed unsound function coercions involving impl Trait return types. The compiler fixed an

Language Stabilize #[derive(Default)] on enums with a #[default] variant Teach flow sensitive checks that visibly uninhabited call expressions never return Fix constants not getting dropped if part of a diverging expression Support unit str

Language const fn signatures can now include generic trait bounds const fn signatures can now use impl Trait in argument and return position Function pointers can now be created, cast, and passed around in a const fn Recursive calls can now

Language Stabilize #[cfg(panic = "...")] for either "unwind" or "abort". Stabilize #[cfg(target_has_atomic = "...")] for each integer size and "ptr". Compiler Enable combining +crt-static and relocation-model=pic on x86_64-unknown-linux-gnu

Language Stabilize default arguments for const parameters and remove the ordering restriction for type and const parameters Stabilize destructuring assignment Relax private in public lint on generic bounds and where clauses of trait impls S

Fix race condition in std::fs::remove_dir_all (CVE-2022-21658) Handle captured arguments in the useless_format Clippy lint Move non_send_fields_in_send_ty Clippy lint to nursery Fix wrong error message displayed when some imports are missin
