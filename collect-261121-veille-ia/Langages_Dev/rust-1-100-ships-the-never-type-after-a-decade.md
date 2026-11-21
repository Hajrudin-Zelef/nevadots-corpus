---
id: collect-261121-veille-ia/langages-dev/rust-1-100-ships-the-never-type-after-a-decade
title: "Rust 1.100 Ships the Never Type After a Decade"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["type", "rust", "never", "after", "fallback", "cargo", "change", "build"]
source: https://www.fosslinux.com/163936/rust-1-100-never-type-stabilized.htm
source_anchor: ""
source_lines: [1, 93]
section: "Langages_Dev"
---
*144*

Rust 1.100 is out. The Rust Release Team announced it on 12 November 2026, and the headline is the never type: stable at last, after what the team calls 10 years and 5 failed stabilization attempts. The quieter half is the part to read first. Rust 1.100 changes the never type fallback from () to ! in the 2015, 2018 and 2021 editions, and that can stop code which compiles today from compiling at all.

How bad is it in practice? The team calls the change largely unnoticeable, softened by future-compatibility warnings far ahead and by fixes shipped to public crates. Rare cases exist anyway. I would rather find those before the update than after.

## Why Rust 1.100 Matters Before You Update

A fallback is the type the compiler picks when nothing else constrains it. In the 2015, 2018 and 2021 editions, an unresolved never-type expression used to fall back to (), the unit type. As of 1.100 it falls back to ! on every edition, which edition 2024 already did.

| Edition | Fallback before 1.100 | Fallback in 1.100 | 
|---|---|---|
| 2015 | () | ! | 
| 2018 | () | ! | 
| 2021 | () | ! | 
| 2024 | ! | ! | 

The reason is structural. As the announcement puts it, allowing Infallible to change into an alias required a breaking change, and the fallback move is that change.

The change can also make the compiler spot more unreachable code, so a build may now warn about code after a diverging call. The edition guide puts the end state plainly: on all editions, the fallback type is now !.

## What Rust 1.100 Actually Stabilized

The never type is the type of a computation that never hands back a value. Expressions like return, break and std::process::exit all have it. The standard library docs call it the canonical uninhabited type, one with no values.

Expression position always worked, and so did naming it in a return type. Naming it anywhere is the new part. Result<T, !> was not allowed before this release, so a function that cannot fail can say so in its signature.

Coercion is what makes ! useful rather than merely odd. A value of type ! turns into any other type, so an if branch or match arm that returns early or panics fits wherever the surrounding type is expected. That is the property I lean on most.

Infallible now points at the same type, so every API that used it as an error type accepts !. That alias is where existing projects are likeliest to meet this release, because a type alias travels through dependencies faster than new syntax does.

## What Else Is New in Rust 1.100

Three additions stand out for anyone whose code leans on the standard library.

- **Custom allocators.** Core parts of allocator_api are stable, so library authors can define an allocator usable with Vec and Box through the Allocator trait.
- **DropGuard.** core::mem::DropGuard wraps a value and a closure, runs the closure when the guard drops, and has a dismiss method that hands the value back untouched.
- **Smaller additions.** bool::toggle, Box::take, Box::map, Rc::map, Arc::map, str::trim_prefix, Vec::from_fn and UnsafeCell::replace all stabilize in this release.

The safety argument behind half of that list is why any of it matters. FOSSLinux’s Rust vs C kernel security comparison covers it at depth, and FOSSLinux’s map of the Rust Linux ecosystem shows where these layers sit.

## Cargo Changes in Rust 1.100 You Will Feel

Cargo now reports unused dependencies. Rustc’s unused_crate_dependencies lint produces false positives under Cargo because one dependency can serve several build targets.

The warning is on by default at warn level, tuned in the [lints.cargo] table in Cargo.toml as unused_dependencies. It only fires when you build every target that might use a dependency. That is why cargo check –bin foo reports nothing, and why unused dev-dependencies are never reported. The dev-dependency gap is the one I trip over.

Build directories move too. The build-dir applies the configuration hash to all content, organising folders by build step rather than file type, and lifts the hash from the file name to the parent directory. Tools that guessed at that layout may break, which is what the cargo issue 17287 thread is about.

On a desktop app with a deep tree, the unused-dependency warning is the one I would keep on. Our look at Tributary, a GTK4 music player in Rust, is that kind of codebase.

## Dependency Cooldowns: Holding Back Fresh Publishes

This is the feature I have wanted for a while. min-publish-age, also known as dependency cooldowns, is stable. Two lines in .cargo/config.toml opt in: [registry], then global-min-publish-age set to a span in days.

Set it to “3 days” and Cargo skips anything newer, then lists what it held back in the cargo update output. The Cargo config reference spells the key out.

- **It needs pubtime.** The check depends on the registry recording when each version was published, and versions without a pubtime field in the index are never held back.
- **cargo install ignores it.** The minimum publish age applies to neither the installed crate nor its dependencies, so pin with cargo install foo@1.2.3.
- **It is not a complete defense.** Cooldowns cut the risk from some supply chain attacks and nothing more. Quarantine support and automated detection of suspicious releases sit in the crates.io supply chain hardening goal.

## What to Do Before and After the Upgrade

Order matters. Lockfile first, toolchain second, then rebuild everything.

- **Run cargo update on a branch first.** The announcement says a fix may already be published for a dependency broken by the never type.
- **Then switch toolchains with rustup update stable** , the documented route to 1.100.0 for anyone installed through rustup.
- **Read new unreachable-code warnings.** The fallback change can make the compiler flag code it used to accept.
- **Keep CI on the same channel.** A CI image on an older stable and a laptop on 1.100 disagree in ways that surface on a Friday afternoon.

If your Rust comes from your distribution rather than rustup, watch the package rather than this release announcement. The 1.100.0 release notes are the reference, and the Rust, Cargo and Clippy changelogs go deeper.

## FAQ

### Is the never type going to break my build?

Usually not. The team calls the change largely unnoticeable, though a return value someone ignored can still trip it.

### Do I need to move to edition 2024 to use it?

No. The never type is stable on every edition, and so is naming it anywhere. The fallback resolves to ! on the 2015, 2018 and 2021 editions too, so behavior matches edition 2024. I would not move editions for this alone.

### How do I turn on dependency cooldowns?

Add a [registry] table to .cargo/config.toml with global-min-publish-age set to a span such as 3 days. Cargo then holds back newer versions and lists them in the cargo update output.

## The Verdict: Update, Then Read the Warnings

Update. Naming the never type is worth having in signatures, Infallible behaving like ! removes a small papercut, and a cooldown in two lines is cheap insurance. What I would not do is treat 1.100 as routine on the machine that ships your builds.

The fallback change surfaces in dependencies nobody reads, and a fix may already be published for the ones that break. Ten minutes on a branch, a bad Friday on a deadline. I know which one I would pick.

The decade is the part worth remembering. A feature that took 10 years and 5 failed attempts arrived quietly, behind a fallback change the team calls largely unnoticeable. The 1.100.0 contributors page lists the people who put the decade in.
