---
id: collect-261121-veille-ia/langages-dev/rust-1-100-will-add-dependency-cooldowns-to-cargo
title: "Rust 1.100 Will Add Dependency Cooldowns to Cargo"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["rust", "cargo", "dependency", "malicious", "releases", "versions", "cooldowns", "before"]
source: https://socket.dev/blog/rust-cargo-cooldowns
source_anchor: ""
source_lines: [1, 46]
section: "Langages_Dev"
---
# Rust 1.100 Will Add Dependency Cooldowns to Cargo

Rust 1.100 will add opt-in dependency cooldowns to Cargo, giving security scanners and maintainers time to catch malicious crate releases.

- Sarah Gooding

Rust developers will soon be able to tell Cargo to wait before adopting newly published dependencies. Rust 1.100, scheduled for November 12, will bring stable support for a configurable minimum publish age, letting teams delay dependency updates to give researchers and maintainers time to detect malicious releases before more projects pull them in.

The stabilization PR merged on August 28, and Rust’s draft release notes list the feature for 1.100. The setting is available on beta but has not yet reached stable Rust.

Teams will have to switch this on themselves. Cargo’s default minimum publish age remains zero, so upgrading alone doesn't buy dependencies any cooling-off time.

In August, attackers published compromised versions of `arrayref`, `internment`, and `append-only-vec` that depended on `proc-macro1`, a malicious crate whose build script downloaded a payload. Socket documented the attack, in which the malicious `proc-macro1` dependency spread through three compromised crates.  According to the Rust Security Response Team, the these releases were available on crates.io for 86, 90, and 107 minutes, respectively, before removal.

All three releases were gone in under two hours. But dependency updates can move faster than an investigation, pulling malicious code onto developer machines and into CI before the registry takes it down. A cooldown gives defenders a head start.

The risk has continued beyond that incident. In September, the crates.io team and Rust security responders warned of an ongoing campaign targeting Rust project members and owners of popular crates. Attackers used purported job and project opportunities to arrange video calls, then tried to persuade targets to install software or execute commands that could compromise their devices and publishing accounts.

Work on the cooldown predates the August compromises. The tracking issue opened on May 18, and the feature became available in nightly Cargo on June 21, roughly two months before the malicious crate releases.

Developers were already pushing to ship it. In a May 25 comment on the tracking issue, Rust contributor Giles Cope raised concerns about AI helping attackers carry out supply chain attacks, arguing that “it's far more important we get an MVP version of this out early than we gold plate it.”

Teams can configure a seven-day waiting period in `.cargo/config.toml`:

```
[registry]
global-min-publish-age = "7 days"
```
Cargo’s configuration reference supports durations expressed in seconds, minutes, hours, days, weeks, or months. The resolver’s `incompatible-publish-age` setting defaults to `"deny"`, so a separate `[resolver]` entry is unnecessary unless another configuration has changed it.

When selecting dependency versions, Cargo excludes releases younger than the configured threshold. An older version can still be selected if it satisfies the project’s other requirements. If no eligible version works, resolution fails.

That failure behavior is deliberate. The feature’s RFC describes an attack in which someone publishes a malicious update and yanks the safe versions. A policy that merely prefers older releases could then fall back to the malicious one. Rejecting releases that are too new closes that route. Attackers yanked legitimate versions of `arrayref` during the August compromise.

Cargo also reports when the cooldown is in use and identifies newer versions it passed over, including their age. If resolution fails, its diagnostics explain how to adjust the version requirement or override the age check.

The resolver preserves versions already recorded in `Cargo.lock`, even if they are younger than the configured minimum. Enabling a cooldown therefore does not retroactively reject a fresh release that has already entered the lockfile.

For an urgent fix, a developer can temporarily disable the check and select a specific version. For example, replacing the package name and version as appropriate:

`CARGO_RESOLVER_INCOMPATIBLE_PUBLISH_AGE=allow cargo update -p some-crate --precise 1.2.3`
The override disables the age check for that invocation, including any transitive dependencies it needs to resolve. Once those versions are recorded in the lockfile, subsequent builds can continue using them under the normal policy.

There are limits to its coverage. The stabilized implementation does not apply the cooldown to `cargo install`. The check also depends on registry publication timestamps, so Git and local path dependencies are outside its scope, as are registries that do not provide that metadata.

The tradeoff is that legitimate fixes have to wait, too, unless developers make an exception. Malicious code also doesn’t magically become harmless with age. A release that escapes detection can simply outlast the cooldown. The value is in buying researchers, maintainers, and registries time to act before the next dependency update carries an attack further.
