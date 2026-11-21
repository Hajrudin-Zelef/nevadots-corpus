---
id: collect-261121-veille-ia/langages-dev/this-month-in-rust-osdev-august-2026
title: "This Month in Rust OSDev: August 2026"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["rust", "month", "osdev", "linux", "target", "august", "issue", "posts"]
source: https://rust-osdev.com/this-month/2026-08/
source_anchor: ""
source_lines: [1, 241]
section: "Langages_Dev"
---
# This Month in Rust OSDev: August 2026

Welcome to a new issue of *"This Month in Rust OSDev"*. In these posts, we give a regular overview of notable changes in the Rust operating system development ecosystem.

This series is openly developed on GitHub. Feel free to open pull requests there with content you would like to see in the next issue. If you find some issues on this page, please report them by creating an issue or using our *comment form* at the bottom of this page.

Please submit interesting posts and projects for the next issue by commenting on the draft pull request or via a PR on GitHub.

Disclaimer: Automated scripts and AI assistance were used for collecting and categorizing links. Everything was proofread and checked manually, with many manual tweaks.
## Announcements, News, and Blog Posts

Here we collect news, blog posts, etc. related to OS development in Rust.

- Rui Ueyama: "We are rewriting the mold linker in Rust"
  - The announcement of mold 3.0: besides the move to Rust, mold is gaining linker script support, with the explicit goal of linking "essentially anything GNU ld can, including kernels and embedded programs".
- RSoC 2026: EEVDF for Redox
  - Report on this year's Redox Summer of Code project, which replaced Redox's DWRR scheduler with an EEVDF ("Earliest Eligible Virtual Deadline First") implementation.
- `rseclinux/ouma` : A hardened libc for GNU/Linux systems
  - A libc for Linux written entirely in Rust, with hardening as an explicit design goal.
- `Luernn/AldecaldOS` : An hybrid RTOS
  - A from-scratch embedded kernel in Rust, written as a hybrid real-time OS.
- RamShared v2: Writing a Linux VRAM Block Driver in Rust & C with 8.74 GiB/s PCIe DMA, io_uring/ublk, and 3 Patches on lore.kernel.org
  - Turns idle GPU VRAM into a block device, via a `ublk` userspace driver built on`io_uring` plus a Linux 6.18 block driver submitted upstream to the linux-block subsystem.
- Turns idle GPU VRAM into a block device, via a 
- `#[target_feature(enable = "avx2")]` does nothing on`x86_64-unknown-uefi`  - A debugging story about hand-written AVX2 kernels that compiled to zero vector instructions on the `x86_64-unknown-uefi` target, because that target enables`+soft-float` .
- A debugging story about hand-written AVX2 kernels that compiled to zero vector instructions on the 
- How `no_std` are`no_std` Rust crates really? A survey
  - Surveys crates that advertise `no_std` support and checks how many of them actually build without`std` , and under which feature combinations.
- Surveys crates that advertise 
- Alpha-2 Release of RSMalloc: A general-purpose Restartable Sequences (RSEQ) based memory allocator
  - An allocator built on Linux's restartable sequences, which let a thread's fast path be restarted rather than locked when it is preempted.
- How Firecracker microVMs work under the hood to sandbox untrusted code and AI agents
  - A tour of the Rust-based Firecracker VMM: how it uses KVM, what its minimal device model looks like, and how the guest kernel is booted.
- How we developed the world's first safety-certified product written in Rust – and why we went bare metal
  - Sonair on getting a Rust codebase through safety certification, including why they dropped an RTOS in favour of running bare metal.
- rustc_codegen_gcc: Progress Report #43
  - Latest progress on the GCC backend for `rustc` , which is one route to Rust on architectures LLVM does not target.
- Latest progress on the GCC backend for 
- A chip-agnostic architecture for bare-metal embedded Rust
  - Argues for keeping the hardware-specific layer of a bare-metal project behind narrow traits, so that most of the firmware can be built and tested without the target chip.
- Minecraft clone in no_std Rust + MMX inline asm running on an IBM ThinkPad 600E
  - A `no_std` Rust renderer with hand-written MMX inline assembly, booted on a Pentium II laptop.
- A 
- The world's first Game Boy ROM written in Pure Rust
  - `rust-gb` compiles Rust for the Game Boy's SM83 CPU on top of an LLVM Z80 backend - a good illustration of what bringing up a genuinely new bare-metal target involves.
- Your E-Paper Panel Isn't Broken: How Retained State Makes Drivers Look Buggy
  - On writing drivers for devices that keep state across resets, and why the usual "initialize everything at startup" assumption breaks down there.
- A gentle introduction to Embedded Rust
- The Embedded Rustacean Issue #77 and Issue #78

## Infrastructure and Tooling

In this section, we collect recent updates to `rustc`, `cargo`, and other tooling that are relevant to Rust OS development.

- target_features: sse (or at least avx2) is incompatible with soft-float ABI
  - Enabling `sse` (and therefore any x86 vector feature) via`#[target_feature]` on a soft-float target such as`x86_64-unknown-uefi` is not supported by LLVM and can crash the backend or silently drop all vector instructions. This is now a future-compatibility warning, reported in dependencies as well, so that whoever builds the final binary actually sees it.
- Enabling 
- Stabilize `extern "custom"`  - An `extern "custom" fn` has a calling convention that Rust knows nothing about and therefore refuses to call normally. This is the supported way to write`#[naked]` entry points that are only ever reached from hardware or hand-written assembly, such as interrupt handlers or compiler-runtime symbols like`__aeabi_uidivmod` .
- An 
- stabilize size_of_val_raw, align_of_val_raw, Layout::for_value_raw
  - Size and alignment of a value can now be queried through a raw pointer on stable, without having to create a reference first. This matters for allocators and anything else that handles memory which is not (yet) a valid value.
- make atomic operations const
  - Atomic loads, stores, and read-modify-write operations are now usable in `const` contexts.
- Atomic loads, stores, and read-modify-write operations are now usable in 
- stabilize `c_variadic_naked_functions`  - `#[naked]` functions may now use the C variadic ABI, which is needed for hand-written trampolines into variadic C interfaces.
- Stabilize passing 128-bit integers via vector registers with `asm!` on x86
  - `i128` and`u128` can now be passed to and from inline assembly in SSE registers.
- Move `std::io::copy` to`alloc::io`  - Continues the move of `std::io` into`core` and`alloc` that we covered last month.
- Continues the move of 
- std: uefi: fix File::seek returning the EOF sentinel
  - On the `x86_64-unknown-uefi` std target,`seek(SeekFrom::End(0))` returned UEFI's`0xFFFF_FFFF_FFFF_FFFF` "end of file" sentinel instead of the actual position, which also broke the default`Seek::stream_len` .
- On the 
- Re-stabilize build-dir layout v2
  - Cargo's new `build-dir` layout is stable again after being reverted in July. Custom runners that locate test binaries themselves may need the same kind of adjustment that`bootimage` made last month.
- Cargo's new 
- volatile: allow accesses to non-AM memory to trap
  - Not merged yet, but worth watching: this specifies that volatile accesses may trap, which is what MMIO code relies on in practice. According to the author it also removes the last case of "time-traveling UB" in Rust.

## `rust-osdev` Projects

In this section, we give an overview of notable changes to the projects hosted under the `rust-osdev` organization.

Maintained by @nicholasbishop and @phip1611

`uefi` makes it easy to develop Rust software that leverages safe, convenient,
and performant abstractions for UEFI functionality.

This month was all about **specification compliance and soundness**. We audited
large parts of `uefi-raw` and `uefi` against the UEFI and PI specifications.
Users now get correct data where the crates previously returned garbage or read
out of bounds, for example:

- `boot::set_watchdog_timer` passed the watchdog data size in characters instead
of bytes, so firmware only saw half of the data.
- `ProcessorInformation` was 24 bytes too small, so firmware could write past
its end.
- `UsbIo::supported_languages` reported twice the actual number of language IDs,
where the second half was an out-of-bounds read.

`MemoryDescriptor` is now portable across x86 targets, so kernels and
bootloaders built for a generic i686 target can finally parse a UEFI memory map.
To keep such bugs away, our ABI tests are now `const` assertions evaluated for
the actual target, instead of unit tests that only ever check the host.

The new `char16!()` macro builds a `Char16` from a character literal in `const`
context - no `unsafe` needed, and a compile error if the character is not valid
in UCS-2.

All of this is available in `uefi-raw v0.16.0` and `uefi v0.40.0`. We also
refreshed our `CONTRIBUTING.md`, which now documents our expectations regarding
code style, commit style, and AI/LLM-assisted contributions.

#### Sponsorship by Anthropic

We are glad to announce that Anthropic sponsors @phip1611 for six months
as part of their open source program. The sponsorship covers `uefi-rs` and
related crates in the `rust-osdev` space, with a focus on security issues,
undefined behavior, and specification compliance. Thank you!

We merged the following PRs this month:

Thanks to @cwize1 and @SpecificProtagonist for their contributions!

Maintained by @phip1611

*Convenient and safe parsing of Multiboot2 Boot Information (MBI) structures and
the contained information tags. Usable in no_std environments, such as a kernel.
An optional builder feature also allows the construction of the corresponding
structures.*

We removed a whole class of undefined behavior. Parsing a structure with a value
unknown to the specification - an unknown framebuffer type, VBE memory model, or
header tag type - used to construct an invalid Rust enum. The new `raw_type!`
macro generates an ABI-safe newtype plus an open-set enum with a `Custom`
variant, so unknown values now pass through safely. `multiboot2-common` got
further soundness fixes around size and alignment validation.

Users also benefit from `BootInformation::get_tags`, which iterates over *all*
occurrences of a tag. Network and SMBIOS tags may legitimately appear multiple
times, but our API only exposed the first one. The builder gained `add_network`

- and it turned out that `Builder::network` never included the tag at all.

Released as `multiboot2 v0.26.1`, `multiboot2-header v0.10.0`, and
`multiboot2-common v0.5.0`. The `raw_type!` work follows in the next release.

We merged the following PRs this month:

Maintained by @phip1611

*Simple yet highly configurable low-level driver for 16550 UART devices,
typically known and used as serial ports or COM ports.*

Two releases, `v0.7.0` and `v0.8.0`, make the driver behave better on real
hardware. Sending no longer waits for the `MSR::CTS` line by default, as modern
hardware tends to leave that pin disconnected - which previously meant no output
at all. Those who need hardware flow control can re-enable the check via
`Config::check_cts_before_sending`.

Further, `Config::default()` now disables *all* interrupts, and `init()` enables
the configured ones only at the very end. This way, a driver does not receive
interrupts before it is ready to handle them.

We merged the following PRs this month:

- Allow configuring whether to care about the CTS line
- config: disable interrupts by default + document interrupt behavior of init()
- doc: tighten cts/hw control flow wording

Thanks to @meithecatte for this contribution!

Maintained by @IsaacWoods

The `acpi` repository contains crates for parsing the ACPI tables – data structures that the firmware of modern computers uses to relay information about the hardware to the OS.

We merged the following changes this month:

- Resolve names used as package elements lazily
- Fix mutex references and the global lock
- Add Support for Multiple IRQs in IrqDescriptor (2)
- Fix acpi_dumper manifest

Thanks to @martin-hughes, @ArthurHeymans, @ChocolateLoverRaj, and @hustlerone for their contributions!

Maintained by @mkroening

The `virtio-spec` crate provides definitions from the Virtual I/O Device (VIRTIO) specification.
This project aims to be unopinionated regarding actual VIRTIO drivers that are implemented on top of this crate.

We merged the following PRs this month:

Maintained by @phil-opp and @Freax13

The `bootloader` crate implements a custom Rust-based bootloader for easy loading of 64-bit ELF executables.

We merged the following changes this month:

- deps: bump uart_16550 to 0.8.0 + fix UEFI weirdness
  - Picks up the `uart_16550` changes described above. On the UEFI path, the bootloader now explicitly disconnects the UEFI console from the serial device before setting up its logger, so that the logger has exclusive ownership of the UART. Previously, UEFI kept driving the device, which duplicated console output on the serial port and fired interrupts before`init()` had finished.
- Picks up the 

Thanks to @phip1611 for this contribution!

## Other Projects

In this section, we describe updates to Rust OS projects that are not directly related to the `rust-osdev` organization. Feel free to create a pull request with the updates of your OS project for the next post.

(Section written by @jenningschaefer)

Open Nexus OS is a capability-based microkernel operating system written in Rust and targeting RISC-V.

This month, the Open Nexus graphical desktop stack reached a major milestone: its declarative UI DSL is now driving a working desktop interface. The `.nx` UI definitions and `.nxtheme` design tokens are used to implement the actual interface, allowing the desktop UI to be built from the same declarative system used to define its design.

The desktop stack includes a compositor, window manager, launcher, and UI components running on top of the Open Nexus userspace architecture. The project also includes a boot-to-desktop demonstration showing the graphical environment running in QEMU.

Website · Repository · Demo video

(Section written by @phip1611)

`tar-no-std` supports a relevant
subset of Tar archives to extract multiple files from a single Tar archive in
`no_std` environments with zero allocations. A typical use case is a kernel
reading an initial ramdisk.

The new `v0.5.0` release stops trusting the input. `TarArchive[Ref]::new` now
rejects invalid headers, checksums, payload sizes, and missing archive
termination, so a malformed archive fails right away instead of producing
garbage entries. Numeric fields with invalid UTF-8 bytes no longer silently
parse as zero, and `CorruptDataError` became an enum that names the violated
invariant. Additionally, there is now limited support for POSIX PAX archives
that use extended records only for optional metadata, such as high-precision
timestamps.

To keep it that way, the repository gained `cargo-fuzz` infrastructure,
including structure-aware fuzzing with checksum-valid archives.

Thanks to @internetisalie and @fogti for their contributions!

## Join Us?

Are you interested in Rust-based operating system development? Our `rust-osdev` organization is always open to new members and new projects. Just let us know if you want to join! A good way to get in touch is our Zulip chat.
