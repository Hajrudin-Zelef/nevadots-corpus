---
id: collect-261121-veille-ia/docs-techniques/whats-new-in-1-98-0
title: "🦀 What’s New in 1.98.0"
domain: docs-techniques
role: reference
task: article
actors: []
dates: []
keywords: ["rust", "just", "rayon", "whats", "compiler", "release", "project", "news"]
source: https://weeklyrust.substack.com/p/whats-new-in-1980
source_anchor: ""
source_lines: [1, 82]
section: "Docs_Techniques"
---
Hello Rustaceans!

In this issue, we cover the recent Rust release of 1.98.0, spotlight a project, highlight 10 awesome links of the week, and present you a challenge.

Here’s issue **133.**

###                                   **MAIN NEWS**

## What’s New in 1.98.0

After a 35-day wait, the Rust team shipped Rust 1.98.0. This release brings a handful of neat additions and improvements that make Rust just a little nicer to use.

First up, algebraic floating-point methods, f32 and f64 now ship with *algebraic_add*, and *algebraic_mul*. These let the compiler rearrange operations using the nice properties of real numbers. Just the compiler getting to vectorize and reorder like it actually owns the place. Non-deterministic? Sure. It is useful when you care more about throughput than bit-identical results across runs.

Then there’s the performance flex, every primitive integer now has *format_into* that writes into a *NumBuffer*. Opaque buffer, returns a borrowed &str, and the benchmarks say it keeps up with the itoa crate. So if you’ve been dragging an extra dependency just to avoid dynamic dispatch on number formatting… you can probably drop it. Feels good, doesn’t it?

Another bonus round for the unsafe enjoyers, the ManuallyDrop + Box interaction got a proper docs upgrade. That sketchy drop the box then move the ManuallyDrop pattern that used to be UB is now explicitly not-UB and will stay that way.

This release just sanded down a few more sharp edges and hands you slightly better tools for the daily grind.

Now if you’ll excuse me, I need to go rewrite a few number-formatting hot paths just because I can.

### **PROJECT SPOTLIGHT** 💡

# Rayon

Rayon is a data-parallelism library for Rust. It is extremely lightweight and makes it easy to convert a sequential computation into a parallel one. It also guarantees data-race freedom.

What makes it stand out:

- **One-line parallel iterators** : Most of the time you just swap the method and Rayon handles the rest.
- **Data-race freedom by design** : If your parallel code compiles, it generally behaves like the sequential version.
- **Works where you need it** : From everyday crates to WebAssembly, it stays practical.

Rayon is open-source at https://github.com/rayon-rs/rayon.

###                                AWESOME LINKS OF THE WEEK 🔗

1. Rust Foundation June 2026 project Director update is out: donation policy discussed, 2FA tokens issued, crates.io funding brainstormed, interop advanced, RustConf hybrid meeting planned. Leadership Council selections open for Infra, Lang, Libs, Mods teams. Confirm choices by Sept 23 and new reps join meetings Sept 25. In other news, The Cargo team published an experiment in reducing target directory size on nightly. Rust enables next-generation trait solver by default on nightly, largest compiler change, fixes 200 plus issues, and unblocks features.
2. Turbovec is a Rust vector index using TurboQuant and compresses 10M docs to 4GB, and searches faster than FAISS with no training.
3. Max Hils and Bastian Kersting wrote how Google AI-rewrote giflib C library to Rust for memory safety, validated with millions of tests, and a neutral performance.
4. Dirkjan Ochtman open-sourced OxiSH, a modern, memory-safe SSH server, because SSH needed a Rusty glow-up.
5. Manuel, Marcelo, Kevin, Alán and Johannes published a paper on GPU Offload in Rust presenting safe portable multi vendor GPU offload for Rust integrated in compiler with competitive speeds to CUDA HIP baselines.
6. Alex Kladov wrote about Glancer, an alternative low-memory Rust LSP that indexes once to disk for instant restarts and works well on 8GB machines.
7. Only 14 days until RustConf 2026. Register now before the deadline sneaks up on you.
8. In other news, Canonical funds PhD research at Bristol for automated C to Rust translation using LLMs and formal methods on Ubuntu software.
9. Jiayi Zhuang wrote a 2026 Survey of Rust GUI Libraries by testing and reviewing each of the libraries listed on the Are We GUI Yet? website.
10. Freya v0.4 is out with major rewrite dropping Dioxus for its own reactive model, typed builders, better stability, and many new features.

### **RUST CHALLENGE 🦀**

In our previous issue, we said the challenges would be back and here it is. Hope you enjoy this one at least half as much as we enjoyed making it.

Given a vector of integers *nums*, return all unique triplets [a, b, c] such that a + b + c == 0. The solution must not contain duplicate triplets. Order of triplets and order inside a triplet does not matter.

Test your solution on Rust Playground. Once completed, please share your solution and tag us either on X, BlueSky, Mastodon, LinkedIn, or reply to this email.

CodeCrafters created amazing Rust courses that push your skills beyond the basics.

You’ll have fun building real-world projects from scratch, including Git, Docker, Redis, Kafka, SQLite, Grep, BitTorrent, HTTP Server, an Interpreter, and DNS.

The courses are self-paced, so you can learn at your own speed.

Join for free and get 40% off when you upgrade. [affiliate]

X | BlueSky | Mastodon | GitHub | LinkedIn | Buy us coffee | hello@rustbytes.com

Been having a rough few weeks. Hope you’re doing great and taking care of your body.

See you next week!

John & Elley.

algebraic_add/algebraic_mul is exactly the right shape for this

opt-in reassociation instead of a global fast-math flag. And format_into finally gives me an excuse to drop itoa from a couple of projects.

Good issue.

The 1.98 roundup is genuinely the most useful five minutes of my Rust week — release notes are where good ideas go to be ignored, so having someone pull out the parts that actually change day-to-day code is worth a lot.

On the "learn by building something small" note: I've been writing oxru, a minimal terminal text editor in Rust. Very much a WIP, only properly tested on macOS, and it has taught me more about ownership than any blog post ever did — mostly by refusing to compile. https://github.com/p32929/oxru
