---
id: collect-261121-veille-ia/langages-dev/rust-release-notes
title: "Rust Release Notes"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["lint", "macro", "cargo", "rust", "unused", "default", "errors", "debug"]
source: https://dev-doc.rust-lang.org/stable/releases.html
source_anchor: ""
source_lines: [1, 1338]
section: "Langages_Dev"
---
`i686-pc-windows-gnu` is now a tier 2 without host tools target`wasm32-wasip3` to Tier 2`powerpc64-sony-ps3`
Refer to Rustâs platform support page for more information on Rustâs tiered platform support.

`ENOTSUP` to `ErrorKind::Unsupported``std::sys::pal::sgx::waitqueue::unsafe_list``[lints.cargo]` table. This includes a new built-in unused dependency check via the `cargo::unused_dependencies` lint. docs #17298`Cargo.lock`. (RFC 3923) docs #17335`.crate` tarball by default. `--locked` in cargo install becomes a no-op. #17388`if let`. This may cause new errors`Infallible` is now a type alias to `!``box` patterns are no longer syntactically allowed`str::escape_debug``macro_rules` `expr` metavariable grouping`Iterator::{min,max}(_by_key)` changed which `Ord` methods they use. This cannot change behaviour (other than being potentially faster) for types correctly following the `VFPv3-D32` FPU as a minimum, as they erroneously had the `d32` feature in their baseline list of features. Breakage introduced as a result of correcting this can be fixed by passing `-Ctarget-feature=+d32` to Rust.`ambiguous_glob_imported_traits` lint fire in slightly more cases`+fix-cortex-a53-835769` a default feature for `aarch64-unknown-fuchsia``Box` have been generalised to arbitrary allocators, which may cause new type inference errors`-Ctarget-feature`/`-Ctarget-cpu` values that would alter the ABI are rejected as hard errors now.`wcslen` on UEFI and embedded targets`bool` return values on x86_64`libLLVM` on macOS`raw_borrows_via_references` lint that checks for references that decay immediately into raw borrows`unconditional_panic` lint to function calls that panic when the chunks/windows size is zero`#[unsafe(naked)]` functions to define C-variadic functions (`#![feature(c_variadic_naked_functions)]`).`Sized``#[my_macro] mod foo;`. This allows outlined modules (`mod foo;`) anywhere in the body of a custom attribute or derive macro.`overflowing_literals` lint with repeated negation. For instance, it will now no longer lint on `--128_i8`, which is already detected by the `arithmetic_overflow` lint.`invalid_runtime_symbol_definitions` and `suspicious_runtime_symbol_definitions` lints`#[path]` attributes on inline modules`unreachable_cfg_select_predicates` lint as part of `unused` lint group`asm!` on x86`UnsafeCell` can be accessed without going through `get`
`global_asm!``doc` attribute is used on a macro invocation`static_position_independent_executables` on all gnu targets`RangeInclusive` (`a..=b` ranges) is now optimized better in some circumstances. As a side effect of this, the behavior of `start()` and `end()` on such ranges may return different values, and using such ranges as slice indexes may have different behavior. These behaviors were not guaranteed to be stable, so these changes are considered to not be breaking changes.`transmute_copy` to accept `?Sized` types`escape_debug_ext``core::fmt::NumBuffer` in `alloc` (and `std`)`debug`. This is a preparation for transitioning the `dev` profile away from debugging to give a saner default for faster development iterations. Currently there is no difference between `default-features` field. For example, `serde = { workspace = true, default-features = false }` now turns off default features even when the workspace definition enables them. On earlier editions, `default-features = false` is ignored with a warning. (RFC 3945) #17126
See also the full Cargo changelog

`unused_footnote_definition` rustdoc lint`std::i32::MAX` should be accessed via `i32::MAX` instead.`no_mangle_generic_items` into hard error`Pin::new_unchecked` has had its safety invariants changed slightly`let` patterns typecheck`unsupported()` instead of `from_raw_os_error(22)``#[repr(simd)]` was accidentally allowed on macro invocations on stable Rust`Box::leak`: tell people to avoid unleaking`doc(cfg())` into account when filtering doctests`'static``semicolon_in_expressions_from_non_local_macros`) even when the macro comes from another crate. Previously, such warnings only appeared for macros from the same crate, to avoid showing warnings that canât be fixed locally; however, this masked problems, as integration tests from the crate providing the macro get compiled as a separate crate, so tests often wouldnât reveal this issue. If you encounter a lint like this, please make sure to report it to the crate providing the macro so they can fix it; donât just silence it in your own crate.
`CI` environment variable. #17220
These changes do not affect any public interfaces of Rust, but they represent significant improvements to the performance or internals of rustc and related tools.

`&mut` when unsize-coercing, even in an invariant position. For example, you can now coerce a `Cell<&'long mut i32>` to a `Cell<&'short mut dyn Send>`. Such shortenings were already previously allowed when coercing a `&`, or coercing a `core` runtime symbols like `memcmp`, `memset`, `strlen`, â¦ and is planned to be expanded in the next few releases.`c_void_returns` lint to check `core::ffi::c_void` as a return type`Location<'_>` lifetime to `Panic[Hook]Info``RangeInclusive::from(legacy::RangeInclusive)``ManuallyDrop`âs `derive` macro is available at `{core,std}::derive`. This was previously unintentionally stabilized in 1.96, but is now explicitly accepted as a stabilized API.
`ambiguous_glob_imports` lint into a hard error`Type = Type` and `Type == Type` are no longer syntactically allowed`repr(transparent)` is now more strict about which fields have âtrivialâ layout and hence can be ignored: `repr(C)` types, types with private fields, and `#[non_exhaustive]` types are no longer considered âtrivialâ`transmute()` when some `repr` attributes are involved.`derive(PartialOrd)` when deriving `assert_eq` and `assert_ne``PartialEq` implementation, when a `derive(PartialEq)` implementation for that type also exists.`-Zemscripten-wasm-eh=false` flag to switch back to JS exceptions has been removed.`File::lock` implementation, it has the wrong semantics (return âunsupportedâ instead)`cfg_select!`
This may cause more code to be formatted which was previously ignored.
These previously stable APIs are now stable in const contexts:

`build.warnings` config. This controls how lint warnings from local packages are treated. Useful for enforcing a warning-free build in CI, replacing `-Dwarnings`. docs`resolver.lockfile-path` config. This allows specifying the path to the lockfile to use when resolving dependencies. Useful when working with read-only source directories. docs`--target-dir` doesnât look like a Cargo target directory. This prevents accidental deletion of non-target directories.`-m` shorthand for `--manifest-path``curl` dependency from `crates-io` crate`f32: From<{float}>` to constrain `{float}``pin!`, in order to prevent unsoundness. The most likely case where this might impact users is: writing `pin!(x)` where `x` has type `&mut T` will now always correctly produce a value of type `Pin<&mut &mut T>`, instead of sometimes allowing a coercion that produces a value of type `Pin<&mut T>`. This coercion was previously incorrectly allowed since Rust 1.88.0.`std::char` constants and functions`f64` methods which have been deprecated since 1.0`varargs_without_pattern` lint in deps`link_section` specifier`enum`s have changed.  This is not a breaking change, as it only applies to `#[export_name = "..."]` where the name is empty`#[link_name = "..."]` & `#[link(name = "...")]` parameters`shutdown` on a socket to shut down the write side, attempting to write to the socket will now produce a `BrokenPipe` error rather than `Other`. Map `WSAESHUTDOWN` to `io::ErrorKind::BrokenPipe``riscv64gc-unknown-fuchsia` baseline to RVA22 + vector`NonZero` integers`target.'cfg(..)'.rustdocflags` support in configuration.`white-space: pre-wrap;` property and stripped any `<p>` elements from the rendered html, however this caused issues and unintuitive behavior.  The new behavior should be more predictable, however some multi-line deprecation notes will now be rendered as as single lines.  If this is undesirable, you can use the standard markdown method of forcing a linebreak, which is two spaces followed by a newline (`"\n"`).`missing_doc_code_examples` lint on impl items`#[repr(Int)]` enums in some edge cases involving fields of uninhabited zero-sized types`Pin<Foo>` where `Foo` doesnât implement `Deref`. Some such coercions were previously allowed, but produce a type with no useful public API.`--allow-undefined` on wasm targets`#![reexport_test_harness_main]` attribute`uninhabited_static` lint in dependencies and make it deny-by-default`-Csoft-float``::{self [as name]}`, e.g., `struct S {}; use S::{self as Other};`, is now no longer permitted because `{self}` imports require a module parent.`export_name`, `link_name`, and `avr` targets, Câs `double` type is 32-bit by default, so change `c_double` to `f32` on `BTreeMap::append()` was optimized, which may now cause panics for types with incorrect `aarch64` softfloat targets now have to have `rustc_abi` set to `"softfloat"``cfg(target_abi)``irrefutable_let_patterns` lint no longer lints on let chains`--remap-path-scope` for controlling the scoping of how paths get remapped in the resulting binary`thread::scope`: document how join interacts with TLS destructors`str::contains` on aarch64 targets with `neon` target feature enabled by default`$crate` without renaming, i.e. `use $crate::{self};`, is now no longer permitted due to stricter error checking for `self` imports.`const` or `static`.`Eq::assert_receiver_is_total_eq` and emit future compatibility warnings on manual impls`mut ref` and `mut ref mut` patterns, part of the unstable Match Ergonomics 2024 RFC, were accidentally allowed on stable within struct pattern field shorthand. These patterns are now correctly feature-gated as unstable in this position.`-Z unstable-options` to use. Previously, they could not be used without the standard library, which has no stable build mechanism. In preparation for the `build-std` project adding that support, JSON target specs are being proactively gated to ensure they remain unstable even if `-Z json-target-spec` CLI flag to automatically pass `#[feature]` attributes on invalid targets are now checked`std::thread::spawn` on wasm32-wasip1-threads`std::os::windows::fs::OpenOptionsExt`
The new methods were unstable, but the trait itself is not sealed and so
cannot be extended with non-default methods.`match_same_arms``<[T]>::array_windows``<[T]>::element_offset``LazyCell::get``LazyCell::get_mut``LazyCell::force_mut``LazyLock::get``LazyLock::get_mut``LazyLock::force_mut``impl TryFrom<char> for usize``std::iter::Peekable::next_if_map``std::iter::Peekable::next_if_map_mut``avx512fp16` intrinsics
(excluding those that depend directly on the unstable `f16` type)`f32::consts::EULER_GAMMA``f64::consts::EULER_GAMMA``f32::consts::GOLDEN_RATIO``f64::consts::GOLDEN_RATIO``CARGO_BIN_EXE_<crate>` available at runtime `dyn`-types`Drop` to run at a different point in time.`#[macro_use]`
This will raise an error if macros of the same name are glob imported.
For example if a crate defines their own `matches` macro and then glob imports that,
itâs now ambiguous whether the custom or standard library `core::panic` and `std::panic`, if their import is ambiguous
a new warning (`ambiguous_panic_imports`) is raised.
This may raise a new warning (`#![no_std]` code glob importing the std crate.
Both `core::panic!` and `std::panic!` are then in scope and which is used is ambiguous.`include!(â¦)`s
This can cause previously working includes to no longer compile if they included files which started with a shebang.`std::time::SystemTime::checked_sub_duration` will return `None` for times before the Windows epoch (1/1/1601)`'a` are now NFC normalized.`--remap-path-prefix` given originally.
One side-effect of this change is that paths emitted for local crates in Cargo (path dependencies and workspace members) are no longer absolute but relative when emitted as part of a diagnostic in a downstream crate.`annotate-snippets` for error emission
This should preserve mostly the same outputs in rustc error messages.`clippy::panicking_unwrap` false-positive on field access with implicit deref.`wasm32-wasip2` target.`-Cjump-tables=bool`. The flag was previously called `-Zno-jump-tables`.`specialization` on the `Copy` trait as it is unsound in the presence of lifetime dependent `Clone::clone` instead of performing bitwise copies`std::thread::current()``BTree::append` not update existing keys when appending an entry which already exists`T: RefUnwindSafe` for `vec::IntoIter<T>: UnwindSafe``cargo tree`, support long forms for `--format` variables`--workspace` to `cargo clean``#![doc(document_private_items)]``import``html_favicon_url`, `html_logo_url`, `html_playground_url`, `issue_tracker_base_url`, or `html_no_source` either has a missing value, an unexpected value, or a value of the wrong type, rustdoc will emit the deny-by-default lint `rustdoc::invalid_doc_attributes`.`pin_v2` into the builtin attributes namespace`panic=unwind` was changed from the JS exception handling ABI to the wasm exception handling ABI. If linking C/C++ object files with Rust objects, `-fwasm-exceptions` must be passed to the linker now. On nightly Rust, it is possible to get the old behavior with `-Zwasm-emscripten-eh=false -Zbuild-std`, but it will be removed in a future release.`#[test]` attribute, used to define tests, was previously ignored in various places where it had no meaning (e.g on trait methods or types). Putting the `test` attribute is applied to structs`CARGO_CFG_DEBUG_ASSERTIONS` environment variable in more situations. This will cause crates depending on `static-init` versions 1.0.1 to 1.0.3 to fail compilation with âfailed to resolve: use of unresolved module or unlinked crate `parking_lot`â. See the linked issue for details.`offset_of!` macro are now checked to be well formed.`cargo publish` no longer emits `build.build-dir` config is unset`deref_nullptr` lint from warn-by-default to deny-by-default`...` function parameters without a pattern outside of `extern` blocks`c_int` or `c_uint``mips64el-unknown-linux-muslabi64` link dynamically`Iterator::eq{_by}` for `TrustedLen` iterators`Extend` for tuples`Debug` for `EncodeWide`.`iter::Repeat::last` and `count` will now panic, rather than looping infinitely.`Iterator::last` as well as impl methods like `std::vec::IntoIter::last`.  Now these impl methods will be hidden, freeing up space for inherent methods like `BTreeSet::last`.`-C panic=abort` on Linux by generating unwind tables by default. Build with `-C force-unwind-tables=no` to keep omitting unwind tables.`invalid_macro_export_arguments` is upgraded to deny-by-default and will be reported in dependencies too.`impl DerefMut for Pin<LocalType>``wasm_import_module` attribute cross-crate. This fixes linker errors on WASM targets.`sysv64`, `win64`, `efiapi`, and `aapcs` ABIs.
This brings these ABIs in line with the C ABI: variadic functions can be declared in extern blocks but not defined.`dangling_pointers_from_locals` lint to warn against dangling pointers from local variables`semicolon_in_expressions_from_macros` from warn to deny`integer_to_ptr_transmutes` lint against integer-to-pointer transmutes`sse4a` and `tbm` target features`target_env = "macabi"` and `target_env = "sim"` cfgs as replacements for the `target_abi` cfgs with the same values.`aarch64-pc-windows-gnullvm` and `x86_64-pc-windows-gnullvm` to Tier 2 with host tools.
Note: llvm-tools and MSI installers are missing but will be added in future releases.`aarch64-pc-windows-msvc` to Tier 1`core::panic::Location::file` return type`_by()` variants of `min` / `max`/ `minmax` in `std::cmp``Clone` and `Eq` traits`std::thread`: Return error if setting thread stack size fails
This used to panic within the standard library.`build-dir` is an implementation detail that may change without notice.
(config doc)
(build cache doc)
#15833
#15840`--target` flag and the `build.target` configuration can now take literal
`"host-tuple"` string, which will internally be substituted by the host
machineâs target triple.
#15838
#16003
#16032`*const u8 ->`, and additionally functions that take or return raw pointers will now display their signature properly in search results.`cc`, and pass it via `SDKROOT` env var. This should fix linking issues with `rustc` running inside Xcode. Libraries in `/usr/local/lib` may no longer be linked automatically, if you develop or use a crate that relies on this, you should explicitly set `cargo::rustc-link-search=/usr/local/lib` in a `build.rs` script.`TraitRef<AssocTy: ?Sized>` are now correctly forbidden`#[sanitize(xyz = "on|off")]` built-in attribute that shadows procedural macros with the same name`macro_rules!` macros in expression position that expand to end in a semicolon (`;`), is now deny-by-default. It was already warn-by-default, and a future compatibility warning (FCW) that warned even in dependencies. This lint will become a hard error in the future.`unsafe`, `default`) in inherent impls are no longer syntactically valid`ill_formed_attribute_input` in dependencies`format_args!`, `write!`, and `writeln!` in `if` expression regardless of Edition.`--cfg` and `--check-cfg` arguments are no longer allowed
Cargo compatibility notes:

`cargo package`.
In a future version, this change will apply regardless of `unknown_or_malformed_diagnostic_attributes` lint. This lint has been split up into four finer-grained lints, with `unknown_diagnostic_attributes`: unknown to the current compiler`misplaced_diagnostic_attributes`: placed on the wrong item`malformed_diagnostic_attributes`: malformed attribute syntax or options`malformed_diagnostic_format_literals`: malformed format string literal`lld` by default on `x86_64-unknown-linux-gnu`.`musl` targets now link dynamically by default. Affected targets:
`mips64-unknown-linux-muslabi64``powerpc64-unknown-linux-musl``powerpc-unknown-linux-musl``powerpc-unknown-linux-muslspe``riscv32gc-unknown-linux-musl``s390x-unknown-linux-musl``thumbv7neon-unknown-linux-musleabihf``http.proxy-cainfo` config for proxy certs`gix` for `impl` blocks, now they do when shift is held`unsafe()` wrappers`core::iter::Fuse`âs `Default` impl construct `I::default()` internally as promised in the docs instead of always being empty`MSG_NOSIGNAL` for `UnixStream`
This may change program behavior but results in the same behavior as other primitives (e.g., stdout, network sockets).
Programs relying on signals to terminate them should update handling of sockets to handle errors on write by exiting.`std::env::home_dir` will use the fallback if the `HOME` environment variable is empty`extern "{abi}"`s consistently in all positions. This primarily affects the use of implementing traits on an `extern "stdcall" fn()`, on a platform that doesnât support that, like aarch64-unknown-linux-gnu. Direct usage of these unsupported ABI strings by declaring or defining functions was already rejected, so this is only a change for consistency.`proc_macro_derive` macro has correct arguments when applied to the crate root`feature(generic_arg_infer)`)`mismatched_lifetime_syntaxes` lint.
This lint detects when the same lifetime is referred to by different syntax categories between function arguments and return values, which can be confusing to read, especially in unsafe code.
This lint supersedes the warn-by-default `elided_named_lifetimes` lint.`unpredictable_function_pointer_comparisons` to also lint on function pointer comparisons in external macros`dangerous_implicit_autorefs` lint deny-by-default`kl` and `widekl` target features for x86`sha512`, `sm3` and `sm4` target features for x86`f`, `d`, `frecipe`, `lasx`, `lbt`, `lsx`, and `lvz``i128` and `u128` from `improper_ctypes_definitions``repr128` (`#[repr(u128)]`, `#[repr(i128)]`)`#![doc(test(attr(..)))]` everywhere`extern "C"` functions on the `wasm32-unknown-unknown` target now have a standards compliant ABI`loongarch32-unknown-none` and `loongarch32-unknown-none-softfloat``x86_64-apple-darwin` is in the process of being demoted to Tier 2 with host tools`NonZero<char>``File::lock_shared``File::try_lock``File::try_lock_shared``File::unlock``NonNull::from_ref``NonNull::from_mut``NonNull::without_provenance``NonNull::with_exposed_provenance``NonNull::expose_provenance``OsString::leak``PathBuf::leak``Result::flatten``std::os::linux::net::TcpStreamExt::quickack``std::os::linux::net::TcpStreamExt::set_quickack``cargo fix` and `cargo clippy --fix` now default to the same Cargo target selection as other build commands. Previously it would apply to all targets (like binaries, examples, tests, etc.). The `--edition` flag still applies to all targets.`runner` setting to run the tests. If you need to disable tests for a target, you can use the ignore doctest attribute to specify the targets to ignore.`missing_fragment_specifier` an unconditional error`aarch64-unknown-none-softfloat` causes a warning because mixing code with and without that target feature is not properly supported by LLVM`sized_hierarchy` unstable feature. See the FCP report for a code example.`std::intrinsics::{copy,copy_nonoverlapping,write_bytes}` are now proper intrinsics. There are no debug assertions guarding against UB, and they cannot be coerced to function pointers.`std::intrinsics::drop_in_place``std::ptr::copy_*` intrinsics also perform the static self-init checks`#![feature(let_chains)]` in the 2024 edition.
This feature allows `&&`-chaining `while`, allowing intermixture with boolean expressions. The patterns inside the `#![feature(naked_functions)]`.
Naked functions allow writing functions with no compiler-generated epilogue and prologue, allowing full control over the generated assembly for a particular function.`#![feature(cfg_boolean_literals)]`.
This allows using boolean literals as `cfg` predicates, e.g. `#[cfg(true)]` and `#[cfg(false)]`.`#[bench]` attribute. Usage of `#![feature(custom_test_frameworks)]` already triggered a deny-by-default future-incompatibility lint since Rust 1.77, but will now become a hard error.`invalid_null_arguments` lint to prevent invalid usage of null pointers.
This lint is uplifted from `clippy::invalid_null_ptr_usage`.`#[should_panic]` test failure message.`[T; N]::from_fn` is generated in order of increasing indices., for those passing it a stateful closure.`--nocapture` is deprecated in favor of the more consistent `--no-capture` flag.`{float}::NAN` is a quiet NaN.`ignore-*` attributes.`--test-runtool` and `--test-runtool-arg` CLI options to specify a program (like qemu) and its arguments to run a doctest.`tt` fragment specifier can often fix these macros.`asm_goto` feature`..EXPR`) after unary operators `-`, and `*`.`Self: Sized` bounds in `feature(precise_capturing_in_traits)` allowing `use<...>` bounds on return position `impl Trait` in `trait`s`Vec::with_capacity` guarantees it allocates with the amount requested, even if `Vec::capacity` returns a different number.`std::arch` intrinsics which donât take pointer arguments can now be called from safe code if the caller has the appropriate target features already enabled (https://github.com/rust-lang/stdarch/pull/1714, https://github.com/rust-lang/stdarch/pull/1716, https://github.com/rust-lang/stdarch/pull/1717)`env::home_dir``ControlFlow` as `#[must_use]``assert_eq!` and `vec!` now support `const {...}` expressions`#![crate_name]` attribute`..` inside of struct patterns (`let Struct { #[attribute] .. }) =``ptr_cast_add_auto_to_object` lint into hard error`unused_unsafe` warnings in existing codebases.`width` and `precision` formatting options to 16 bits on all targets`advapi32`, except on win7. Code such as C libraries that were relying on this assumption may need to explicitly link advapi32.`cfg(true)` attributes.`#[target_feature]` attribute.`missing_abi` lint now warns-by-default.`--x`) as written in other languages. This was previously a clippy lint, `clippy::double_neg`, and is now available directly in Rust as `double_negations`.`repr()` attribute applied to invalid items are now correctly rejected.`#![test]` and `#![rustfmt::skip]` are no longer accepted in more places than intended.`-O` to mean `-C opt-level=3` instead of `-C opt-level=2` to match Cargoâs defaults.`i686-unknown-redox` target with `i586-unknown-redox`.`i686-unknown-hurd-gnu` to Pentium 4.`{aarch64-unknown,x86_64-pc}-nto-qnx710_iosock`.
For supporting Neutrino QNX 7.1 with `io-socket` network stack.`{aarch64-unknown,x86_64-pc}-nto-qnx800`.
For supporting Neutrino QNX 8.0 (`no_std`-only).`{x86_64,i686}-win7-windows-gnu`.
Intended for backwards compatibility with Windows 7. `{x86_64,i686}-win7-windows-msvc` are the Windows MSVC counterparts that already exist as Tier 3 targets.`amdgcn-amd-amdhsa`.`x86_64-pc-cygwin`.`{mips,mipsel}-mti-none-elf`.
Initial bare-metal support.`m68k-unknown-none-elf`.`armv7a-nuttx-{eabi,eabihf}`, `aarch64-unknown-nuttx`, and `thumbv7a-nuttx-{eabi,eabihf}`.`FromBytesWithNulError` in `CStr::from_bytes_with_nul(bytes: &[u8]) -> Result<&Self, FromBytesWithNulError>` was changed from an opaque struct to an enum, allowing users to examine why the conversion failed.`RustcDecodable` and `RustcEncodable`.`--logfile` option.`std::fs::remove_file` will now remove read-only files.
These APIs are now stable in const contexts:

`--package` and `cargo login` to avoid shell history leaks.`SourceID` comparisons. This may potentially change behavior if the canonicalized URL compares differently in alternative registries.`wasm_c_abi` future compatibility warning is now a hard error.
Users of `wasm-bindgen` should upgrade to at least version 0.2.89, otherwise compilation will fail.`#![no_start]` and `#![crate_id]`.`cenum_impl_drop_cast` has been made into a hard error. This means it is now an error to cast a field-less enum to an integer if the enum implements `#[diagnostic::do_not_recommend]``#[no_mangle]` and `#[export_name]` attributes.`-Zpolymorphize` has been removed, see https://github.com/rust-lang/compiler-team/issues/810 for some background.
Panics in the standard library now have a leading `library/` in their path

`std::env::home_dir()` on Windows now ignores the non-standard `$HOME` environment variable

It will be un-deprecated in a subsequent release.

Add `AsyncFn*` to the prelude in all editions.

`CARGO_CFG_FEATURE` to build scripts
`rustc` no longer treats the `test` cfg as a well known check-cfg, instead it is up to the build systems and users of `--check-cfg`<sup>1</sup> to set it as a well known cfg using `--check-cfg=cfg(test)`.

This is done to enable build systems like Cargo to set it conditionally, as not all source files are suitable for unit tests.
Cargo (for now) unconditionally sets the `test` cfg as a well known cfg.

Disable potentially incorrect type inference if there are trivial and non-trivial where-clauses

`std::env::home_dir()` has been deprecated for years, because it can give surprising results in some Windows configurations if the `HOME` environment variable is set (which is not the normal configuration on Windows). We had previously avoided changing its behavior, out of concern for compatibility with code depending on this non-standard configuration. Given how long this function has been deprecated, weâre now fixing its behavior as a bugfix. A subsequent release will remove the deprecation for this function.

Make `core::ffi::c_char` signedness more closely match that of the platform-default `char`

This changed `c_char` from an `i8` to `u8` or vice versa on many Tier 2 and 3
targets (mostly Arm and RISC-V embedded targets). The new definition may
result in compilation failures but fixes compatibility issues with C.

The `libc` crate matches this change as of its 0.2.169 release.

When compiling a nested `macro_rules` macro from an external crate, the content of the inner `macro_rules` is now built with the edition of the external crate, not the local crate.

Increase `sparcv9-sun-solaris` and `x86_64-pc-solaris` Solaris baseline to 11.4.

Show `abi_unsupported_vector_types` lint in future breakage reports

Error if multiple super-trait instantiations of `dyn Trait` need associated types to be specified but only one is provided

Change `powerpc64-ibm-aix` default `codemodel` to large

These APIs are now stable in const contexts

`LSX` target feature for LoongArch Linux targets`-Zprofile` flag (âgcov-styleâ coverage instrumentation) has been removed. This does not affect the stable flags for coverage instrumentation (`-Cinstrument-coverage`) and profile-guided optimization (`-Cprofile-generate`, `-Cprofile-use`), which are unrelated and remain available.`wasm32-wasi` has been removed as the target is now named `wasm32-wasip1`. This completes the transition plan for this target following the introduction of `&pin (mut|const) T` is now parsed as a type which in theory could affect macro expansion results in some edge cases`CARGO_MANIFEST_PATH` environment variable, similar to `CARGO_MANIFEST_DIR` but pointing directly to the manifest file.`package.autolib` to the manifest, allowing `[lib]` auto-discovery to be disabled.`# headers` from the main itemâs doc comment. This is similar to a third-party feature provided by the rustdoc-search-enhancements browser extension.
Warn against function pointers using unsupported ABI strings.

Check well-formedness of the source typeâs signature in fn pointer casts. This partly closes a soundness hole that comes when casting a function item to function pointer

Use equality instead of subtyping when resolving type dependent paths.

Linking on macOS now correctly includes Rustâs default deployment target. Due to a linker bug, you might have to pass `MACOSX_DEPLOYMENT_TARGET` or fix your `#[link]` attributes to point to the correct frameworks. See https://github.com/rust-lang/rust/pull/129369.

Rust will now correctly raise an error for `repr(Rust)` written on non-`struct`/`enum`/`union` items, since it previously did not have any effect.

The future incompatibility lint `deprecated_cfg_attr_crate_type_name` has been made into a hard error. It was used to deny usage of `#![crate_type]` and `#![crate_name]` attributes in `#![cfg_attr]`, which required a hack in the compiler to be able to change the used crate type and crate name after cfg expansion.
Users can use `--crate-type` instead of `#![cfg_attr(..., crate_type = "...")]` and `--crate-name` instead of `#![cfg_attr(..., crate_name = "...")]` when running `rustc`/`cargo rustc` on the command line.
Use of those two attributes outside of `#![cfg_attr]` continue to be fully supported.

Until now, paths into the sysroot were always prefixed with `/rustc/$hash` in diagnostics, codegen, backtrace, e.g.

```
thread 'main' panicked at 'hello world', map-panic.rs:2:50
stack backtrace:
   0: std::panicking::begin_panic
             at /rustc/a55dd71d5fb0ec5a6a3a9e8c27b2127ba491ce52/library/std/src/panicking.rs:616:12
   1: map_panic::main::{{closure}}
             at ./map-panic.rs:2:50
   2: core::option::Option<T>::map
             at /rustc/a55dd71d5fb0ec5a6a3a9e8c27b2127ba491ce52/library/core/src/option.rs:929:29
   3: map_panic::main
             at ./map-panic.rs:2:30
   4: core::ops::function::FnOnce::call_once
             at /rustc/a55dd71d5fb0ec5a6a3a9e8c27b2127ba491ce52/library/core/src/ops/function.rs:248:5
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
```
RFC 3127 said

We want to change this behaviour such that, when `rust-src` source files can be discovered, the virtual path is discarded and therefore the local path will be embedded, unless there is a `--remap-path-prefix` that causes this local path to be remapped in the usual way.


#129687 implements this behaviour, when `rust-src` is present at compile time, `rustc` replaces `/rustc/$hash` with a real path into the local `rust-src` component with best effort.
To sanitize this, users must explicitly supply `--remap-path-prefix=<path to rust-src>=foo` or not have the `rust-src` component installed.

The allow-by-default `missing_docs` lint used to disable itself when invoked through `rustc --test`/`cargo test`, resulting in `#[expect(missing_docs)]` emitting false positives due to the expectation being wrongly unfulfilled. This behavior has now been removed, which allows `#[expect(missing_docs)]` to be fulfilled in all scenarios, but will also report new `missing_docs` diagnostics for publicly reachable `#[cfg(test)]` items, integration test crate-level documentation, and publicly reachable items in integration tests.

The `armv8r-none-eabihf` target now uses the Armv8-R required set of floating-point features.

Fix a soundness bug where rustc wouldnât detect unconstrained higher-ranked lifetimes in a `dyn Trait`âs associated types that occur due to supertraits.

Update the minimum external LLVM version to 18.

Remove `aarch64-fuchsia` and `x86_64-fuchsia` target aliases in favor of `aarch64-unknown-fuchsia` and `x86_64-unknown-fuchsia` respectively.

The ABI-level exception class of a Rust panic is now encoded with native-endian bytes, so it is legible in hex dumps.

Visual Studio 2013 is no longer supported for MSVC targets.

The sysroot no longer contains the `std` dynamic library in its top-level `lib/` dir.

`explicit_builtin_cfgs_in_flags` lint in order to prevent incoherent state, eg. `windows` cfg active but target is Linux based. The appropriate `binary_search` which significantly improves performance (#128254). However when a sorted slice has multiple values which compare equal, the new implementation may select a different value among the equal ones than the old implementation.`multivalue` and `reference-types` are now
both enabled by default. These two features both have subtle changes implied
for generated WebAssembly binaries. For the `-Ctarget-feature=+multivalue` has a different meaning than it did in LLVM 18
and prior. There is no longer any supported means to generate a module that has
a function with multiple returns in WebAssembly from Rust source code. For the
`call_indirect`, a
commonly used instruction by the WebAssembly backend, has changed. Validators
and parsers which donât understand the `target_features` custom section and may affect downstream tooling such as
`wasm-opt` consuming the module. Generating a WebAssembly module that disables
default features requires `-Zbuild-std` support from Cargo and more information
can be found at
rust-lang/rust#128511.`#[expect]` for lints (RFC 2383), like `#[allow]` with a warning if the lint is `elided_lifetimes_in_associated_constant` to deny.`offset_from`: always allow pointers to point to the same address.`-Cinline-threshold=...`.`transmute` size checks.`box_pointers` lint.`xtensa-esp32-none-elf`, `xtensa-esp32s2-none-elf`, `xtensa-esp32s3-none-elf``xtensa-esp32-espidf`, `xtensa-esp32s2-espidf`, `xtensa-esp32s3-espidf``arm64ec-pc-windows-msvc` to Tier 2.`loongarch64-unknown-linux-musl` to Tier 2 with host tools.`PanicInfo` and stdâs `{Rc,Arc}::make_mut()` to unsized types.`driftsort` and unstable `ipnsort`. All `slice::sort*` and `slice::select_nth*` methods are expected to see significant performance improvements. See the research project for more details.`create_dir_all` with respect to empty paths.`Command`âs batch files argument escaping not working when file name has trailing whitespace or periods (CVE-2024-43402).`char::from_u32_unchecked` (function)`CStr::count_bytes``CStr::from_ptr``.cargo_vcs_info.json` is always included, even when `--allow-dirty` is passed.`package.license-file` and `package.readme` pointing to non-existent files during packaging.`--release`/`--debug` flag along with the `--profile` flag.`lib.plugin` key support in `Cargo.toml`. Rust plugin support has been deprecated for four years and was removed in 1.75.0.
Usage of the `wasm32-wasi` target will now issue a compiler warning and request users switch to the `wasm32-wasip1` target instead. Both targets are the same, `wasm32-wasi` is only being renamed, and this change to the WASI target is being done to enable removing `wasm32-wasi` in January 2025.

We have renamed `std::panic::PanicInfo` to `std::panic::PanicHookInfo`. The old name will continue to work as an alias, but will result in a deprecation warning starting in Rust 1.82.0.

`core::panic::PanicInfo` will remain unchanged, however, as this is now a *different type*.

The reason is that these types have different roles: `std::panic::PanicHookInfo` is the argument to the panic hook in std context (where panics can have an arbitrary payload), while `core::panic::PanicInfo` is the argument to the `#[panic_handler]` in no_std context (where panics always carry a formatted *message*). Separating these types allows us to add more useful methods to these types, such as `std::panic::PanicHookInfo::payload_as_str()` and `core::panic::PanicInfo::message()`.

The new sort implementations may panic if a typeâs implementation of `Ord` (or the given comparison function) does not implement a total order as the trait requires. `Ord`âs supertraits (`PartialOrd`, `Eq`, and `PartialEq`) must also be consistent. The previous implementations would not ânoticeâ any problem, but the new implementations have a good chance of detecting inconsistencies, throwing a panic rather than returning knowingly unsorted data.

In very rare cases, a change in the internal evaluation order of the trait solver may result in new fatal overflow errors.

`dead_code` lint from 1.80.0`custom_code_classes_in_docs` feature`std::env::{set_var, remove_var}` can no longer be converted to safe function pointers and no longer implement the `Fn` family of traits`Self` constructor from outer item is referenced in inner nested item`indirect_structural_match` and `pointer_structural_match` lints into hard errors`where_clause_object_safety` lint a regular object safety violation`proc_macro_back_compat` lint into a hard error.`std::sync::ReentrantLockGuard<T>` is no longer `Sync` if `T: !Sync` which means `std::io::StdoutLock` and `std::io::StderrLock` are no longer Sync`FromIterator for Box<str>`.
Notably, this breaks versions of the `time` crate before 0.3.35, due to no longer inferring the implementation for `Box<[_]>`.`-C strip` to only affect binaries, not artifacts like `.pdb`.`-Crelro-level` for controlling runtime link hardening.`aarch64-apple-visionos` and `aarch64-apple-visionos-sim` tier 3 targets.`riscv32ima-unknown-none-elf` tier 3 target.`i686-pc-windows-gnullvm`, and `lib.name`, always normalizing to `_`.`cargo add`.`gitoxide` by default for listing files.`/` in docs to begin a search.`wasm-bindgen v0.2.88` to generate forward-compatible bindings.`INVALID_DOC_ATTRIBUTES` lint deny by default`use` checking
Target changes:

`impl Read for &Stdin``std::error::Error` related implementations`impl<Fd: AsFd>` impl take `impl From<TryReserveError> for io::Error``Freeze` trait will allow proper support for such code.`soft_unstable` in future breakage reports.`--verbose` in diagnostic output.`unused_tuple_struct_fields` lint into `fn`/`riscv32{im|imafc}-unknown-none-elf` targets to tier 2.`()`.`localStorage`.`ambiguous_wide_pointer_comparisons` that supersedes `clippy::vtable_address_comparisons``Arc::unwrap_or_clone``Rc::unwrap_or_clone``Result::inspect``Result::inspect_err``Option::inspect``type_name_of_val``std::hash::{DefaultHasher, RandomState}`
These were previously available only through `std::collections::hash_map`.`ptr::{from_ref, from_mut}``ptr::addr_eq`
See Cargo release notes.

`print_tts`
This change can break some naive manual parsing of token trees in proc macro
code which expect a particular structure after `.to_string()`, rather than just arbitrary Rust code.`IMPLIED_BOUNDS_ENTAILMENT` into a hard error from a lint`impl FromIterator for Vec`
for more details.`[workspace.members]` automatically.`#[repr(transparent)]` if it isnât part of the public ABI.`librustc_driver.so` with BOLT.`rustc-codegen-cranelift` as rustup component on the nightly channel.`--extern``impl From<{&,&mut} [T; N]> for Vec<T>` is known to cause some inference failures with overly-generic code. In those examples using the `tui` crate, the combination of `AsRef<_>` and `Into<Vec>` leaves the middle type ambiguous, and the new None this cycle.

`#![cfg(FALSE)]` behaves on crate root`clippy::undropped_manually_drops` lint`clippy::invalid_utf8_in_unchecked` lint as `invalid_from_utf8_unchecked` and `invalid_from_utf8``clippy::cast_ref_to_mut` lint as `invalid_reference_casting``clippy::cmp_nan` lint as `invalid_nan_comparisons``dyn Trait` objects`[]``-Zdoctest-in-workspace` by default. When running each documentation
test, the working directory is set to the root directory of the package the
test belongs to.
docs
#12221
#12288`build.jobs`
parallelism back to the default.
#12222`Display` for `Ipv6Addr` for IPv4-compatible addresses`mips*-unknown-linux-gnu*` targets from host tier 2 to target tier 3 support.`raw-dylib`, `link_ordinal`, `import_name_type` and `-Cdlltool`.`clippy::{drop,forget}_{ref,copy}` lints.`PlaceMention`,
making `let _ =` patterns more consistent with respect to the borrow checker.`--print deployment-target` flag for Apple targets.`extern "C-unwind"` and friends.
The existing `*-linux-musl` targets to 1.2.3,
enabling time64 on 32-bit systems.`debugger_visualizer`
for embedding metadata like Microsoftâs Natvis.`apple-m1` as the target CPU for `aarch64-apple-darwin`.`x86_64h-apple-darwin` target.`loongarch64-unknown-linux-gnu` to Tier 2 with host tools.`From<&[T]> for Box<[T]>` bound to `T: Clone`.`T: Send` bound
in `Error for mpsc::SendError<T>` and `TrySendError<T>`.`alloc::realloc`
to match `Layout` requirements that the size must not exceed `isize::MAX`.`const {}` syntax for `std::thread_local`.
This syntax was stabilized in Rust 1.59, but not previously mentioned in release notes.`CStr::is_empty``BuildHasher::hash_one``NonZeroI*::is_positive``NonZeroI*::is_negative``NonZeroI*::checked_neg``NonZeroI*::overflowing_neg``NonZeroI*::saturating_neg``NonZeroI*::wrapping_neg``Neg for NonZeroI*``Neg for &NonZeroI*``From<[T; N]> for (T...)`
(array to N-tuple for N in 1..=12)`From<(T...)> for [T; N]`
(N-tuple to array for N in 1..=12)`windows::io::AsHandle for Box<T>``windows::io::AsHandle for Rc<T>``windows::io::AsHandle for Arc<T>``windows::io::AsSocket for Box<T>``windows::io::AsSocket for Rc<T>``windows::io::AsSocket for Arc<T>``workspace_default_members` to the output of `cargo metadata`.`cargo new`/`cargo init`.`rustdoc::unescaped_backticks` lint for broken inline code.`~~old~~` vs. `~new~`)`TypeId`.
Code that uses a constant `log`
crateâs `kv_unstable` feature should update to `log v0.4.18` or later.`sysroot` crate to represent the standard library crates.
This does not affect stable users, but may require adjustment in tools that build their own standard library.`rustup`. When
Cargo detects it will run `RUSTUP_TOOLCHAIN`. However, itâs not expected to affect normal users.`RUSTUP_HOME` and
`[env]` configuration
table. This is considered to be not a use case Cargo would like to support, since it will likely
cause problems or lead to confusion.`format_args` invocations to uses captures`let _ = expr` position.`-Cdebuginfo=line-tables-only`), which may eventually become the default for `-Cdebuginfo=1`.`unused_allocation` lint against `Box::new` too`i586-pc-nto-qnx700` (QNX Neutrino RTOS, version 7.0),`concat!``Cell``partial_cmp` to implement tuple `lt`/`le`/`ge`/`gt``atomic_as_ptr``nonnull_slice_from_raw_parts``once_cell``nonzero_min_max``FnPtr` trait that is implemented for all function pointers
This extends `Pointer`, `Hash`, `PartialOrd`, and `libtest` from supporting `-Zunstable-options``sign-ext` and `mutable-globals` features in codegen
This may cause incompatibility with older execution environments.`*-pc-windows-gnu` on CI to mingw-w64 v10 and GCC 12.2.`true` and `false` as boolean flags in compiler arguments.`DispatchFromDyn` for cell types, allowing downstream experimentation with custom method receivers.`fmt::Arguments::as_str()` may return `Some(_)` in more cases after optimization, subject to change.`AsFd` and `AsRawFd` for `Rc`.`CARGO_BIN_NAME` environment variable also for binary examples.`rust-analysis` component from `-Zsave-analysis` flag has been removed from the compiler as well.`default-features = true`, Cargo will enable default features of that dependency.`CARGO_HOME` in the `tools = [...]` is set in bootstrap.toml, we will respect a missing rustdoc in that list. By
default rustdoc remains included. To retain the prior behavior explicitly add `"rustdoc"` to the
list.`@revoked` and a better error message for `@cert-authority` in Cargoâs SSH host key verification`$prefix-clang` as `clang` in linker detection code`{core,std}::pin::pin!``impl From<bool> for {f32,f64}``std::path::MAIN_SEPARATOR_STR``impl DerefMut for PathBuf``cargo build --verbose` tells you more about why it recompiles.`net.git-fetch-with-cli` option enabled`clippy::uninlined_format_args` to pedantic.`C` or `cdecl`.`verbatim`.
Added, updated, and removed targets:

`powerpc64-ibm-aix`.`mipsel-sony-psx`.`aarch64-unknown-nto-qnx710` and `x86_64-pc-nto-qnx710`.`aarch64-unknown-uefi`, `i686-unknown-uefi`, and `x86_64-unknown-uefi`.`linuxkernel` targets (not used by the actual kernel).`crossbeam-channel` into `std::sync::mpsc`.`compiler_builtins` with `repr(Rust)` types now groups m*2^n-sized fields with
equivalently aligned ones.
This is intended to be an optimization, but it is also known to increase type
sizes in a few cases for the placement of enum tags. As a reminder, the layout
of `||` will now drop temporaries from their sub-expressions in
evaluation order, left-to-right.
Previously, it was âtwistedâ such that the `-export-dynamic` to `wasm-ld`.`main` is now mangled as `__main_void` on `repr(Int)` enums
`must_use` on `impl ExactSizeIterator` respect the existing `Iterator`.`..=X` in patterns`clippy::for_loops_over_fallibles` lint into rustc`sym` operands in inline assembly`#[derive(Default)]` on a generic `#[default]` enum adding unnecessary `cargo remove` to remove dependencies from Cargo.toml
See detailed release notes for more.

`ProceduralMasquerade` hack to older versions of `rental``__heap_base` and `__data_end` on wasm32-wasi.`__wasm_init_memory` on WebAssembly.`__tls_*` on wasm32-unknown-unknown.`libresolv` in libstd on Darwin`libstd` no longer pull in `libiconv.dylib` on Darwin)`order_dependent_trait_objects` show up in future-breakage reports`as` casts of enums with `let else``let_underscore_drop` and `let_underscore_lock` from Clippy`break`ing from arbitrary labeled blocks (âlabel-break-valueâ)`MaybeUninit` is the correct way to work with uninitialized memory.
New targets:

`PartialEq::ne` in derive(PartialEq)`BCRYPT_RNG_ALG_HANDLE` by default`System` with direct system allocator calls`std::layout::Layout` size must not overflow `align`
This also changes the safety conditions on `Layout::from_size_align_unchecked`.`PollFn` now only implements `Unpin` if the closure is `#![cfg_attr(..., crate_type = ...)]` to set the crate type
This strengthens the forward compatibility lint deprecated_cfg_attr_crate_type_name to deny.`llvm-has-rust-patches` allows setting the build system to treat the LLVM as having Rust-specific patches
This option may need to be set for distributions that are building Rust with a patched LLVM via `llvm-config`, not the built-in LLVM.`x.sh` and `x.ps1` shell scripts`&T` if every byte in `T` is inside an `#[used]` as llvm.compiler.used for ELF targets`--diagnostic-width` compiler flag to define the terminal width.`rust-lld` for iOS, tvOS and watchOS`OsString`: Implement `fmt::Write` for `FusedIterator` for `std::net::[Into]Incoming``impl<T: AsRawFd> AsRawFd for {Arc,Box}<T>``ptr::copy` and `ptr::swap` are doing untyped copies`available_parallelism``mem::uninitialized`
These types were previously stable in `std::ffi`, but are now also available in `core` and `alloc`:

These types were previously stable in `std::os::raw`, but are now also available in `core::ffi` and `std::ffi`:

These APIs are now usable in const contexts:

`workspace.package`
and
`workspace.dependencies`
for more details on how to define these common settings.`--jobs` argument can now take a negative number to count backwards from
the max CPUs.`--crate-type`
flag to `cargo rustc` to override the crate type.`rev` field.`-linux-gnu` targets are now at least kernel 3.2 and glibc 2.17, for targets that previously supported older versions: Increase the minimum linux-gnu versions`U` is not larger than `BTreeMap` was fixed that allowed data it was borrowing to be dropped before the container.`thumbv6m-none-eabi` target, some incorrect `Vec::insert` for the case where `index == len`.`bundle` native library modifier, also removing the
deprecated `static-nobundle` linking kind.
* Refer to Rustâs platform support page for more information on Rustâs tiered platform support.

`--config path` command-line argument.`CARGO_PKG_RUST_VERSION`.`#[link]` attributes are now checked more strictly, which may introduce
errors for invalid attribute arguments that were previously ignored.`Duration`. The converted
duration can differ slightly from what it was.
These changes provide no direct user facing benefits, but represent significant improvements to the internals and overall performance of rustc and related tools.

Rust 1.62.1 addresses a few recent regressions in the compiler and standard library, and also mitigates a CPU vulnerability on Intel SGX.

`unaligned_references` lint deny-by-default
This lint is also a future compatibility lint, and is expected to eventually
become a hard error.`#[thread_local]` attribute on non-static items`x86_64-unknown-none` target to Tier 2*`name@version` syntax in addition to the
previous `name:version` to align with the behavior in `cargo install` and `cargo yank` also now support this syntax so the
version does not need to passed as a separate flag.`git` and `registry` directories in Cargoâs home directory (usually
`~/.cargo`) are now marked as cache directories so that they are not
included in backups or content indexing (on Windows).`@` argfile support, which will use âresponse filesâ if the
command-line to `cargo test` now passes `rustdoc` if the specified target is
the same as the host target.
#10594`File` reads or writes cannot
complete synchronously.`const fn` signatures can now include generic trait bounds`ManuallyDrop<T>` is now documented to have the same layout as `#[ignore = "â¦"]` messages are printed when running tests`std::io::stdio::lock()` return `let out = std::io::stdout().lock();` because `out` would outlive the return value of `stdout()`. Such code now works, eliminating a common pitfall that affected many Rust users.`Vec::from_raw_parts` is now less restrictive about its inputs`std::thread::available_parallelism` now takes cgroup quotas into account. Since `Pin::static_mut``Pin::static_ref``Vec::retain_mut``VecDeque::retain_mut``Write` for `Cursor<[u8; N]>``std::os::unix::net::SocketAddr::from_pathname``std::process::ExitCode` and `std::process::Termination`. The stabilization of these two APIs now makes it possible for programs to return errors from `std::thread::JoinHandle::is_finished`
No feature changes, but see compatibility notes.

`whole-archive` in some cases, but now rustc tries not to use `a` depends on `b` then `+whole-archive` modifier.`ident` matchers wrapped in groups`#` in `r#` raw string literals is now required to be less than 256`cargo vendor` now only accepts one value for each `--sync` flag`all()` and `any()` are always evaluated to detect errors, instead of short-circuiting. The compatibility considerations here arise in nightly-only code that used the short-circuiting behavior of `all` to write something like `cfg(all(feature = "nightly", syntax-requiring-nightly))`, which will now fail to compile. Instead, use either `cfg_attr(feature = "nightly", ...)` or nested uses of `#[cfg(panic = "...")]` for either `"unwind"` or `"abort"`.`#[cfg(target_has_atomic = "...")]` for each integer size and `"ptr"`.`Instant::now`. On platforms that donât provide monotonic clocks, an
instant is not guaranteed to be greater than an earlier instant anymore.`Instant::{duration_since, elapsed, sub}` do not panic anymore on underflow,
saturating to `0` instead. In the real world the panic happened mostly
on platforms with buggy monotonic clock implementations rather than catching
programming errors like reversing the start and end times. Such programming
errors will now result in This release disables incremental compilation, unless the user has explicitly opted in via the newly added RUSTC_FORCE_INCREMENTAL=1 environment variable. This is due to a known and relatively frequently occurring bug in incremental compilation, which causes builds to issue internal compiler errors. This particular bug is already fixed on nightly, but that fix has not yet rolled out to stable and is deemed too risky for a direct stable backport.

As always, we encourage users to test with nightly and report bugs so that we can track failures and fix issues earlier.

See 94124 for more details.

`Result::copied``Result::cloned``arch::asm!``arch::global_asm!``ops::ControlFlow::is_break``ops::ControlFlow::is_continue``TryFrom<char> for u8``char::TryFromCharError`
implementing `Error``iter::zip``NonZeroU8::is_power_of_two``NonZeroU16::is_power_of_two``NonZeroU32::is_power_of_two``NonZeroU64::is_power_of_two``NonZeroU128::is_power_of_two``NonZeroUsize::is_power_of_two``DoubleEndedIterator for ToLowercase``DoubleEndedIterator for ToUppercase``TryFrom<&mut [T]> for [T; N]``UnwindSafe for Once``RefUnwindSafe for Once`
Const-stable:

`mem::MaybeUninit::as_ptr``mem::MaybeUninit::assume_init``mem::MaybeUninit::assume_init_ref``ffi::CStr::from_bytes_with_nul_unchecked``#![cfg_attr]`
This adds a future compatibility lint to supporting the use of cfg_attr
wrapping either crate_type or crate_name specification within Rust files;
it is recommended that users migrate to setting the equivalent command line
flags.`#[no_link]` attribute on name resolution
This may expose new names, leading to conflicts with preexisting names in a
given namespace and a compilation failure.
Fix many cases of normalization-related ICEs

Replace dominators algorithm with simple Lengauer-Tarjan

Store liveness in interval sets for region inference

Remove `in_band_lifetimes` from the compiler and standard library, in preparation for removing this
unstable feature.

`std::fs::remove_dir_all` (CVE-2022-21658)`useless_format` Clippy lint`non_send_fields_in_send_ty` Clippy lint to nursery`{ident}` in the string. This works in all macros accepting format strings. Support for this in `panic!` (`panic!("{ident}")`) requires the 2021 edition; panic invocations in previous editions that appear to be trying to use this will result in a warning lint about not having the intended effect.`*const T` pointers can now be dereferenced in const contexts.`Unsize` have been relaxed.`strip` option to remove these symbols to produce smaller release binaries. Note that this release only includes support in rustc, not directly in cargo.`-Cremark``copy` and `copy_nonoverlapping``RefUnwindSafe` for `Rc<T>``Termination` for `Result<Infallible, E>`. This allows writing `fn main() -> Result<Infallible, ErrorType>`, for a program whose successful exits never involve returning from `exit`, or that uses `exec` to run another program).`std::process::Command` will no longer search the current directory for executables.`#[derive]` and will see the original (pre-`m!{ .. }.method()` and `m!{ .. }?`.`armv6k-nintendo-3ds` at Tier 3*.`armv7-unknown-linux-uclibceabihf` at Tier 3*.`m68k-unknown-linux-gnu` at Tier 3*.`aarch64-kmc-solid_asp3`, `armv7a-kmc-solid_asp3-eabi`, `armv7a-kmc-solid_asp3-eabihf``Vec::leak``#[repr(i8)]` to `Ordering``File::read_to_end` and `read_to_string``#![deny(dead_code)]`.`binding @ pattern` can now also introduce new bindings.`--force-warn`.
This feature is primarily intended for usage by `aarch64-apple-ios-sim` to Tier 2*.`powerpc-unknown-freebsd` at Tier 3*.`riscv32imc-esp-espidf` at Tier 3*.`AtomicU{64,128}` over Mutex for Instant backsliding protection.
For this use case, atomics scale much better under contention.`Extend<(A, B)>` for `(Extend<A>, Extend<B>)``impl From<[(K, V); N]>` for all collections.`P: Unpin` bound on impl Future for Pin.`=`). Now, these functions will
just treat such names as nonexistent variables, since the OS cannot represent
the existence of a variable with such a name.`std::os::unix::fs::chroot``UnsafeCell::raw_get``BufWriter::into_parts``core::panic::{UnwindSafe, RefUnwindSafe, AssertUnwindSafe}`
These APIs were previously stable in `Vec::shrink_to``String::shrink_to``OsString::shrink_to``PathBuf::shrink_to``BinaryHeap::shrink_to``VecDeque::shrink_to``HashMap::shrink_to``HashSet::shrink_to``SEMICOLON_IN_EXPRESSIONS_FROM_MACROS` warn by default`libc::getpid` / `std::process::id` from `Command::pre_exec`
may return different values on glibc <= 2.24.
Rust now invokes the `clone3` system call directly, when available, to use new functionality
available via that system call. Older versions of glibc cache the result of `getpid`, and only
update that cache when calling glibcâs clone/fork functions, so a direct system call bypasses
that cache update. glibc 2.25 and newer no longer cache `X..`), which will start at `X` and
will end at the maximum value of the integer.`std::prelude` (e.g. `use std::prelude::rust_2021::*;`).`string::Drain` now implements `AsRef<str>` and `AsRef<[u8]>`.
The following previously stable functions are now `const`.

`"default_run"`
field from the manifest.`cargo d` as an alias for `cargo doc`.`{lib}` as formatting option for `"lib_name"`
of packages.`CTRL+F` in your browser.`#[doc(hidden)]` will no longer appear in the
âTrait Implementationsâ section.`io::Error` will no longer use the
`ErrorKind::Other` variant. This is to better reflect that these
kinds of errors could be categorised into newer more specific `ErrorKind`
variants, and that they do not represent a user error.`process::Command` on Windows now
behaves as expected. Previously using environment variables with
`rustdoc::``RUSTFLAGS` is no longer set for build scripts. Build scripts
should use `CARGO_ENCODED_RUSTFLAGS` instead. See the
documentation
for more details.
You can now use macros for values in some built-in attributes.
This primarily allows you to call macros within the `#[doc]` attribute. For
example, to include external documentation in your crate, you can now write
the following:

You can now cast between unsized slice types (and types which contain
unsized slices) in `const fn`.

You can now use multiple generic lifetimes with `impl Trait` where the
lifetimes donât explicitly outlive another. In code this means
that you can now have `impl Trait<'a, 'b>` where as before you could
only have `impl Trait<'a, 'b> where 'b: 'a`.

`/lib/rustlib/<target-triple>/target.json` where `rustc --print sysroot`.`wasm` as a `target_family` for WebAssembly platforms.`bpfel-unknown-none`
and `bpfeb-unknown-none`.`-Zmutable-noalias=yes` is enabled by default when using LLVM 12 or above.`panic::panic_any` will now `#[track_caller]`.`OutOfMemory` as a variant of `io::ErrorKind`.`proc_macro::Literal` now implements `FromStr`.`--prune <spec>` option to `cargo-tree` to remove a package from
the dependency graph.`--depth` option to `no-proc-macro` value to `cargo-tree --edges` to hide procedural
macro dependencies.`CARGO_TARGET_TMPDIR` is available.
This variable points to a directory that integration tests and benches
can use as a âscratchpadâ for testing filesystem operations.`?` is no longer permitted in closures for inferred types.`codegen-units = 1`.`â` or `ð¦`. More specifically you can now use any identifier that
matches the UAX #31 âUnicode Identifier and Pattern Syntaxâ standard. This
is the same standard as languages like Python, however Rust uses NFC
normalization which may be different from other languages.`|` (OR) on complete patterns. E.g.
`:pat_param` `:pat` matcher. This is to allow `wasm64-unknown-unknown` target.`android_set_abort_message` on
Android platforms when available.`slice::IterMut<'_, T>` now implements `AsRef<[T]>``IntoIterator`.
Currently calling `.into_iter()` as a method on an array will
return `impl Iterator<Item=&T>`, but this may change in a
future edition to change `Item` to `IntoIterator::into_iter`
directly on arrays will provide `impl Iterator<Item=T>` as expected.`leading_zeros`, and `trailing_zeros` are now available on all
`{f32, f64}::from_str` now parse and print special values
(`NaN`, `-0`) according to IEEE 754.`(Bound<usize>, Bound<usize>)`.`BITS` associated constant to all numeric types.`HEAD` branch is not
âmasterâ. This also includes a switch to the version 3 `unpacked` split-debuginfo.`authors` field is no longer included in `Ipv4::from_str` will now reject octal format IP addresses in addition
to rejecting hexadecimal IP addresses. The octal format can lead
to confusion and potential security vulnerabilities and is no
longer recommended.`lexical-core` crate,
but they have published fixes for semantic versions 0.4 through 0.7. To
update this dependency alone, use `cargo update -p lexical-core`.`RUSTC_FORCE_INCREMENTAL=1` environment variable added in 1.52.1.`std::sys::windows::alloc` implementation.`get_blanket_impls`
This is due to the widespread, and frequently occurring, breakage encountered by
Rust users due to newly enabled incremental verification in 1.52.0. Notably,
Rust users **should** upgrade to 1.52.0 or 1.52.1: the bugs that are detected by
newly added incremental verification are still present in past stable versions,
and are not yet fixed on any channel. These bugs can lead to miscompilation of
Rust binaries.

These problems only affect incremental builds, so release builds with Cargo should not be affected unless the user has explicitly opted into incremental. Debug and check builds are affected.

See 84970 for more details.

`unsafe_op_in_unsafe_fn` lint, which checks whether the unsafe code
in an `unsafe fn` is wrapped in a Added tier 3* support for the following targets.

`riscv64gc-unknown-linux-musl``powerpc-unknown-openbsd``FromIterator`.`cmp::Reverse` now has `Arc<impl Error>` now implements `error::Error`.
The following previously stable APIs are now `const`.

`#[warn(rustdoc::broken_intra_doc_links)]`).
Using the old style is still allowed, and will become a warning in
a future release.```
- [x] Complete
- [ ] Todo
```
`cargo test -- foo bar` will run all tests that match `foo` and `bar`.`struct`s by constant
values in addition to by types and lifetimes. Also known as âconst genericsâ
E.g. you can now write the following. Note: Only values of primitive integers,
`char` types are currently permitted.
`-Csplit-debuginfo` codegen option for macOS platforms.
This option controls whether debug information is split across multiple files
or packed into a single file. `aarch64_be-unknown-linux-gnu`,
`aarch64-unknown-linux-gnu_ilp32`, and `aarch64_be-unknown-linux-gnu_ilp32` targets.`i386-unknown-linux-gnu` and `i486-unknown-linux-gnu` targets.`target-cpu=native` option will now detect individual features of CPUs.`split-debuginfo` profile option to control the -Csplit-debuginfo
codegen option.`resolver` field to `no-std` feature in the final binary. See the
Cargo book documentation for more information on the feature.`--default-theme` flag which sets the default theme to use for
documentation.
Various improvements to intra-doc links:

`slice`.`Vec<T>`.`armv5te-unknown-linux-gnueabi``sparc64-unknown-linux-gnu``thumbv7neon-unknown-linux-gnueabihf``armv7-unknown-linux-gnueabi``x86_64-unknown-linux-gnux32``atomic::spin_loop_hint` has been deprecated. Itâs recommended to use `hint::spin_loop` instead.`[x; N]` array expressions.
This has been technically possible since 1.38.0, as it was unintentionally stabilized.`armv5te-unknown-linux-uclibceabi` target.`aarch64-apple-ios-macabi` target.`x86_64-unknown-freebsd` is now built with the full toolset.`proc_macro::Punct` now implements `PartialEq<char>`.`ops::{Index, IndexMut}` are now implemented for fixed sized arrays of any length.`std::fs::File` type now has a ânicheâ of `-1`.
This value cannot be a valid file descriptor, and now means `Option<File>` takes
up the same amount of space as The following previously stable methods are now `const`.

`IpAddr::is_ipv4``IpAddr::is_ipv6``IpAddr::is_unspecified``IpAddr::is_loopback``IpAddr::is_multicast``Ipv4Addr::octets``Ipv4Addr::is_loopback``Ipv4Addr::is_private``Ipv4Addr::is_link_local``Ipv4Addr::is_multicast``Ipv4Addr::is_broadcast``Ipv4Addr::is_documentation``Ipv4Addr::to_ipv6_compatible``Ipv4Addr::to_ipv6_mapped``Ipv6Addr::segments``Ipv6Addr::is_unspecified``Ipv6Addr::is_loopback``Ipv6Addr::is_multicast``Ipv6Addr::to_ipv4``Layout::size``Layout::align``Layout::from_size_align``pow` for all integer types.`checked_pow` for all integer types.`saturating_pow` for all integer types.`wrapping_pow` for all integer types.`next_power_of_two` for all unsigned integer types.`checked_next_power_of_two` for all unsigned integer types.`[build.rustc-workspace-wrapper]` option.
This option sets a wrapper to execute instead of `cargo:rerun-if-changed` will now, if provided a directory, scan the entire
contents of that directory for changes.`cargo update` command.`compare_and_swap` method on atomics has been deprecated. Itâs
recommended to use the `compare_exchange` and `compare_exchange_weak` methods instead.`TokenStream`s are checked have fixed some cases where you could write
unhygenic `forbid` lint at the same level that it was set is now a hard error.`#[panic_handler]` attribute to provide your own implementation.`struct Foo {};`) now produce a warning.`contains` and indexing.`ToString::to_string` now no longer shrinks the internal buffer in the default implementation.`cargo-package` should now be independently reproducible.`CARGO_PRIMARY_PACKAGE` build-time environment variable. This
variable will be set if the crate being built is one the user selected to build, either
with `-p` or through defaults.`i686-unknown-freebsd` from host tier 2 to target tier 2 support.`initial-exec` Thread
Local Storage model.`system` to the `llvm-libunwind` bootstrap config option.`--color` for configuring terminal color support to bootstrap.`-C link-self-contained=<yes|no>` compiler flag. This tells
`windows-gnu`, `linux-musl`, and `wasi` platforms.)`-C target-feature=+crt-static` on `linux-gnu` targets.
Note: If youâre using cargo you must explicitly pass the `aarch64-unknown-linux-musl`.
The following previously stable methods are now `const fn`âs:

``/// Uses [`std::future`]`` will automatically generate
a link to `std::future`âs documentation. See âLinking to items by
nameâ for more information.`#[doc(alias = "<alias>")]` on items to add search aliases
when searching through `&foo()` will not be
promoted to `#[link_name]` or `#[cold]`
in places where they have no effect.`_mm256_extract_epi8` and `_mm256_extract_epi16` signatures in
`arch::{x86, x86_64}` to return `i32` to match the vendor signatures.`catch_unwind` and will cause an abort.
Note: This behaviour is not guaranteed and is still considered undefined behaviour,
see the `ninja` by default over `make`.
You can continue building with `ninja=false` in
your `bootstrap.toml`.`fewer_names` in `uncached_llvm_type``ensure_sufficient_stack()` non-generic`-C control-flow-guard` codegen option, which enables
Control Flow Guard for Windows platforms, and is ignored on other
platforms.`thumbv4t-none-eabi` target.`RUST_BACKTRACE`âs output is now more compact.`CStr` now implements `Index<RangeFrom<usize>>`.`ops::RangeFull` and `ops::Range` now implement Default.`panic::Location` now implements The following previously stable APIs have now been made const.

`new` method for all `checked_add`,`checked_sub`,`checked_mul`,`checked_neg`, `checked_shl`,
`checked_shr`, `saturating_add`, `saturating_sub`, and `saturating_mul`
methods for all integers.`checked_abs`, `saturating_abs`, `saturating_neg`, and `signum`  for all
signed integers.`is_ascii_alphabetic`, `is_ascii_uppercase`, `is_ascii_lowercase`,
`is_ascii_alphanumeric`, `is_ascii_digit`, `is_ascii_hexdigit`,
`is_ascii_punctuation`, `is_ascii_graphic`, `is_ascii_whitespace`, and
`is_ascii_control` methods for `u8`.`build-dependencies` are now built with opt-level 0 by default.
You can override this by setting the following in your ```
[profile.release.build-override]
opt-level = 3
```
`cargo-help` will now display man pages for commands rather just the
`--help` text.`cargo-metadata` now emits a `workspace.default-members` now respects `workspace.exclude`.`cargo-publish` will now use an alternative registry by default if itâs the
only registry specified in `package.publish`.`{} && false` in tail expressions.`Delimiter::None`.`rustc-docs` component. This allows you to install
and read the documentation for the compiler internal APIs. (Currently only
available for `x.py`. You can read details about this change in the âChanges to `match`, and `loop` expressions can now be used in const functions.`&[T]`) in
const functions.`x.0.0` over `(x.0).0`.`mem::transmute` can now be used in statics and constants. `cdylib` target on Apple iOS and tvOS platforms.`x86_64-unknown-linux-musl`.
Added a number of new environment variables that are now available when compiling your crate.

`CARGO_CRATE_NAME` Providing the name of
the specific binary being compiled and the name of the crate.`CARGO_PKG_LICENSE` The license from the manifest of the package.`CARGO_PKG_LICENSE_FILE` The path to the license file.`abi_blacklist` has been renamed
to `unsupported_abis`. The old name will still continue to work.`{f32, f64}::asinh` now returns the correct values for negative numbers.`libz` (also called `zlib`) on Linux.
The library will need to be installed for `{f64, f32}::to_int_unchecked` methods to continue using the current behaviour, which
may be desirable in rare performance sensitive situations.`mem::Discriminant<T>` now uses `u64`.`target-feature`
flag. E.g. `-C target-feature=+avx2 -C target-feature=+fma` is now
equivalent to `-C target-feature=+avx2,+fma`.`force-unwind-tables` flag. This option allows
rustc to always generate unwind tables regardless of panic strategy.`embed-bitcode` flag. This codegen flag allows rustc
to include LLVM bitcode into generated `rlib`s (this is on by default).`tiny` value to the `code-model` codegen flag.`mipsel-sony-psp` target.`thumbv7a-uwp-windows-msvc` target.`~~outdated information~~` becomes â`{f32, f64}::powi` now returns a slightly different value on Windows.
This is due to changes in LLVMâs intrinsics which `lto` flag is incompatible with the new `embed-bitcode=no`.
This may cause issues if LTO is enabled through `.cargo/config`,
or by setting `CARGO_PROFILE_<name>_LTO` in the environment.
**Syntax-only changes**

These are still rejected semantically, so you will likely receive an error but these changes can be seen and parsed by macros and conditional compilation.

`-C codegen-units` flag in incremental mode.
Additionally when in incremental mode rustc defaults to 256 codegen units.`aarch64-unknown-none` and
`arm64-apple-tvos` and
`x86_64-apple-tvos` targets.`vec![]` to map directly to `Vec::new()`. This allows
`convert::Infallible` now implements `DerefMut` and `IndexMut` returning
a `&mut OsStr`.`String` now implements `From<&mut str>`.`IoSlice` now implements `From<[T; N]>`. Where `N` is at most 32.`proc_macro::LexError` now implements `fmt::Display` and `from_le_bytes`, `to_le_bytes`, `from_be_bytes`, `to_be_bytes`,
`from_ne_bytes`, and `to_ne_bytes` methods are now ```
  mdbook v0.3.2 (/Users/src/rust/mdbook)
âââ ammonia v3.0.0
â   âââ html5ever v0.24.0
â   â   âââ log v0.4.8
â   â   â   âââ cfg-if v0.1.9
â   â   âââ mac v0.1.1
â   â   âââ markup5ever v0.9.0
â   â       âââ log v0.4.8 (*)
â   â       âââ phf v0.7.24
â   â       â   âââ phf_shared v0.7.24
â   â       â       âââ siphasher v0.2.3
â   â       â       âââ unicase v1.4.2
â   â       â           [build-dependencies]
â   â       â           âââ version_check v0.1.5
...
```
`cargo tree -d` (short for `cargo tree --duplicates`).`.a` extension, rather than the previous `.lib`.`-C no_integrated_as` flag from rustc.`file_name` property in JSON output of macro errors now points the actual
source file rather than the previous format of `<NAME macros>`.
`mem::{zeroed, uninitialised}` will now panic when used with types that do
not allow zero initialization such as `NonZeroU8`. This was
previously a warning.`u32` using the `cargo package --list` not working with unpublished dependencies.`&{number}` (e.g. `&1.0`) not having
the type inferred correctly.`#[cfg()]` can now be used on **Syntax only changes**

`type Foo: Ord` syntactically.`item` macro fragments can be interpolated into `rustc -D unused -A unused-variables` denies
everything in the `unused-variables` which
is explicitly allowed. However, passing `rustc -A unused-variables -D unused` denies
everything in the `Arc<[T; N]>`, `Box<[T; N]>`, and `Rc<[T; N]>`, now implement
`TryFrom<Arc<[T]>>`,`TryFrom<Box<[T]>>`, and `TryFrom<Rc<[T]>>`
respectively. `0..=32`.`u32::MAX` or
`f32::NAN` with no imports.`u8::is_ascii` is now `AsMut<str>`.`primitive` module to `HashMap` and `HashSet`.`string::FromUtf8Error` now implements `Clone + Eq`.`[profile]`s in your `CARGO_BIN_EXE_<name>` pointing to a binaryâs
executable path when running integration tests or benchmarks.
`<name>` is the name of your binary as-is e.g. If you wanted the executable
path for a binary named `my-program`you would use `env!("CARGO_BIN_EXE_my-program")`.`const_err` lint were deemed unrelated to const
evaluation, and have been moved to the `assert!` macro is now a hard error. This
has been a warning since 1.36.0.
You can now use the slice pattern syntax with subslices. e.g.

You can now use `#[repr(transparent)]` on univariant `enum`s. Meaning
that you can create an enum that has the exact layout and ABI of the type
it contains.

You can now use outer attribute procedural macros on inline modules.

There are some *syntax-only* changes:

`type`s, and `type Foo: Ord;`).
These are still rejected *semantically*, so you will likely receive an error
but these changes can be seen and parsed by procedural macros and
conditional compilation.

`armv7a-none-eabi`.`riscv64gc-unknown-linux-gnu`.`Option::{expect,unwrap}` and
`Result::{expect, expect_err, unwrap, unwrap_err}` now produce panic messages
pointing to the location where they were called, rather than
`Error::description` has been deprecated, and its use will now produce a
warning. Itâs recommended to use `to_string` instead.`Layout::repeat``impl<T> From<Foo> for Vec<T> {}`.`fn foo(self: Box<Box<Self>>) {}`. Previously only `&Self`,
`&mut Self`, `Arc<Self>`, `Rc<Self>`, and `Box<Self>` were allowed.`pub`) are now syntactically allowed on trait items and
enum variants. These are still rejected semantically, but
can be seen and parsed by procedural macros and conditional compilation.`Box<T>` and use `T*` as the corresponding
type on the C side. Please see the documentation for more information,
including the important caveat about preferring to avoid `'label`s.`i686-unknown-dragonfly` target.`@path` syntax
to rustc. Note that the format differs somewhat from what is
found in other tooling; please see the documentation for
more information.`-L` flag.`NonZero*` numerics now implement `From<NonZero*>` if itâs a smaller integer
width. E.g. `NonZeroU16` now implements `From<NonZeroU8>`.`MaybeUninit<T>` now implements `fmt::Debug`.`cargo-install` will now reinstall the package if it detects that it is out
of date.`[profile.dev.package.image] opt-level = 2` sets the `image` crateâs
optimisation level to `2` for debug builds. You can also use
`[profile.<profile>.build-override]` to override build scripts and
their dependencies.`edition` in documentation code blocks to compile the block
for that edition. E.g. `edition2018` tells rustdoc that the code sample
should be compiled the 2018 edition of Rust.`--theme`, and check the
current theme with `--check-theme`.`#[cfg(doc)]` to compile an item when building documentation.
You can now use tuple `struct`s and tuple `enum` variantâs constructors in
`const` contexts. e.g.

You can now mark `struct`s, `enum`s, and `enum` variants with the `#[non_exhaustive]` attribute to
indicate that there may be variants or fields added in the future.
For example this requires adding a wild-card branch (`_ => {}`) to any match
statements on a non-exhaustive `enum`. (RFC 2008)

You can now use function-like procedural macros in `extern` blocks and in
type positions. e.g. `type Generated = macro!();`

Function-like and attribute procedural macros can now emit
`macro_rules!` items, so you can now have your macros generate macros.

The `meta` pattern matcher in `macro_rules!` now correctly matches the modern
attribute syntax. For example `(#[$m:meta])` now matches `#[attr]`,
`#[attr{tokens}]`, `#[attr[tokens]]`, and `#[attr(tokens)]`.

`--all-features`) passed to a virtual workspace will
now produce an error. Previously these flags were ignored.`dev-dependencies` without including
a `version`.`include!` macro will now warn if it failed to include the
entire file. The `#[inline]` on function prototypes and consts now emits a warning under
`unused_attribute` lint. Using `async` functions and blocks with `async fn`, `async move {}`, and
`async {}` respectively, and you can now call `.await` on async expressions.`cfg_attr`, `allow`, `warn`,
`deny`, ```
fn main() {
    let array: Box<[u8; 4]> = Box::new([1, 2, 3, 4]);
    match array {
        nums
//      ---- `nums` is bound by move.
            if nums.iter().sum::<u8>() == 10
//                 ^------ `.iter()` implicitly takes a reference to `nums`.
        => {
            drop(nums);
//          ----------- Legal as `nums` was bound by move and so we have ownership.
        }
        _ => unreachable!(),
    }
}
```
`sparc64-unknown-openbsd` target.`--show-output` argument to test binaries to print the
output of successful tests.`Vec::new` and `String::new` are now `LinkedList::new` is now a `str::len`, `[T]::len` and `str::as_bytes` are now `abs`, `wrapping_abs`, and `overflowing_abs` numeric functions are
now `--all` flag has been renamed to `try!` macro will now issue a deprecation warning. It is
recommended to use the `asinh(-0.0)` now correctly returns `-0.0`. Previously this
returned `0.0`.`#[global_allocator]` attribute can now be used in submodules.`#[deprecated]` attribute can now be used on macros.`aarch64-uwp-windows-msvc`, `i686-uwp-windows-gnu`,
`i686-uwp-windows-msvc`, `x86_64-uwp-windows-gnu`, and
`x86_64-uwp-windows-msvc` targets.`armv7-unknown-linux-musleabi` targets.`hexagon-unknown-linux-musl` target.`riscv32i-unknown-none-elf` target.`ascii::EscapeDefault` now implements `std::clone::Clone`). This also makes all built-in macros
available in `std::include_bytes!`.`str::Chars` now implements `slice::{concat, connect, join}` now accepts `*mut T` now implement `marker::Unpin`.`Arc<[T]>` and `Rc<[T]>` now implement `FromIterator<T>`.`div_euclid`,
`rem_euclid`) to all numeric primitives. Additionally `checked`,
`overflowing`, and `wrapping` versions are available for all
integer primitives.`thread::AccessError` now implements `iter::{StepBy, Peekable, Take}` now implement `DoubleEndedIterator`.`cargo`.`--features` option multiple times to enable
multiple features.`armv7-unknown-linux-gnueabihf` platform is known to have
issues with certain crates such as libc.`const _: u32 = 5;`.`#[repr(align(X)]` on enums.`-C profile-generate`
and `-C profile-use` flags. For more information on how to use profile
guided optimization, please refer to the rustc book.`rust-lldb` wrapper script should now work again.`default-run="foo"` in `[package]` to specify the
default executable to use for `cargo run`.`..=` syntax for inclusive
ranges instead.`dyn Send + fmt::Debug` is now equivalent to
`dyn fmt::Debug + Send`, where this was previously not the case.`hashbrown::HashMap` implementation.`TryFromSliceError` now implements `From<Infallible>`.`mem::needs_drop` is now available as a const fn.`alloc::Layout::from_size_align_unchecked` is now available as a const fn.`BorrowMut<str>`.`io::Cursor` now implements `NonNull::{dangling, cast}` are now const fns.`Vec`, `Arc`) in `From<&String>`.`dbg!` macro. `Result::{is_err, is_ok}` are now `--offline` flag to run cargo without accessing the network.
You can find further changeâs in Cargoâs 1.36.0 release notes.

There have been numerous additions and fixes to clippy, see Clippyâs 1.36.0 release notes for more details.

`mem::MaybeUninit`, `FnOnce`, `FnMut`, and the `Box<FnOnce>`,
`Box<FnMut>`, and `Box<Fn>` respectively.`armv6-unknown-freebsd-gnueabihf` and
`armv7-unknown-freebsd-gnueabihf` targets.`wasm32-unknown-wasi` target.`cargo:rustc-cdylib-link-arg` at build time to pass custom
linker arguments when building a `redundant_closure` Clippy lint`missing_const_for_fn` Clippy lint`#[deprecated = "reason"]` as a shorthand for
`#[deprecated(note = "reason")]`. This was previously allowed by mistake
but had no effect.`#[attr()]`,`#[attr[]]`, and
`#[attr{}]` procedural macros.`extern crate self as foo;` to import your
crateâs root into the extern prelude.`riscv64imac-unknown-none-elf` and
`riscv64gc-unknown-none-elf`.`-C linker-plugin-lto`. This allows rustc to compile your Rust code
into LLVM bitcode allowing LLVM to perform LTO optimisations across C/C++ FFI
boundaries.`powerpc64-unknown-freebsd`.`HashMap<K, V, S>`âs and
`HashSet<T, S>`âs basic methods. Most notably you no longer require
the `BinaryHeap<T>`âs basic
methods. Most notably you no longer require the `overflowing_neg` and `wrapping_neg` are now `str` is now generic over all types that
implement `SliceIndex<str>`.`str::trim`, `str::trim_matches`, `str::trim_{start, end}`, and
`str::trim_{start, end}_matches` are now `overflowing_pow` are now available for all numeric types. These are
equivalent to methods such as `wrapping_add` for the `Command::before_exec` is being replaced by the unsafe method
`ATOMIC_{BOOL, ISIZE, USIZE}_INIT` is now deprecated as you
can now use `cfg(target_vendor)` attribute. E.g.
`#[cfg(target_vendor="apple")] fn main() { println!("Hello Apple!"); }``0..=255` and
you would no longer be required to have a `_ => unreachable!()` case.`while let`
expressions. You can do this with the same syntax as a `if let _ = 5 {}``#[cfg_attr(all(), must_use, optimize)]``#[repr(packed)]`
attribute. E.g. `#[repr(packed(2))] struct Foo(i16, i32);` is a struct
with an alignment of 2 bytes and a size of 6 bytes.`Pin` as method receivers.`-Clinker-flavor`
command line argument.`x86_64-fortanix-unknown-sgx` target support has been upgraded to
tier 2 support. Visit the platform support page for
information on Rustâs platform support.`thumbv7neon-linux-androideabi` and
`cargo publish --features` or `cargo publish --all-features`.`str::{trim_left, trim_right, trim_left_matches, trim_right_matches}`
are now deprecated in the standard library, and their usage will now produce a warning.
Please use the `str::{trim_start, trim_end, trim_start_matches, trim_end_matches}`
methods instead.`Error::cause` method has been deprecated in favor of `Error::source` which supports
downcasting.`--test-threads=1`.  It also runs the tests in deterministic order`+` operators.`super`, `crate`, will
now always resolve to the item (`PhantomData<T>` types.`literal`
specifier. This will match against a literal of any type.
E.g. `1`, `'A'`, `"Hello World"``PathBuf` now implements `Box<[T]>` now implements The following APIs are now `const` functions and can be used in a
`const` context.

`cargo c` as an alias for `cargo check`.`_mm256_stream_si256`, `_mm256_stream_pd`, `_mm256_stream_ps` have
been changed from `*const` to `*mut` as the previous implementation
was unsound.`powerpc-unknown-netbsd``impl<'a> Reader for BufReader<'a> {}` can now be
`impl Reader for BufReader<'_> {}`. Lifetimes are still required to be defined
in structs.`#[allow(clippy::filter_map)]`.`num::NonZero*` types to their raw equivalents using the
`From` trait. E.g. `&Option<T>` into `Option<&T>` and `&mut Option<T>`
into `Option<&mut T>` using the `time::Duration` by a `package` key in your dependencies.`let r#for = true;``use crate::foo;` refers to `src/lib.rs`.`::`.
Previously, using a external crate in a module without a use statement
required `let json = ::serde_json::from_str(foo);` but can now be written
as `let json = serde_json::from_str(foo);`.`#[used] static FOO: u32 = 1;``#[macro_export]` are now placed into
the root module of the crate. If your macro relies on calling other local
macros, it is recommended to export with the
`#[macro_export(local_inner_macros)]` attribute so users wonât have to import
those macros.`pub(crate)`) in macros
using the `vis` specifier.`#[attr("true")]`, and you can now
write `#[attr(true)]`.`riscv32imc-unknown-none-elf` target.`aarch64-unknown-netbsd` target
`Ipv4Addr::BROADCAST`

`Ipv4Addr::LOCALHOST`

`Ipv4Addr::UNSPECIFIED`

`Ipv6Addr::LOCALHOST`

`Ipv6Addr::UNSPECIFIED`

`Iterator::find_map`

The following methods are replacement methods for `trim_left`, `trim_right`,
`trim_left_matches`, and `trim_right_matches`, which will be deprecated
in 1.33.0:

`str::trim_end_matches`

`str::trim_end`

`str::trim_start_matches`

`str::trim_start`

`--message-format=json`. This is
equivalent to calling `rustdoc --error-format=json`.`--error-format` (specify error format, e.g. `json`) options.`rust-gdbgui` script that invokes `gdbgui` with Rust
debug symbols.`rustfmt` or `clippy` are now
available, e.g. `#[rustfmt::skip]` will skip formatting the next item.`rls-preview` component on the windows-gnu targets has been restored.
The standard libraryâs `str::repeat` function contained an out of bounds write
caused by an integer overflow. This has been fixed by deterministically
panicking when an overflow happens.

Thank you to Scott McMurray for responsibly disclosing this vulnerability to us.

`powerpc64le-unknown-linux-musl` target.`aarch64-unknown-hermit` and `x86_64-unknown-hermit` targets.`cargo-fix` subcommand to automatically move project code from
2015 edition to 2018.`--document-private-items` flag.`--cap-lints` option which demotes all lints above
the specified level to that level. For example `--cap-lints warn`
will demote `101` if there is a panic.`rustup component add clippy-preview`.`str::{slice_unchecked, slice_unchecked_mut}` are now deprecated.
Use `str::get_unchecked(begin..end)` instead.`home_dir` function from
https://crates.io/crates/dirs instead.`struct NewType<T>(T);`) to be represented as
the inner type across Foreign Function Interface (FFI) boundaries.`pure`, `sizeof`, `alignof`, and `offsetof` have been unreserved
and can now be used as identifiers.`GlobalAlloc` trait and `Result<(), E: Debug>` in addition to `lifetime` specifier for `s` and `z` optimisation levels are now stable. These optimisations
prioritise making smaller binary sizes. `--error-format=short` this option will provide a more compressed output of
rust error messages.`macro_export`s.`&mut str`.`From<bool>` for all integer and unsigned number types.`Duration { secs: 1, nanos: 0 }` and will now be printed as `1s`.`Cow<str>`, `From<&Vec<T>>` for `Cow<[T]>`,
`From<Cow<CStr>>` for `CString`, `From<CString>, From<CStr>, From<&CString>`
for `Cow<CStr>`, `From<OsString>, From<OsStr>, From<&OsString>` for
`Cow<OsStr>`, `From<&PathBuf>` for `Cow<Path>`, and `From<Cow<Path>>`
for `Shl` and `Shr` for `Wrapping<u128>`
and `Wrapping<i128>`.`DirEntry::metadata` now uses `fstatat` instead of `lstat` when
possible. This can provide up to a 40% speed increase.`format!`.`src` directory. The `suggestion_applicability` field in rustdoc would execute plugins in the /tmp/rustdoc/plugins directory when running, which enabled executing code as some other user on a given machine. This release fixes that vulnerability; you can read more about this on the blog. The associated CVE is CVE-2018-1000622.

Thank you to Red Hat for responsibly disclosing this vulnerability to us.

`proc` to
be used as an identifier.`Trait` syntax, and should make it clearer when being used in tandem with
`&Trait == &dyn Trait`, `&mut Trait == &mut dyn Trait`, and
`Box<Trait> == Box<dyn Trait>`.`fn foo<#[lifetime_attr] 'a, #[type_attr] T: 'a>() {}``arch::x86` & `arch::x86_64` modules which contain
SIMD intrinsics, a new macro called `is_x86_feature_detected!`, the
`#[target_feature(enable="")]` attribute, and adding `target_feature = ""` to
the `[u8]`, `Rhs` type parameter on `ops::{Shl, ShlAssign, Shr}` now defaults
to `std::str::replace` now has the `Iterator::collect`, and `ToOwned::to_owned` now have
the `categories`, `keywords`,
`readme`, and `repository` fields.`metadata` table.`target` for placing compilation artifacts.`[[bin]]`, and have other binaries in locations
where cargo would infer a binary, Cargo will produce a warning. You can
disable this feature ahead of time by setting any of the following to false:
`autobins`, `autobenches`, `autoexamples`, `autotests`.`CARGO_CACHE_RUSTC_INFO=0` in your environment.`doc.rust-lang.org` are now searchable.`CharExt` or `StrExt` method directly on core will no longer
work. e.g. `::core::prelude::v1::StrExt::is_empty("")` will not
compile, `"".is_empty()` will still compile.`atomic::{AtomicBool, AtomicIsize, AtomicPtr, AtomicUsize}`
will only print the inner type. E.g.
`print!("{:?}", AtomicBool::new(true))` will print `AtomicBool(true)`.`repr(align(N))` is now 2Â²â¹. Previously you
could enter higher numbers but they were not supported by LLVM. Up to 512MB
alignment should cover all use cases.`.description()` method on the `fn main() -> impl Trait` no longer works for non-Termination
trait.
This reverts an accidental stabilization.`NaN > NaN` no longer returns true in const-fn contexts.`for x in 0..=10` is now stable.`'_` lifetime is now stable. The underscore lifetime can be used anywhere a
lifetime can be elided.`fn foo() -> impl Iterator<Item=u8>` or
`fn open(path: impl AsRef<Path>)`.`From<u16> for usize` & `From<{u8, i16}> for isize`.`assert!(format!("{:02x?}", b"Foo\0") == "[46, 6f, 6f, 00]")``Default, Hash` for `str::repeat` being 8x faster in large cases.`ascii::escape_default` is now available in libcore.`Copy, Clone` for `char::{ToLowercase, ToUppercase}`.`-v` is
passed with `--list``AsciiExt` trait in favor of inherent methods.`".e0"` will now no longer parse as `#[repr(align(x))]` attribute is now stable. RFC 1358`use std::{fs::File, io::Read, path::{Path, PathBuf}};````
enum Foo { A, B, C }
fn main() {
    let x = Foo::A;
    match x {
        | Foo::A
        | Foo::B => println!("AB"),
        | Foo::C => println!("C"),
    }
}
```
The following functions can now be used in a constant expression.
eg. `static MINUTE: Duration = Duration::from_secs(60);`

`rust` or `rs` prefixes/suffixes.`net::lookup_host`.`#[simd]`.`extern "sysv64" fn foo () {}``codegen-units=1`.`armv4t-unknown-linux-gnueabi` target.`aarch64-unknown-openbsd` support`str::find::<char>` now uses memchr. This should lead to a 10x
improvement in performance in the majority of cases.`OsStr`âs `time::{SystemTime, Instant}` now implement `AtomicBool``From<{CString, &CStr}>` for `{Arc<CStr>, Rc<CStr>}``From<{OsString, &OsStr}>` for `{Arc<OsStr>, Rc<OsStr>}``From<{PathBuf, &Path}>` for `{Arc<Path>, Rc<Path>}``T: Sized` requirement on `ptr::is_null()``From<RecvError>` for `{TryRecvError, RecvTimeoutError}``f32::{min, max}` to generate more efficient x86 assembly`[u8]::contains` now uses memchr which provides a 3x speed improvement
The following functions can now be used in a constant expression.
eg. `let buffer: [u8; size_of::<usize>()];`, `static COUNTER: AtomicUsize = AtomicUsize::new(1);`

`Formatter::flags` method is now deprecated. The `sign_plus`,
`sign_minus`, `alternate`, and `sign_aware_zero_pad` should be used instead.`column!()` macro is one-based instead of zero-based`fmt::Arguments` can no longer be shared across threads`auto` traits are now permitted in trait objects.`TrapUnreachable` in LLVM which should mitigate the impact of
undefined behavior.`cargo uninstall foo bar` uninstalls `cargo install --version``char::escape_debug` now uses Unicode 10 over 9.`non_snake_case` lint now allows extern no-mangle functions`T op= &T` now works for numeric types. eg. `let mut x = 2; x += &8;``le32-unknown-nacl``armv5te_unknown_linux_gnueabi``Box<Error>` now impls `From<Cow<str>>``std::mem::Discriminant` is now guaranteed to be `Send + Sync``fs::copy` now returns the length of the main stream on NTFS.`Instant += Duration`.`Hasher` for `{&mut Hasher, Box<Hasher>}``SplitWhitespace`.`Option<T>` now impls `Try` This allows for using `Option` types.`examples`
folder that have a `main.rs` file.`[root]` to `libbacktrace` is now available on Apple platforms.`compile_fail` attribute for code fences in doc-comments.
This now lets you specify that a given code example will fail to compile.`4.0` from `2.3``<` is now allowed in all contexts.
Example:
`Stdin`, `Stdout`, and `Stderr` now implement `From<&[T]> where T: Clone`, `From<str>`,
`From<String>`, `From<Box<T>> where T: ?Sized`, and `From<Vec<T>>`.`rustup component add rls-preview``std::os` documentation for Unix, Linux, and Windows now appears on doc.rust-lang.org
Previously only showed `std::os::unix`.`CodeMap` struct which
required the unstable library `libsyntax` to correctly use.`unused_results` lint no longer ignores booleans`wasm32-experimental-emscripten` target.`msp430-none-elf` target.`{HashMap,BTreeMap}::{Keys,Values}`.`fmt::{Display, Debug}` for `Ref`, `RefMut`, `MutexGuard`,
`RwLockReadGuard`, `RwLockWriteGuard``DefaultHasher`.`SyncSender`.`{f32, f64}::{is_sign_negative, is_sign_positive}` handles
NaN.`unimplemented!()` macro.
ie. `unimplemented!("Waiting for 1.21 to be stable")``pub(restricted)` is now supported in the `thread_local!` macro.`{f32, f64}::{min, max}` in Rust instead of using CMath.`ops::{Range, RangeFrom}` is now done in `#[repr(align(N))]` attribute max number is now 2^31 - 1. This was
previously 2^15.`{OsStr, Path}::Display` now avoids allocations where possible`~/.cargo/config` to
`~/.cargo/credentials`.`src/bin`. ie. Having `src/bin/server/main.rs` and
`src/bin/client/main.rs` generates `target/debug/server` and `target/debug/client``--vers`.`--no-fail-fast` flag to cargo to run all benchmarks regardless of
failure.`{f32, f64}::is_sign_{negative, positive}` now
takes the sign of NaN into account where previously didnât.`struct Point(u32, u32); let x = Point { 0: 7, 1: 0 };`.`let x = loop { break 7; };``union`s are now available. RFC 1444 They can only
contain `union Foo { bar: u8, baz: usize }``let foo: fn(u8) -> u8 = |v: u8| { v };``arm-linux-androideabi` to correspond to the `armeabi`
official ABI. If you wish to continue targeting the `armeabi-v7a` ABI
you should use `--target armv7-linux-androideabi`.`aborting due to previous error(s)` instead of
`aborting due to N previous errors` This was previously inaccurate and
would only count certain kinds of errors.`target-feature=+crt-static` option RFC 1721 Which allows
libraries with C Run-time Libraries(CRT) to be statically linked.`FromIterator<Cow<'a, str>>` and
`Extend<Cow<'a, str>>``From<&mut [T]>``Box<[u8]>` now implements `From<Box<str>>``[u8]::reverse` is now 5x faster and `[u16]::reverse` is now
1.5x faster`eprint!` and `eprintln!` macros added to prelude. Same as the `print!`
macros, but for printing to stderr.`println!("cargo:rustc-env=FOO=bar");``cargo bench` subcommand to run benchmarks of all
the members in a given workspace.`libssh2-sys` to 0.2.6`--exclude` option for excluding certain packages when using the
`rust-windbg.cmd` for loading rust `.natvis` files in the
Windows Debugger.`##` is now `MutexGuard<T>` may only be `-Z` flags are now no longer allowed to be used on the stable
compiler. This has been a warning for a year previous to this.`cargo-check` plugin no
longer works. Users should migrate to the built-in `check`
command, which has been available since 1.16.`._` is now a hard error.
Example: `42._` .`extern crate` outside of its module is now a
hard error. This was previously a warning.`use ::self::foo;` is now a hard error. `VCINSTALLDIR` is set incorrectly, `pub(crate) mod utils;`. RFC 1422.`#![windows_subsystem]` attribute conservative exposure of the
`/SUBSYSTEM` linker flag on Windows platforms. RFC 1665.`ty` in macros can accept
types like `Write + Send`, trailing `#[repr(Rust)]` are reordered to minimize padding and produce a smaller
representation in some cases.`--emit mir``Vec::from_iter` being passed `vec::IntoIter` if the
iterator hasnât been advanced the original `AtomicBool::fetch_nand``cargo new --vcs pijul``--bins` and `--tests` flags now you can build all programs
of a certain type, for example `cargo build --bins` will build all
binaries.`--enable-commonmark` flag
Changes to how the `0` flag works in format! Padding zeroes are now
always placed after the sign if it exists and before the digits. With the `#`
flag the zeroes are placed after the prefix and before the digits.

Due to the struct field optimisation, using `transmute` on structs
that have no `repr` attribute or `#[repr(Rust)]` will no longer work. This has
always been undefined behavior, but is now more likely to break in practice.

The refactor of trait object type parsing fixed a bug where `+` was
receiving the wrong priority parsing things like `&for<'a> Tr<'a> + Send` as
`&(for<'a> Tr<'a> + Send)` instead of `(&for<'a> Tr<'a>) + Send`

Overlapping inherent `impl`s are now a hard error

`PartialOrd` and `Ord` must agree on the ordering.

`rustc main.rs -o out --emit=asm,llvm-ir` Now will output
`out.asm` and `out.ll` instead of only one of the filetypes.

 calling a function that returns `Self` will no longer work when
the size of `Self` cannot be statically determined.

rustc now builds with a âpthreadsâ flavour of MinGW for Windows GNU this has caused a few regressions namely:

`where` clause of `impls`. RFC 1647`T: Unsize<U>`. For example,
coercing `&mut [&'a X; N]` to `&mut [&'b X]` requires `'b`. Soundness fix.`--emit dep-info``-C relocation-model` more correctly determine whether the linker
creates a position-independent executable`-C overflow-checks` to directly control whether integer overflow
panics`panic=abort``&str + &str``BTreeSet` can iterate over ranges`FromIterator<&char>``From<Box<str>> for String`,
`From<Box<[T]>> for Vec<T>`,
`From<Box<CStr>> for CString`,
`From<Box<OsStr>> for OsString`,
`From<Box<Path>> for PathBuf`,
`Into<Box<str>> for String`,
`Into<Box<[T]>> for Vec<T>`,
`Into<Box<CStr>> for CString`,
`Into<Box<OsStr>> for OsString`,
`Into<Box<Path>> for PathBuf`,
`Default for Box<str>`,
`Default for Box<CStr>`,
`Default for Box<OsStr>`,
`From<&CStr> for Box<CStr>`,
`From<&OsStr> for Box<OsStr>`,
`From<&Path> for Box<Path>``ffi::FromBytesWithNulError` implements `PartialOrd<A> for [A] where A: Ord``slice::sort``ToString` trait specialization for `Cow<'a, str>` and `From<&[T]> where T: Copy`,
`Box<str>` implements `From<&str>``IpAddr` implements `SocketAddr` implements
`From<(I, u16)> where I: Into<IpAddr>``PartialEq<[A]>` for `VecDeque<A>``rustbook``rustdoc --test` includes file names in test names`aarch64-unknown-linux-fuchsia`, and `x86_64-unknown-linux-fuchsia`.`aarch64-unknown-freebsd``i686-unknown-netbsd``--test-threads=0` produces an error`Display::to_string` panic if an underlying formatting
implementation returns an error. Previously the error was silently
ignored. It is incorrect for `write_fmt` to return an error when writing
to a string.`proc_macro_derives``hr_lifetime_in_assoc_type` future-compatibility lint has been in effect since
April of 2016.`--emit=metadata`, which causes rustc to emit
a `.rmeta` file containing only crate metadata. This can be
used by tools like the Rust Language Service to perform
metadata-only builds.`transmute::<T, U>` where `VecDeque::truncate``VecDeque::resize``String::insert_str``Duration::checked_add``Duration::checked_sub``Duration::checked_div``Duration::checked_mul``str::replacen``SocketAddr::is_ipv4``SocketAddr::is_ipv6``Vec::dedup_by``Vec::dedup_by_key``Result::unwrap_or_default``<*const T>::wrapping_offset``<*mut T>::wrapping_offset``CommandExt::creation_flags``File::set_permissions``String::split_off``--sysroot` argument that, like `armv7-linux-androideabi` target no longer enables NEON
extensions, per Googleâs ABI guide`Stdin.read()` when reading from the console on
Windows`legacy_imports` lint since 1.14, with no known regressions.`path` fragments can now be parsed as type parameter
bounds`#![type_size_limit]` crate attribute, similarly to
the `#![recursion_limit]` attribute`--test` flag works with procedural macro crates`extern "aapcs" fn` ABI`-C no-stack-check` flag is deprecated. It does nothing.`printf` and shell-style
formatting directives and suggests the correct format.`Iterator::nth` no longer has a `Extend<&T>` is specialized for `T: Copy` to improve
performance.`chars().count()` is much faster and so are `chars().last()`
and `char_indices().last()``std::env::args``mpsc::RecvTimeoutError` implements `OUT_DIR`
environment variable at build time via `env!("OUT_DIR")`. They
should instead check the variable at runtime with `std::env`. That the value
was set at build time was a bug, and incorrect when cross-compiling. This
change is known to cause breakage.`cargo install --vers` to take a semver version`build.rustflags` config key`--message-format JSON` when rustc emits non-JSON warnings`--exact` argument that makes the test filter
match exactly, instead of matching only a substring of the test name`--playground-url` flag`--disable-rustbuild` to the
configure script, but they will be deleted soon. Note that the new build
system uses a different on-disk layout that will likely affect any scripts
building Rust.`legacy_directory_ownership`
lint, which is a warning in this release, and will become a hard error in the
future.`Peekable` peeks a `rustup target add`. The new target triples are:
`mips-unknown-linux-gnu``mipsel-unknown-linux-gnu``mips64-unknown-linux-gnuabi64``mips64el-unknown-linux-gnuabi64` `powerpc-unknown-linux-gnu``powerpc64-unknown-linux-gnu``powerpc64le-unknown-linux-gnu``s390x-unknown-linux-gnu` `arm-unknown-linux-musleabi``arm-unknown-linux-musleabihf``armv7-unknown-linux-musleabihf``wasm32-unknown-emscripten` target. This target is known to have major
defects. Please test, report, and fix.`rustup component add rust-docs` to install.`SipHasher` is deprecated. Use `--print target-list``--print target-cpus`, `--print target-features`,
`--print relocation-models`, and `--print code-models` print the available
options to the `-C target-cpu`, `-C target-feature`, `-C code-model` code generation arguments`{integer}` or
`Cell::as_ptr``RefCell::as_ptr``Ipv4Addr::is_unspecified``Ipv6Addr::octets``LinkedList::contains``VecDeque::contains``ExitStatusExt::from_raw`.
Both on Unix and Windows.`Receiver::recv_timeout``RecvTimeoutError``BinaryHeap::peek_mut``PeekMut``iter::Product``iter::Sum``OccupiedEntry::remove_entry``VacantEntry::into_key``panic::catch_unwind` is more optimized`--test-threads` argument to specify the number
of threads used to run tests, and which acts the same as the
`RUST_TEST_THREADS` environment variable`rustup component add rust-src`.
The resulting source code can be used by tools and IDES, located in the
sysroot under `lib/rustlib/src`.`BinaryHeap::append``BTreeMap::append``BTreeMap::split_off``BTreeSet::append``BTreeSet::split_off``f32::to_degrees`
(in libcore - previously stabilized in libstd)`f32::to_radians`
(in libcore - previously stabilized in libstd)`f64::to_degrees`
(in libcore - previously stabilized in libstd)`f64::to_radians`
(in libcore - previously stabilized in libstd)`Iterator::sum``Iterator::product``Cell::get_mut``RefCell::get_mut``harness = false` on `links` contains a â.â`-vv` prints warnings for all crates.`package.metadata` keys.
This provides room for expansion by arbitrary tools.`cargo doc --open` on Windows`Ipv4Addr` is 10x faster`#[repr(..)]` attribute.`#[derive(RustcEncodable)]` in the presence of other `encode` methods.`os::windows::fs::OpenOptionsExt::access_mode``os::windows::fs::OpenOptionsExt::share_mode``os::windows::fs::OpenOptionsExt::custom_flags``os::windows::fs::OpenOptionsExt::attributes``os::windows::fs::OpenOptionsExt::security_qos_flags``os::unix::fs::OpenOptionsExt::custom_flags``sync::Weak::new``Default for sync::Weak``panic::set_hook``panic::take_hook``panic::PanicInfo``panic::PanicInfo::payload``panic::PanicInfo::location``panic::Location::file``panic::Location::line``ffi::CStr::from_bytes_with_nul``fs::Metadata::modified``fs::Metadata::accessed``fs::Metadata::created``sync::atomic::Atomic{Usize,Isize,Bool,Ptr}::compare_exchange``sync::atomic::Atomic{Usize,Isize,Bool,Ptr}::compare_exchange_weak``collections::{btree,hash}_map::{Occupied,Vacant,}Entry::key``os::unix::net::{UnixStream, UnixListener, UnixDatagram, SocketAddr}``SocketAddr::is_unnamed``SocketAddr::as_pathname``UnixStream::connect``UnixStream::pair``UnixStream::try_clone``UnixStream::local_addr``UnixStream::peer_addr``UnixStream::set_read_timeout``UnixStream::set_write_timeout``UnixStream::read_timeout``UnixStream::write_timeout``UnixStream::set_nonblocking``UnixStream::take_error``UnixStream::shutdown``UnixListener::bind``UnixListener::accept``UnixListener::try_clone``UnixListener::local_addr``UnixListener::set_nonblocking``UnixListener::take_error``UnixListener::incoming``UnixListener``UnixDatagram::bind``UnixDatagram::unbound``UnixDatagram::pair``UnixDatagram::connect``UnixDatagram::try_clone``UnixDatagram::local_addr``UnixDatagram::peer_addr``UnixDatagram::recv_from``UnixDatagram::recv``UnixDatagram::send_to``UnixDatagram::send``UnixDatagram::set_read_timeout``UnixDatagram::set_write_timeout``UnixDatagram::read_timeout``UnixDatagram::write_timeout``UnixDatagram::set_nonblocking``UnixDatagram::take_error``UnixDatagram::shutdown``UnixDatagram``{BTree,Hash}Map::values_mut``<[_]>::binary_search_by_key``abs_sub` method of floats is deprecated.
The semantics of this minor method are subtle and probably not what
most people want.`getrandom` they
will fall back to `/dev/urandom` temporarily to avoid blocking
during early boot.`binary_heap::IntoIter`.`std::num::Wrapping`.`&CStr`, `Into<Vec<T>>` for `VecDeque<T>`.`fmt::Error`, `Condvar`,
`Mutex`, `RwLock`.`profile.*.panic` option.
This controls the runtime behavior of the `CARGO_MANIFEST_LINKS` environment
variable that corresponds to the `net.retry` value in `--force` flag to `flock` on NFS mounts.`cargo test --doc`.`cargo --explain`.`-q` is passed.`cargo doc --lib` and `--bin`.`find` implementation for `Chain` iterators is 2x faster.`#[derive(Copy, Clone)]` to avoid bloat.`&mut self` to `&mut mut self`.`env::current_exe`, which canât be relied
on.`rust-gdb` and `libc::abort` instead of
generating an illegal instruction.`target_env` for Linux ARM targets is just `gnu`, not
`gnueabihf`, `gnueabi`, etc.`Duration::new`.`String::truncate` to panic less.`:block` to the follow set for `:ty` and `:path`.
Affects how macros are parsed.`#[allow(deprecated)]`. RFC 1270.`PATTERN_WHITE_SPACE` category to be whitespace.`std::sync::Once` is poisoned if its initialization function
fails.`cell::Ref` and `cell::RefMut` can contain unsized types.`BufReader` and `BufWriter` was
reduced to 8K, from 64K. This is in line with the buffer size
used by other languages.`Instant`, `SystemTime` and `+=` and `-=`.
`*=` and `/=`.`Skip` is a `From<[u8; 4]>` is implemented for `BufRead`.`CARGO_PKG_AUTHORS` environment variable.`to_owned`.`Command::output` no longer creates extra
threads.`#[derive(PartialEq)]` and `#[derive(PartialOrd)]` emit less code
for C-like enums.`--quiet` flag to a test runner will produce
much-abbreviated output.`mips-unknown-linux-musl`, `mipsel-unknown-linux-musl`, and
`i586-pc-windows-msvc` targets.`AddAssign`, `SubAssign`,
`MulAssign`, `DivAssign`, `RemAssign`, `BitAndAssign`,
`BitOrAssign`, `BitXorAssign`, `ShlAssign`, or `ShrAssign`
traits. RFC 953.`struct Foo { }`, in
addition to the non-braced form, `struct Foo;`. RFC 218.`str::encode_utf16` (renamed from `utf16_units`)`str::EncodeUtf16` (renamed from `Utf16Units`)`Ref::map``RefMut::map``ptr::drop_in_place``time::Instant``time::SystemTime``Instant::duration_since` (renamed from `duration_from_earlier`)`Instant::elapsed``SystemTime::now``SystemTime::duration_since` (renamed from `SystemTime::elapsed``Add`/`Sub` impls for `Time` and `SystemTimeError``SystemTimeError::duration``UNIX_EPOCH``raw` modules, which contain a number of
redefined C types are deprecated, including `os::raw::unix`,
`os::raw::macos`, and `os::raw::linux`. These modules defined types
such as `ino_t` and `dev_t`. The inconsistency of these definitions
across platforms was making it difficult to implement `libc`
crate. RFC 1415.`MetadataExt` traits, including
`os::unix::fs::MetadataExt`, which expose values such as inode
numbers no longer return platform-specific types, but
instead return widened integers. RFC 1415.`btree_set::{IntoIter, Iter, Range}` are covariant.`sync::mpsc` implement `--print targets` flag prints a list of supported targets.`--print cfg` flag prints the `configure --rustbuild`.`x86_64-rumprun-netbsd`. These can be installed with
tools such as multirust.`verbose` and `color`, respectively, go in the
`[term]` section of `CARGO_BUILD_JOBS`. Environment variables take
precedence over config files.`CARGO_TARGET_ROOT`, `RUSTC`, and
`RUSTDOC` take precedence over the `build.target-dir`,
`build.rustc`, and `build.rustdoc` configuration values.`Variant(..)` syntax. This has been a warning since 1.6.`Command::spawn` and its equivalents return an error if any of
its command-line arguments contain interior `NUL`s.`staticlib` library
type instead of `foo.dll.lib`
instead of `foo.lib`.`Path`
`Path::strip_prefix` (renamed from relative_from)`path::StripPrefixError` (new error type returned from strip_prefix)`<[T]>::``clone_from_slice`, which now requires the two slices to
be the same length`sort_by_key``i32::checked_rem`, `i32::checked_neg`, `i32::checked_shl`, `i32::checked_shr``i32::saturating_mul``i32::overflowing_add`, `i32::overflowing_sub`, `i32::overflowing_mul`, `i32::overflowing_div``i32::overflowing_rem`, `i32::overflowing_neg`, `i32::overflowing_shl`, `i32::overflowing_shr``u32::checked_rem`, `u32::checked_neg`, `u32::checked_shl`, `u32::saturating_mul``u32::overflowing_add`, `u32::overflowing_sub`, `u32::overflowing_mul`, `u32::overflowing_div``u32::overflowing_rem`, `u32::overflowing_neg`, `u32::overflowing_shl`, `u32::overflowing_shr``ffi::IntoStringError``CString::into_string``CString::into_bytes``CString::into_bytes_with_nul``From<CString> for Vec<u8>``IntoStringError`
`IntoStringError::into_cstring``IntoStringError::utf8_error``Error for IntoStringError``LineWriter` (and thus `io::stdout`) was
improved by using `memchr` to search for newlines.`Iter`, `IntoIter`, and `Range` are
covariant over their contained type.`LinkedList` and its iterators, `str::replace` now accepts a `Pattern`, like other string
searching methods.`Any` is implemented for unsized types.`--enable-new-dtags` to GNU ld. This makes
any RPATH entries (emitted with `-C rpath`) `LD_LIBRARY_PATH`.`rerun-if-changed` build script directive no longer causes the
build script to incorrectly run twice in certain scenarios.`private_in_public` lint.`".".parse::<f32>()` returns `Err`, not `Ok(0.0)`.`Read::read_exact`,
`ErrorKind::UnexpectedEof` (renamed from `UnexpectedEOF`),
`fs::DirBuilder`, `fs::DirBuilder::new`,
`fs::DirBuilder::recursive`, `fs::DirBuilder::create`,
`os::unix::fs::DirBuilderExt`,
`os::unix::fs::DirBuilderExt::mode`, `vec::Drain`,
`vec::Vec::drain`, `string::String::drain`,
`vec_deque::Drain`, `vec_deque::VecDeque::drain`,
`collections::hash_map::Drain`,
`collections::hash_map::HashMap::drain`,
`collections::hash_set::Drain`,
`collections::hash_set::HashSet::drain`,
`collections::binary_heap::Drain`,
`collections::binary_heap::BinaryHeap::drain`,
`Vec::extend_from_slice` (renamed from `push_all`),
`Mutex::get_mut`, `Mutex::into_inner`, `RwLock::get_mut`,
`RwLock::into_inner`,
`Iterator::min_by_key` (renamed from `min_by`),
`Iterator::max_by_key` (renamed from `max_by`).`Condvar::wait_timeout_ms`, `thread::sleep_ms`, and
`thread::park_timeout_ms`.`isize` and `usize`
because the implementations would be platform-specific. `From<&Path>` and `From<PathBuf>` are implemented for `From<T>` is implemented for `Arc<T>`.`&PathBuf` and `&Path`.`BinaryHeap` was refactored for modest performance
improvements.`$CARGO_HOME/bin` for subcommands by default.`unreachable_code` lint warns when a function callâs argument
diverges.`RUST_PATH`
environment variable when locating crates. This was a pre-cargo
feature for integrating with the package manager that was
accidentally never removed.`Foo(..)`) can no longer be used to match
unit structs. This is a warning now, but will become an error in
future releases. Patterns that share the same name as a const are
now an error.`BinaryHeap::from`, `BinaryHeap::into_sorted_vec`,
`BinaryHeap::into_vec`, `Condvar::wait_timeout`,
`FileTypeExt::is_block_device`, `FileTypeExt::is_char_device`,
`FileTypeExt::is_fifo`, `FileTypeExt::is_socket`,
`FileTypeExt`, `Formatter::alternate`, `Formatter::fill`,
`Formatter::precision`, `Formatter::sign_aware_zero_pad`,
`Formatter::sign_minus`, `Formatter::sign_plus`,
`Formatter::width`, `Iterator::cmp`, `Iterator::eq`,
`Iterator::ge`, `Iterator::gt`, `Iterator::le`,
`Iterator::lt`, `Iterator::ne`, `Iterator::partial_cmp`,
`Path::canonicalize`, `Path::exists`, `Path::is_dir`,
`Path::is_file`, `Path::metadata`, `Path::read_dir`,
`Path::read_link`, `Path::symlink_metadata`,
`Utf8Error::valid_up_to`, `Vec::resize`,
`VecDeque::as_mut_slices`, `VecDeque::as_slices`,
`VecDeque::insert`, `VecDeque::shrink_to_fit`,
`VecDeque::swap_remove_back`, `VecDeque::swap_remove_front`,
`slice::split_first_mut`, `slice::split_first`,
`slice::split_last_mut`, `slice::split_last`,
`fs::canonicalize`,
`str::MatchIndices`, `str::RMatchIndices`,
`str::match_indices`, `str::rmatch_indices`,
`str::slice_mut_unchecked`, `string::ParseError`.`~/.cargo/bin` with the `cargo-foo` is found in `$PATH` it can be
invoked as `cargo foo`.`AsRef` and `AsMut` were added to `no_default_libraries`
setting that controls whether `-nodefaultlibs` is passed to the
linker, and in turn the `is_like_windows` setting no longer affects
the `#[derive(Show)]`, long-deprecated, has been removed.`#[repr]` attributes can only appear
in valid locations.`#[no_debug]` and
`#[omit_gdb_pretty_printer_section]` are feature gated.`improper_ctypes` lint no longer warns about using `SockAddrV4` and `SockAddrV6`.`fs::Metadata` implements `parse` method accepts a leading â+â when parsing
integers.`clone_from` implementations for `extern "Rust"`, `unsafe extern "Rust"` and
`unsafe extern "C"` function types now implement `fmt::Pointer`, and
`VecDeque`
with zero-sized types was resolved.`str::lines` and `BufRead::lines` iterators treat `\r\n` as
line breaks in addition to `\n`.`str::parse` no longer introduces avoidable rounding error when
parsing floating point numbers. Together with earlier changes to
float formatting/output, âround tripsâ like f.to_string().parse()
now preserve the value of f exactly. Additionally, leading plus
signs are now accepted.`use foo::{bar as kitten, baz as puppy}`.`pub extern crate`, which does not behave as expected, issues a
warning until a better solution is found.`<Box<str>>::into_string`,
`Arc::downgrade`, `Arc::get_mut`, `Arc::make_mut`,
`Arc::try_unwrap`, `Box::from_raw`, `Box::into_raw`, `CStr::to_str`,
`CStr::to_string_lossy`, `CString::from_raw`, `CString::into_raw`,
`IntoRawFd::into_raw_fd`, `IntoRawFd`,
`IntoRawHandle::into_raw_handle`, `IntoRawHandle`,
`IntoRawSocket::into_raw_socket`, `IntoRawSocket`, `Rc::downgrade`,
`Rc::get_mut`, `Rc::make_mut`, `Rc::try_unwrap`, `Result::expect`,
`String::into_boxed_str`, `TcpStream::read_timeout`,
`TcpStream::set_read_timeout`, `TcpStream::set_write_timeout`,
`TcpStream::write_timeout`, `UdpSocket::read_timeout`,
`UdpSocket::set_read_timeout`, `UdpSocket::set_write_timeout`,
`UdpSocket::write_timeout`, `Vec::append`, `Vec::split_off`,
`VecDeque::append`, `VecDeque::retain`, `VecDeque::split_off`,
`rc::Weak::upgrade`, `rc::Weak`, `slice::Iter::as_slice`,
`slice::IterMut::into_slice`, `str::CharIndices::as_str`,
`str::Chars::as_str`, `str::split_at_mut`, `str::split_at`,
`sync::Weak::upgrade`, `sync::Weak`, `thread::park_timeout`,
`thread::sleep`.`BTreeMap::with_b`,
`BTreeSet::with_b`, `Option::as_mut_slice`, `Option::as_slice`,
`Result::as_mut_slice`, `Result::as_slice`, `f32::from_str_radix`,
`f64::from_str_radix`.`std::io::copy` allows `Windows`, `Chunks`, and `ChunksMut` iterators over slices all
override `nth` and `last` with an `[T; 32]`.`Extend<String>` and `FromIterator<String` are both implemented for
`Result`.`Borrow` and `BorrowMut` are implemented for fixed-size
arrays.`extern fn`s with the âRustâ and âCâ ABIs implement common
traits including `T: std::fmt::Write` also implements
`std::fmt::Write`.`VecDeque::push_back` and other
capacity-altering methods that caused panics for zero-sized types
was fixed.`&'a Box<Trait>` (or `&'a Rc<Trait>`, etc) will change from
being interpreted as `&'a Box<Trait+'a>` to `&'a Box<Trait+'static>`.`#[prelude_import]` attribute, an internal implementation
detail, was accidentally stabilized previously. It has been put
behind the `prelude_import` feature gate. This change is
believed to break no existing code.`size_of_val` and `align_of_val` is
more sane for dynamically sized types. Code that relied on
the previous behavior is thought to be broken.`dropck` rules, which checks that destructors canât access
destroyed values, have been updated to match the
RFC. This fixes some soundness holes, and as such will
cause some previously-compiling code to no longer build.`ref` bindings choose between `std::time` module, which presently contains only `ToOwned`. The two of
these allow C strings to be borrowed and cloned in generic code.`AtomicPtr` implements `is`, `downcast`,
`downcast_ref` and `downcast_mut` methods, similarly to the
`rfind`, `split`. `starts_with` and
`ends_with` are also faster.`hash_slice`, which
is overridden and optimized by the implementations for scalars.`write_*`
methods for primitive types, for efficiency.`std::io::Error`, gained a set of
methods for accessing the âinner errorâ, if any: `get_ref`,
`get_mut`, `into_inner`. As well, the implementation
of `std::error::Error::cause` also delegates to the inner
error.`process::Child` gained the `id` method, which returns a
`connect` method on slices is deprecated, replaced by the new
`join` method (note that both of these are on the `SliceConcatExt` trait, but through the magic of the prelude are
available to stable code anyway).`Div` operator is implemented for `Wrapping` types.`Send`.`read_to_end` implementations for `--explain` flag.`unused_mut`, `unconditional_recursion`,
`negate_unsigned` lints are
more strict.`-Z no-landing-pads`), `-C codegen-units=N` flag to
`to_uppercase` and `to_lowercase` methods on `mem::align_of` now specifies the `mem::min_align_of` is deprecated. This is not known to
break real code.`#[packed]` attribute is no longer silently accepted by the
compiler. This attribute did nothing and code that
mentioned it likely did not work as intended.`associated_type_defaults` feature gate. In 1.1 associated type
defaults `ref mut` now correctly invoke `EnumSet`, `VecMap`,
`iter::once` function returns an iterator that yields a single
element, and `iter::empty` returns an iterator that yields no
elements.`rmatches` methods on `RefCell` both implement `wrapping_div`, `wrapping_rem`,
`wrapping_shl`, `wrapping_shr`. These are in
addition to the existing `wrapping_sub`, and
`wrapping_mul` methods, and alternatives to the `{:#?}` formatting specifier displays the alternate,
pretty-printed form of the `fmt::Formatter` implements `fmt`-specific trait
for writing data to formatted strings, similar to `io::Write`.`debug_struct`,
`debug_tuple`, `debug_list`, `debug_set`, `debug_map`. These
are used by code generators to emit implementations of `PoisonError`
type, returned by failing lock operations, exposes `is_poisoned` method of `FromRawFd` trait is implemented for `Stdio`, and
`ChildStdin`, `ChildStdout`, `ChildStderr`.
On Windows the `FromRawHandle` trait is implemented for `AsRawHandle` for `InvalidData`, which indicates
malformed input.`std::fs` module has been expanded to expand the set of
functionality exposed:
`DirEntry` now supports optimizations like `file_type` and `symlink_metadata` function has been added.`str::split_whitespace` method splits a string on unicode
whitespace boundaries.`TcpStream`,
`TcpListener`, and `UpdSocket`. Further implementations for
`std::process` will be stabilized later.`std::os::unix::symlink` creates symlinks. On
Windows, symlinks can be created with
`std::os::windows::symlink_dir` and
`std::os::windows::symlink_file`.`mpsc::Receiver` type can now be converted into an iterator with
`into_iter` on the `From<u32>`
implementation of the `RangeFull` creates output that is
more consistent with other implementations.`Sync + Send`.`Cloned` iterator, which was accidentally left unstable for
1.0 has been stabilized.`Incoming` iterator, which iterates over incoming TCP
connections, and which was accidentally unnamable in 1.0, is now
properly exported.`sift_up` or `sift_down` panic.`split_off` method of `drop_with_repr_extern` lint warns about mixing `#[stable]`. It is
no longer possible to use unstable features with a stable build of
the compiler.`0b1234` is
now lexed as `0b1` and `234`.`PhantomFn` and `MarkerTrait` lang items, which have been
removed.`extern crate "foo" as bar` syntax has been replaced with `extern crate foo as bar`, and Cargo now automatically translates â-â in `MyType::default()`.`SliceExt`.`IteratorExt` to be merged into the traits they extended.`unsafe_destructor` feature is now deprecated in favor of the
new `thread_local` module has been renamed to `Into` have been
centralized in the `std::convert` module.`FromError` trait was removed in favor of `std::thread::sleep_ms`.`splitn` function now takes an `n` parameter that represents the
number of items yielded by the returned iterator instead of the
number of âsplitsâ.`CLOEXEC` by default.`&char`, `&str`, `FnMut(char) -> bool` and
some others.`String::from_str` has been deprecated in favor of the `String::from`.`words` method on `split_whitespace`, to avoid answering the tricky question, âwhat is
a word?â`.`,
adjusting the tradeoffs in favor of the most common usage.`Index` and `hash_map["string"]` to work.
~1300 changes, numerous bugfixes

Highlights

`io` module remains temporarily
at `std::old_io`.`#![feature(...)]`
attribute. The impact of this change is described on the
forum. RFC.
Language

`for` loops now operate on the `.iter()`, etc. to iterate
over collections. There are some new subtleties to remember
though regarding what sort of iterators various types yield, in
particular that `for foo in bar { }` yields values from a move
iterator, destroying the original collection. RFC.`Box<Trait+'static>` when you donât care about
storing references. RFC.`#[unsafe_destructor]` is now
required. Read the gorgeous RFC for details.`Deref<U>` now automatically
coerce to references to the dereferenced type `&T where T: Deref<U>` automatically coerces to `&U`. This
should eliminate many unsightly uses of `&*`, as when converting
from references to vectors into references to
slices. RFC.`|&:|`, `|&mut:|`,
`|:|`) is obsolete and closure kind is inferred from context.
Libraries

`Show` and `format!("{:?}", something_to_debug)` is
now written `#[derive(Debug)]`.`std::ff::{OsString, OsStr}`, provide strings in platform-specific encodings for easier
interop with system APIs. RFC.`boxed::into_raw` and Tooling

`/usr/local/lib/rustlib/uninstall.sh`.`#[rustc_on_unimplemented]` attribute, requiring the
âon_unimplementedâ feature, lets rustc display custom error
messages when a trait is expected to be implemented for a type
but is not.
Misc

~2400 changes, numerous bugfixes

`int` and `uint`, for pointer-sized
integers. Guidelines will be rolled out during the alpha cycle.`i..j`, `i..`, and
`..j` that produce range types and which, when combined with the
`[i..j]`.`[T; N]`.`unsafe impl` if a type confirms to the requirements for them
even though the internals do not (e.g. structs containing unsafe
pointers like `vec![1i32, 2, 3].len()` work as expected.`#[derive(...)]`
not `#[deriving(...)]` for consistency with other naming
conventions.`mod`, as in use `foo::{self, bar}``fail!` macro has been renamed to `OsRng` prefers the new, more reliable `{:?}`
specifier and is intended for use by all types, for uses
such as `println!` debugging. The new `{}` specifier, and is intended
for full-fidelity conversions of things that can logically be
represented as strings.`Option<Vec<T>>` and `Option<String>` take up no more
space than the inner types themselves.
~1900 changes, numerous bugfixes

`[0..4]`) has been introduced behind
the âslicing_syntaxâ feature gate, and can be overloaded with
the `Slice` or `SliceMut` traits.`[a, b, c..]`), for
consistency with other uses of `0..3` to `0...4` to be consistent with the exclusive range
syntax for slicing.`[a.., b, c]`) has been put behind the âadvanced_slice_patternsâ feature
gate and may be removed in the future.`value.0` syntax, currently behind the `tuple_indexing`
feature gate.`#[crate_id]` attribute is no longer supported; versioning
is handled by the package manager.`extern crate bar = foo`.`use foo as bar` instead
of `use bar = foo`.`move` has been added as a keyword, for indicating closures
that capture by value.`Share` trait is now called `<Sized? T>`. Most
types are `[T]`) and trait types.`|| -> !` or `proc() -> !`.`Gc<T>` which was once
denoted by the `std::time::Duration` type has been added for use in I/O
methods that rely on timers, as well as in the âtimeâ crateâs
`Timespec` arithmetic.`collections::btree` has been rewritten to have a more
idiomatic and efficient design.`--crate-name` flag can specify the name of the crate
being compiled, like `#[crate_name]`.`-C metadata` specifies additional metadata to hash into
symbol names, and `-C extra-filename` specifies additional
information to put into the output filename, for use by the
package manager for versioning.
~1700 changes, numerous bugfixes

`<'a>|A, B|: 'b + K -> T``priv` keyword has been removed from the language.`use foo, bar, baz;` syntax has been removed from the language.`rev()` on
their forward-iteration counterparts.`--cfg ndebug` is passed to the compiler.`[breaking-change]` to allow for easy
discovery of breaking changes.`pub use` has been greatly improved.
~1500 changes, numerous bugfixes

`~[T]`) have been removed from the
language.`~str`) have been removed from the
language.`@str` has been removed from the language.`@[T]` has been removed from the language.`@self` has been removed from the language.`@Trait` has been removed from the language.`~` allocations which contain `#[deriving]` with raw pointers`log_syntax!` are now behind feature gates.`do` keyword has been removed, it is now a reserved keyword.`extern mod` is now `static mut` locations has been tweaked.`~Trait` and `Unsafe` type was introduced for interior mutability. It is now
considered undefined to transmute from `#[foo];` to `#![foo]`.`Pod` was renamed to `libextra` library has been removed. It has now been decomposed into
component libraries with smaller and more focused nuggets of
functionality. The full list of libraries can be found on the
documentation index page.`std::condition` has been removed. All I/O errors are now propagated
through the `vec` module has been renamed to `std::io` now has more public re-exports. Types such as `BufferedReader`
are now found at `std::io::BufferedReader` instead of
`std::io::buffered::BufferedReader`.`print` and `println` are no longer in the prelude, the `Weak` pointer for breaking cycles, and it no longer
attempts to statically prevent cycles.`slice::last()`) now return `fmt::Default` has been renamed to `fmt::Show`, and it now has a new
deriving mode: `#[deriving(Show)]`.`ToStr` is now implemented for all types implementing `&self` instead of `invert()` method on iterators has been renamed to `std::num` has seen a reduction in the genericity of its traits,
consolidating functionality into a few core traits.`eof()` has been removed from the `Reader` trait. Specific types may
still implement the function.`assert_approx_eq!` has been removed`e` and `E` formatting specifiers for floats have been added to
print them in exponential notation.`Times` trait has been removed`std::kinds::marker` now`hash` has been rewritten, `IterBytes` has been removed, and
`#[deriving(Hash)]` is now possible.`SharedChan` has been removed, `Sender` is now cloneable.`Chan` and `Port` were renamed to `Receiver`.`Chan::new` is now `channel()`.`select!` macro is now provided for selecting over `hashmap` and `trie` have been moved to `libcollections``run` has been rolled into `io::process``rand` has moved to `librand`.`to_{lower,upper}case` has been implemented for `liblog`.`libnative`. If `libgreen` is desired,
it can be booted manually. The runtime guide has more information and
examples.`extra::sync` module has been updated to modern rust (and moved
to the `sync` library), tweaking and improving various interfaces while
dropping redundant functionality.`Barrier` type has been added to the `base64` module has seen some improvement. It treats
newlines better, has non-string error values, and has seen general
cleanup.`fourcc!` macro was introduced`hexfloat!` macro was implemented for specifying floats via a
hexadecimal literal.`rustpkg` has been deprecated and removed from the main repository. Its
replacement, `--emit` flag.`-C` flag.
~1800 changes, numerous bugfixes

`float` type has been removed. Use `#[feature(foo)]` attribute.`#[feature(managed_boxes)]`) in preparation for future removal. Use the
standard libraryâs `Gc` or `@mut` has been removed. Use `std::cell::{Cell, RefCell}` instead.`continue` instead of
`r"foo"` syntax or with matched
hash delimiters, as in `r###"foo"###`.`~fn` is now written `proc (args) -> retval { ... }` and may only be
called once.`&fn` type is now written `|args| -> ret` to match the literal form.`@fn`s have been removed.`#[link(...)]` attribute has been replaced with
`#[crate_id = "name#vers"]`.`fmt!` string formatting macro has been removed.`printf!` and `printfln!` (old-style formatting) removed in favor of
`mut` works in patterns now, as in `let (mut x, y) = (1, 2);`.`extern mod foo (name = "bar")` syntax has been removed. Use
`extern mod foo = "bar"` instead.`#[feature(asm)]`).`#[repr(int)]` for integer-sized, and
`#[repr(C)]` to match C enums.`0o7777`.`#[fixed_stack_segment]` and `#[rust_stack]` attributes have been
removed as Rust no longer uses segmented stacks.`#[feature(non_ascii_idents)]`).`.._`.`extern "win64"`.`type_overflow` lint (default: warn) checks literals for overflow.`unsafe_block` lint (default: allow) checks for usage of `attribute_usage` lint (default: warn) warns about unknown
attributes.`unknown_features` lint (default: warn) warns about unknown
feature gates.`#[link_args]` is behind the `link_args` feature gate.`#[link(name = "foo")]``#[link(name = "foo", kind = "static")]`).`#[link(name = "foo", kind = "framework")]`).`return` keyword may be used in closures.`option` and `result` APIâs have been overhauled to make them
simpler, more consistent, and more composable.`io::util` contains a number of useful implementations of
`Writer`, including `NullReader`, `NullWriter`,
`ZeroReader`, `TeeReader`.`extra::rc` moved into std.`gc` module will replace `Either` type has been removed.`format!("{}", myfoo)`.`rust_begin_unwind` function, useful for inserting breakpoints
on failure in gdb, is now named `rust_fail`.`each_key` and `each_value` methods on `keys` and `values` iterators.`sys` module to the `mem` module.`str::from_utf8` has been changed to cast instead of allocate.`ImmutableEqVector` trait, which is in the prelude.`get_opt` method, which returns `~Any` can be passed to the `task::try`.`_iter`
suffix now.`cell::Cell` and `cell::RefCell` can be used to introduce mutability
roots (mutable fields, etc.). Use instead of e.g. `util::ignore` renamed to `prelude::drop`.`sort` and `sort_by` methods via the `MutableVector`
trait.`vec::raw` has seen a lot of cleanup and API changes.`comm` module has been rewritten to be much faster, have a
simpler, more consistent API, and to work for both native and green
threading.`flatpipes` module had bitrotted and was removed.`c_vec` has been modernized.`rusti` commands have been removed, due to lack of
maintenance.`--dep-info` flag for communicating dependencies to
build tools.
~2200 changes, numerous bugfixes

`debug!` macro if it is passed
`std::c_str` module
provides new mechanisms for converting to C strings.`extern "C" fn` instead of `*u8â.`externfn!` macro can be used to declare both a foreign function and
a `&'static T`) is no longer allowed in `@T`.`\0` is now accepted as an escape in chars and strings.`yield` is a reserved keyword.`typeof` is a reserved keyword.`extern mod foo = "url";`.`enum E { V = 0u }``static foo: [u8, .. 100]: [0, .. 100];`.`static foo: Foo = Foo { a: 5, .. bar };`.`cfg!` can be used to conditionally execute code based on the crate
configuration, similarly to `#[cfg(...)]`.`unnecessary_qualification` lint detects unneeded module
prefixes (default: allow).`std::unstable::simd`.`std::rt::io`.`#[link_section = "..."]`.`proto!` syntax extension for defining bounded message protocols
was removed.`unreachable!` can be used to indicate unreachable code, and fails
if executed.`rt::io`, based on the new
runtime.`range` function was added to the prelude, replacing
`uint::range` and friends.`range_rev` no longer exists. Since range is an iterator it can be
reversed with `range(lo, hi).invert()`.`chain` method on option renamed to `and_then`; `unwrap_or_default`
renamed to `unwrap_or`.`iterator` module was renamed to `iter`.`option,` result` were renamed for
consistency.`to_foo` for copying, `into_foo` for moving, `as_foo` for temporary
and cheap casts.`c_str` provides new ways to convert to and
from C strings.`mut_split` method on vectors partitions an `&mut [T]` into
two splices.`str::from_bytes` renamed to `pop_opt` and `shift_opt` methods added to vectors.`swap_unwrap` method of `take_unwrap`.`SharedPort` to `ne`; only `eq` is required
in implementations.`is_utf8` performance is improved, impacting many string functions.`os::MemoryMap` provides cross-platform mmap.`ptr::offset` is now unsafe, but also more optimized. Offsets that
are not âin-boundsâ are considered undefined.`collect` method.`unstable::atomics`.`comm::PortSet` removed.`Set` and `Map` traits have been moved into
the `MutableSet` and `MutableMap` traits. `Container::is_empty`,
`Map::contains_key`, `MutableMap::insert`, and `MutableMap::remove` have
default implementations.`from_str` functions were removed in favor of a generic
`util::unreachable` removed in favor of the `dlist`, the doubly-linked list was modernized.`hex` module with `ToHex` and `FromHex` traits.`glob` module, replacing `std::os::glob`.`rope` was removed.`deque` was renamed to `ringbuf`. `RingBuf` implements `Deque`.`net`, and `timer` were removed. The experimental replacements
are `std::rt::io::net` and `std::rt::io::timer`.`SmallIntMap`.`Bitv` and `BitvSet`.`SmallIntSet` removed. Use `semver` updated to SemVer 2.0.0.`term` handles more terminals correctly.`dbg` module removed.`par` module removed.`future` was cleaned up, with some method renames.`getopts` were converted to methods.`-Z debug-info`) is greatly improved.`--target-cpu` to compile to a specific CPU architecture,
similarly to gccâs `--march` flag.`-h` and ~2000 changes, numerous bugfixes

`&'self self`,
and can be explicitly marked with a lifetime.`Durable` trait is replaced with the `Option<~T>` is now represented as a nullable pointer.`#[main]` attribute is still valid anywhere.`#[no_send]` attribute makes a type that would otherwise be
`#[no_freeze]` attribute makes a type that would otherwise be
`RUST_MAX_STACK` environment variable (default: 1GB).`vecs_implicitly_copyable` lint mode has been removed. Vectors
are never implicitly copyable.`#[static_assert]` makes compile-time assertions about static bools.`use mod` statement no longer exists.
Syntax extensions

`&'static str` or `Encodable`, `Decodable`, `TotalOrd`, `TotalEq`, `DeepClone`,
`Rand`, `Zero` and `bytes!` macro returns a vector of bytes for string, u8, char,
and unsuffixed integer literals.`extra`.`any`, `finalize` method of `drop`.`ApproxEq`, `Equiv`,
`IteratorUtil`, many numeric traits, many tuple traits.`Fractional`, `Real`, `RealExt`, `Integer`, `Ratio`,
`Algebraic`, `Trigonometric`, `Exponential`, `Primitive`.`(0, 1, 2).n2()` or `(0, 1, 2).n2_ref()`.`Mut` type removed.`reinterpret_cast` removed. Use `std::ascii`.`unstable::atomic`.`LinearMap` and `LinearSet` renamed to `ptr` to `borrow`.`os::mkdir_recursive`.`os::glob` function performs filesystems globs.`FuzzyEq` renamed to `pop` and `swap` methods.`rc` module adds the reference counted pointers, `RcMut`.`flate` module moved from `fileinput` module for iterating over a series of files.`Complex` number type and `complex` module.`Rational` number type and `rational` module.`BigInt`, `BigUint` implement numeric and comparison traits.`arc` functions converted to methods.`unused_variables` lint mode for unused variables (default: warn).`dead_assignment` lint mode for unread variables (default: warn).`unnecessary_allocation` lint mode detects some heap allocations that are
immediately borrowed so could be written without allocating (default: warn).`missing_doc` lint mode (default: allow).`--link-args` flag to pass arguments to the linker.`-Z print-link-args` flag for debugging linkage.`-g` will make the binary record information about
dynamic borrowcheck failures for debugging.
~2100 changes, numerous bugfixes

Syntax changes

`fn f(&self) { }`). Implicit self is deprecated.`impl Trait for Type`
instead of `impl Type: Trait``&'l foo` instead of `&l/foo``export` keyword has finally been removed`[mut T]`, has been
removed. Use `~mut T`. Use inherited mutability`fail` is no longer a keyword. Use `fail!()``assert` is no longer a keyword. Use `assert!()``(T,)``@mut T`, `core::mut` or `core::cell``extern mod { ... }` is no longer valid syntax for foreign
function modules. Use extern blocks: `extern { ... }``#[deriving_eq]` with `#[deriving(Eq)]`, etc.`#[deriving(Clone)]``@foo as @Bar`
instead of `foo as Bar`.`[int, .. 3]`
instead of `[int * 3]`.`[int, .. GL_BUFFER_SIZE - 2]`)
Semantic changes

`super::`
or `self::` to change the search behavior.`std::bigint``core::oldcomm` module`core::comm` module`core::num``vec::slice` finally returns a slice`debug!(Foo)``core::container``core::dvec` removed, `core::send_map` renamed to `core::hashmap``std::map` removed; replaced with `std::treemap` reimplemented as an owned balanced tree`std::deque` and `std::smallintmap` reimplemented as owned containers`core::trie` added as a fast ordered map for integer keys`rustc --test` now supports benchmarks with the ~900 changes, numerous bugfixes

`<-` move operator`#fmt` extension syntax to `[T]/N``quote_tokens!`, `quote_expr!`, etc.`a.b()` is always parsed as a method call, never as a field projection`#[deriving_iter_bytes]` respectively`.rc` files`struct Foo(Bar, Baz)`. Will replace newtype enums.`*T`
Improved support for language features

`~self` all generally work as expected`core::condition``std::sort``std::priority_queue``oldcomm`. Still deprecated
Syntax

`ret` became `alt` became `use is now` extern mod``os::args` instead
Semantics

`std::net::url` for representing URLs
Concurrency

`core::pipes``std::arc`, an atomically reference counted, immutable, shared memory
type`std::sync`, various exotic synchronization tools based on arcs and pipes
New coding conveniences

Semantic cleanup

Experimental new language features

Type reflection

Removal of various obsolete features

Keywords: âbeâ, âproveâ, âsyntaxâ, ânoteâ, âmutableâ, âbindâ, âcrustâ, ânativeâ (now âexternâ), âcontâ (now âagainâ)

Constructs: do-while loops (âdoâ repurposed), fn binding, resources (replaced by destructors)

Compiler reorganization

New library code

Tool improvements

1500 changes, numerous bugfixes


New docs and doc tooling

New port: FreeBSD x86_64

Compilation model enhancements

Scheduling, stack and threading fixes

Various language extensions

Most language features work, including:

Compiler works with the following configurations:

Cross compilation / multi-target configuration supported.

Preliminary API-documentation and package-management tools included.

Known issues:

Documentation is incomplete.

Performance is below intended target.

Standard library APIs are subject to extensive change, reorganization.

Language-level versioning is not yet operational - future code will break unexpectedly.
