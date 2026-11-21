---
id: collect-261121-veille-ia/langages-dev/rust-1-100-0-release-never-type-stabilizes-after-a-decade
title: "Rust 1.100.0 Release: Never Type Stabilizes After a Decade"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["type", "rust", "never", "decade", "release", "years", "after", "stable"]
source: https://www.linuxcompatible.org/story/rust-11000-release-never-type-stabilizes-after-a-decade/
source_anchor: ""
source_lines: [1, 61]
section: "Langages_Dev"
---
### Rust 1.100.0 ships, stabilizing the never type after a decade

**The first three-digit Rust release lands !, custom allocators, and supply-chain cooldowns.**

Rust has finally crossed into three-digit version territory. Rust 1.100.0 went stable today, carrying the language's first 1.100 milestone alongside a feature that's been in the works for a decade.

The number itself is a milestone of sorts. It's the 87th stable release in Rust's ten years of backward compatibility, and there's a bit of math humor in the community: in decimal, 1,100 reads as "12" in base 12, a fitting nod for a language that's been stable for roughly a dozen years. The release branched from master on September 25, 2026, and carried roughly 5,340 commits to main before going stable.

But the version bump is mostly a distraction. The actual reason people are celebrating is the never type. Spelled !, the feature was unstable for a full ten years, survived five failed stabilization attempts, and was finally completed by contributor waffle (WaffleLapkin) after more than two years of focused work.

That backward-compatibility promise dates back to Rust 1.0 in 2015. Code written a decade ago still compiles today, and that's the reason the language can ship a new version every six weeks without forcing the whole ecosystem to rewrite itself each time. The never type isn't something you'd write from scratch anyway. You could already produce ! values and even name them in return types like fn f() -> !. What was missing was the ability to name the type in any position, including as a generic argument like Result<_, !>.

The payoff comes from an unusual property. Because you can never actually hold a value of type !, Rust lets a ! expression coerce silently into any other type. That single trick gives cleaner control flow and lets generic code drop error-handling machinery that can never fire.

Here's the corner that tripped people up for years. Since 2015, the standard library carried Infallible, an empty enum filling the same conceptual role as !. It worked, but without compiler support, code using it often shipped dead code or enum tags the optimizer couldn't fully erase. The plan was to make Infallible a type alias for ! once the never type stabilized. The catch is that ! auto-coerces while Infallible doesn't, so the alias hid a breaking change inside what looked like a cleanup.

Add a second wrinkle. Before edition 2024, the compiler's type fallback defaulted to the unit type (). Edition 2024 flipped it to !. Aligning those two changes meant pushing the fallback shift across all earlier editions too, 2015, 2018, and 2021 included.

So Rust 1.100 changes that fallback from () to ! on pre-2024 editions, and a narrow corner case can now break. The classic trigger is the ? operator against a generic T: Default. The ? expands into a match. One branch is !, the other is an unnamed T. The old fallback picked (), which implements Default. The new one picks !, which does not, and you get a compile error.

The fix is just to name the type yourself.

The team didn't ship it and hope for the best. They emitted future-compatibility warnings years ahead so libraries had time to react. Then waffle ran Crater, the community tool that compiles every publicly available crate on crates.io against the candidate change to measure real-world breakage.

April's run flagged about 3,300 crates as negatively impacted. Only seven were fully broken. The rest failed only because they pinned to old versions of libraries that have since been fixed.

waffle then worked directly with maintainers to backport fixes, even past end-of-life. Roughly 1,553 failing crates got patched. Some maintainers declined, arguing old versions shouldn't be maintained, and those users were told to either stay on an older Rust or upgrade.

As one commenter noted, the 2014 stability manifesto already reserved the right to "change type inference in ways that may occasionally require new type annotations." The net effect is a theoretically breaking change that was practically contained, a decent textbook example of Rust's compatibility culture holding up under pressure.

#### Everything else in 1.100

Beyond the never type, this release kicks off what the team calls "Allocators 1.0." The core of the allocator_api feature is stable now, letting library authors define custom allocators for Vec and Box. The centerpiece is the new Allocator trait, plus allocator-aware constructors like Box::new_in and Vec::with_capacity_in. The hard design questions are settled, so broader stabilization should follow soon.

There's also a practical DropGuard in core::mem. It takes a value and a closure, keeps the value usable through Deref, and runs the closure on exit. A dismiss() method skips the cleanup, handy for transaction-style APIs that want automatic rollback.

For unsafe code, Rust 1.100 introduces std::mem::conjure_zst, a single explicit way to build a zero-sized type. Before this, developers leaned on incantations like transmuting () or MaybeUninit::uninit().assume_init(). The function is deliberately unsafe, because some safe APIs depend on not being able to conjure certain types out of thin air. PyO3's Python token is a prime example: letting code conjure one freely would open the door to GIL-violating calls routed through a "safe" API.

Cargo gets the most security-relevant upgrade in this release. A new "dependency cooldown" feature, implemented via RFC 3923, lets projects wait out the window when malicious crate versions typically get caught. Set global-min-publish-age in .cargo/config.toml and Cargo holds back anything newer than your cooldown, reporting exactly what it skipped.

Force an override when you actually need the new version immediately, with CARGO_RESOLVER_INCOMPATIBLE_PUBLISH_AGE=allow.

Cargo also lints unused dependencies now, part of the new [lints.cargo] system completing the "cargo-lints" goal. Keep in mind the warning only fires when every possible build target is compiled, so a bare cargo check won't catch everything. The tool also reorganizes its build-directory layout to cut cache collisions, and it now uses the lockfile from the .crate tarball by default, making --locked a no-op.

Then there's the platform list, which includes a PS3. Yes, the PlayStation 3. powerpc64-sony-ps3 lands as a new Tier 3 target, alongside wasm32-wasip3 gaining Tier 2 and i686-pc-windows-gnu getting promoted without host tools. Cheap to support, it arguably isn't, but it's clearly Rust having fun.

On the language side, extern "custom" ABIs stabilize, a new lint catches repeated #[repr(..)] attributes, and the "next solver" becomes the default in orphan coherence checking. The compiler also gains -Cprofile-sample-use for sample-profile optimization.

#### What could break

Not everything is frictionless. The never-type fallback shift is the most likely culprit, hitting pre-2024 editions first. Infallible is now a type alias, closures capture more precisely in if let bindings and may surface new borrow errors (see #154210), and box patterns are gone entirely.

Some Box impls generalized to arbitrary allocators, which may nudge type inference in tight spots. The standard library now follows Unicode 18.0.0, and the WASI SDK jumped from 33 to 34. ARM and x86 ABI rules got stricter too, with -Ctarget-feature values that alter the ABI now hard errors on ARM.

If something breaks, test against the beta channel ahead of time with rustup default beta, bump your dependencies, or pin to the previous toolchain.

The three-digit number always invites the same question: why not call it Rust 2.0? The answer goes back to that compatibility contract. Instead of a clean break that would force an ecosystem-wide rewrite, Rust uses editions, so far 2015, 2018, 2021, and 2024, to absorb incompatible changes in place. Some argue the "we still won't be on Rust 2.0" framing undersells what's happening. Crossing 1.100 is a symbolic milestone, but the substance, features like the never type, lands through persistence rather than revolution.

Update an existing rustup installation with rustup update stable, or install rustup.

Head here to the official announcement for the full changelog.
