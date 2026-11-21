---
id: collect-261121-veille-ia/langages-dev/rust-1-99-1-released-three-critical-bugs-fixed-in-record-two-week-turnaround
title: "Rust 1.99.1 Released: Three Critical Bugs Fixed in Record Two-Week Turnaround"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-10-15"]
keywords: ["rust", "three", "october", "critical", "bugs", "release", "released", "targets"]
source: https://www.linuxcompatible.org/story/rust-1991-released-three-critical-bugs-fixed-in-record-twoweek-turnaround/
source_anchor: ""
source_lines: [1, 47]
section: "Langages_Dev"
---
### Rust 1.99.1 ships after 1.99.0 shipped three bugs

**Three regressions from last month's stable get the boot in Rust's latest point release, two of them critical.**

Rust's release team published 1.99.1 on October 15, 2026. It clears three serious defects that rode along in 1.99.0, the stable cut released October 1. Two carry a "critical" rating. That's an unusually rough week for a language that sells itself on never shipping broken code.

Point releases exist precisely so you don't have to wait six weeks for a nasty bug. A patch normally removes one or two issues. This one cut three, all born from a single minor release.

#### The three fixes, worst first

The worst one is a silent correctness bug. Rust functions returning a bool across a foreign function interface could leave bits 1 through 7 of the return register unspecified. Callers might see raw values other than 0 or 1. In one report someone got 16 where a clean boolean was expected.

Wesley Moore, who runs yeslogic, opened the issue on October 7. His real-world reproduction involved interop with the Mercury programming language. A Rust function returning a bool worked fine when logged from the Rust side, then crashed the Mercury runtime when the value was read on Mercury's end. Logging it through a C foreign_proc exposed the 16. This isn't some exotic server workload. yeslogic makes the Glyphs font editor, so a designer's desktop caught the bug first.

The root is an ABI violation. The x86-64 System V ABI demands that a returned boolean be zero-extended to at least 8 bits. But the LLVM IR rustc was emitting had silently dropped the zeroext attribute, which lets LLVM treat the high bits as free. Contributor asquared31415 pinned the exact cause on October 7: the regression came from PR #142389, which stripped zero-extension from all scalar return types on x86_64. It only surfaced once PR #159317 removed its safety net. The test suite never caught it because it exercised x86_64-unknown-uefi, which handles bools correctly by accident, instead of the far more common x86_64-unknown-linux-gnu.

It's platform-specific as well. Only x86_64 broke. On aarch64 and loongarch64 the value gets masked before it returns, so those targets walked away fine. Member saethlin fixed it (PR #163917, reviewed by nikic) by restoring the zero-extension, merged October 10. It was rated P-critical, the highest triage level, and several teams had already pinned their CI to 1.98.1 while waiting.

The first is a missing symbol called wcslen. Some targets threw link errors naming it, and UEFI took the biggest hit. Any project using std on those targets failed to link entirely. Contributor mathisbot opened the issue on October 1 with a minimal #![no_std] example that compiled on 1.98.1 but blew up on the new stable. It only showed up in release mode, which is why it slipped past casual testing.

LLVM's loop-idiom-wcslen pass rewrites certain loops into string-length calls like wcslen, harmless on targets that already have the function in their system C library. The deeper cause was a narrow backport: an earlier fix added several intrinsics to compiler-builtins, but the commit that actually added wcslen "wasn't on the radar," per member tgr35. cuviper said the backport request itself was too narrow, which is why he annotated it "(partial)."

Since the broken calls are baked into the precompiled standard library, people couldn't just fix their own code. The practical workaround was RUSTFLAGS="-C llvm-args=-disable-loop-idiom-wcslen" to skip the offending optimization. 1.99.1 adds wcslen as a fallback in compiler-builtins, so the symbol is always available now.

The third is the most niche and the quietest. On Apple Silicon Macs, the bundled linker rust-lld would abort at load time with SIGABRT when rustc was invoked directly by path instead of through rustup. Try linking a wasm32-unknown-unknown cdylib and it chokes with a dyld error about libLLVM.dylib failing to load.

Keep in mind that this one is masked for most users. Through the normal rustup proxy, linking works fine because rustup injects the right search path automatically. The failure only bites direct invocation, or toolchains installed without rustup's shim, like those from the fenix installer.

This traces back to 1.96.1. PR #157205, first shipped in 1.98.0, enabled shared linking for the Apple Silicon build and pointed the rpath at a subdirectory the tarball doesn't fill with the library. Member rmi3l diagnosed that a Linux-only rpath tweak was never mirrored for Darwin, and the original PR's author, sgasho, confirmed the oversight. The fix (PR #163954) points the rpath at the directory that genuinely contains libLLVM.dylib. Merged October 9.

#### How it all landed in two weeks

The timeline reads like a case study in self-correction. Bug 163614 hit on October 1, with the bool miscompile and the macOS linker issue both landing on the 7th. Fixes rolled out between October 8 and 10, pre-release testing went out October 13, and stable shipped the 15th. Three serious regressions, two of them critical, gone in about two weeks.

At the time of writing, beta has moved on to 1.100.0 and nightly sits at 1.101.0, with 1.99.1 as the current stable.

On Lobsters, wezm described the slog of chasing the bool bug. "I found and reported the bool FFI miscompilation. The behaviour I was observing was very confusing, and I'm generally extremely hesitant to blame the compiler." The chain ran from failing tests to disassembly comparison across versions to a clean, well-formed issue filing. Then he floated a possible link to an earlier nightly miscompile in the SimplifyComparisonIntegral pass, hinting that bool and ABI handling keeps producing subtle bugs. Worth watching.

Compiler bugs are at their worst when silent. The bool issue is the poster child: it produced valid-looking but wrong machine code from correct source, with no diagnostic, across one of the most common calling conventions. Two of the three regressions also trace to changes nearly impossible to spot without deep tooling knowledge. The safety net worked here, and fast, but it's the same machinery that can go subtly wrong in ways that are easy to miss.

For you, the fix is a single command. Run rustup update stable to grab all three.

rustup update stable

If you build UEFI targets, do FFI across bool on x86_64, or invoke the macOS Apple Silicon toolchain directly, upgrade promptly. The first two especially.

Head here to the official announcement for full details.
