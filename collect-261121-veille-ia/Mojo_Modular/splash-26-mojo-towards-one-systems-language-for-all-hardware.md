---
id: collect-261121-veille-ia/mojo-modular/splash-26-mojo-towards-one-systems-language-for-all-hardware
title: "[SPLASH'26] Mojo: Towards One Systems Language for All Hardware"
domain: mojo-modular
role: reference
task: article
actors: []
dates: []
keywords: ["mojo", "rust", "about", "language", "lifetimes", "like", "splash", "systems"]
source: https://vibingtalk.com/index.php?threads/splash26-mojo-towards-one-systems-language-for-all-hardware.1965122/
source_anchor: ""
source_lines: [1, 29]
section: "Mojo_Modular"
---
[SPLASH'26] Mojo: Aimed at one systems language for all hardware
Chris Lattner dives into Mojo's design. Key points: comptime vs Rust's const generics and derive macros, origins vs Rust lifetimes.

I don’t like long talks, but this one’s worth it. I’ve been watching Mojo for years, waiting for it to match Rust’s features, and it’s finally starting to catch up. Mojo’s focus on multi-hardware support seems mostly about running AI models on any device, cool, but not what I care about most. I’m hoping they’re giving the language more time to build out real Rust-like features.

Yeah, same here. They added pattern matching and enums recently, so it's worth trying now, but still missing a lot.

The comptime with symbolic reasoning is solid, kinda like Zig mixed with a simple proof assistant, way more useful than Rust's const generics in my book.

Origins feel stronger than Rust's lifetimes, especially being able to generalize over mutability and support interior origins, which helps with collections.

My biggest concern with Rust is that lifetime annotations pop up on nearly every line, yet they rarely connect to the actual business logic of what you're trying to build.

Rust’s strong enums and pattern matching do help you think more clearly about problems and write cleaner code by working with types.

Ownership and lifetimes prevent certain bugs, but it's not obvious they make the program itself better.

I hope Mojo eases some of that boilerplate, but you still need to spend time thinking about ownership and borrowing.

lifetime annotations aren't really about business logic, they're about memory safety. it's not an upgrade, more like a rule you have to follow to avoid crashes. if you don't want to handle memory manually, you can `.clone()` when the borrow checker complains, but only if you understand how borrows and moves work, especially with closures. otherwise, you end up with runtime overhead. alternatively, use a language with garbage collection, rust’s core design just doesn’t support what some people want to build. ownership and lifetimes? they're like regulations in a restaurant. you could source meat yourself and track where it comes from, or get it from a third party who handles safety (like a gc), but then you can't verify quality or origin, just that it meets basic standards.

my biggest worry with rust is that lifetime annotations pop up on nearly every line, but they rarely tie into the actual business logic you're trying to solve

it's not a huge deal honestly, you rarely need to hold references in structs or enums, and for functions or methods, the lifetime elision rules usually work fine

i hope mojo helps reduce some of that boilerplate, even if you still gotta think about ownership and borrow rules

i don't think it's much better though, because if you want safe memory references without garbage collection, you still need some kind of type hint or parameter that defines how long a value lives
rust uses lifetimes, which are basically chunks of code, while mojo tracks where a value came from
