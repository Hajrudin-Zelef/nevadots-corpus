---
id: collect-261121-veille-ia/langages-dev/rust-devreads-fyi
title: "Rust - devreads.fyi"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["rust", "rustup", "team", "version", "programming", "language", "everyone", "build"]
source: https://www.devreads.fyi/source/rust/
source_anchor: ""
source_lines: [1, 135]
section: "Langages_Dev"
---
TL;DR: We are enabling the parallel frontend of the Rust compiler on nightly by default, to find remaining issues in preparation for its stabilization. Context The Rust compiler has been using multiple threads to compile code in its "backend" part (using LLVM) for a long time. However, its "frontend" part (which includes type checking and borrow checking) has been executingâ¦

## 16 Nov

## 12 Nov

The Rust team is happy to announce a new version of Rust, 1.100.0. Rust is a programming language empowering everyone to build reliable and efficient software. If you have a previous version of Rust installed via rustup, you can get 1.100.0 with: $ rustup update stable If you don't have it already, you can get rustup from the appropriate pageâ¦

## 11 Nov

The Rust Project participated in Google Summer of Code (GSoC) for the third time this year. This time we had 13 projects, and our dear contributors have been working very hard on them for several months. As usual, the projects had various durations, so while some of them have ended in August, the last one has only finished in November,â¦

## 3 Nov

The rustup team has published a new point release of rustup, 1.29.3. Rustup is the recommended tool to install Rust, a programming language that empowers everyone to build reliable and efficient software. What's new in rustup 1.29.3 This new patch release has fixed some minor regressions over the previous one: Fixed "Illegal instruction" error on certain x86_64 CPUs without SSE4.1.â¦

## 26 Oct

TLDR: To fund general maintenance, donate to the Rust Foundation Maintainers Fund or sponsor the RustNL Maintainers Team To fund specific features and improvements, sponsor fundable Rust Project Goals To fund individual contributors, donate to their GitHub Sponsors accounts There is always so much to do in the Rust Project! New features to implement, bugs to fix, performance optimizations toâ¦

## 20 Oct

The rustup team is happy to announce the release of rustup version 1.29.2. Rustup is the recommended tool to install Rust, a programming language that empowers everyone to build reliable and efficient software. What's new in rustup 1.29.2 This is a small release which only updates the rustls-webpki and webpki-root-certs dependencies to address the problematic eMudhra intermediate certificates issue describedâ¦

## 15 Oct

The Rust team has published a new point release of Rust, 1.99.1. Rust is a programming language that is empowering everyone to build reliable and efficient software. If you have a previous version of Rust installed via rustup, getting Rust 1.99.1 is as easy as: rustup update stable If you don't have it already, you can get rustup from theâ¦

## 2 Oct

With Rust 1.100.0, the following changes to 32-bit Windows targets will happen: i686-pc-windows-msvc Tier 1 with host tools target will be demoted to Tier 1 without host tools. i686-pc-windows-gnu Tier 2 with host tools target will be demoted to Tier 2 without host tools. Builds of the standard library will continue to be distributed, but host tools such as theâ¦

## 1 Oct

The Rust team is happy to announce a new version of Rust, 1.99.0. Rust is a programming language empowering everyone to build reliable and efficient software. If you have a previous version of Rust installed via rustup, you can get 1.99.0 with: $ rustup update stable If you don't have it already, you can get rustup from the appropriate pageâ¦

## 22 Sept

At the end of August, we announced our first Maintainers in Residence, Rust Project contributors who are funded for their upstream contributions and maintenance work from the Rust Foundation Maintainers Fund (RFMF). Since then, the Rust Leadership Council has dedicated more funds from its Project Priorities budget to RFMF, and together with AWS also providing additional funds, this allowed usâ¦

## 21 Sept

The Rust Security Response Team was notified that Miri stores all environment variables to target/, allowing secrets to persist in caches. While not necessary a vulnerability in and of itself, when paired with GitHub Actions caching behavior, it is possible for this to expose secrets to PRs. Overview GitHub Actions makes it possible to cache directories between runs. Typical setupsâ¦

## 17 Sept

We believe that there is an ongoing campaign targeting rust-lang members and owners of popular crates that is attempting to compromise devices and accounts in order to use them to publish malware. What we've seen A video call is set up for something positive â maybe for a job, maybe for a project, maybe for a contract opportunity â andâ¦

## 7 Sept

One of the biggest challenges Rust developers report in our annual surveys is a subpar debugging experience. So, back in February, we ran our first Rust Debugging Survey, in the hopes of identifying how Rust developers are using debuggers and what problems they are facing when doing so. We received over 2,300 responses, and we'd like to thank everyone whoâ¦

## 3 Sept

The Rust team has published a new point release of Rust, 1.98.1. Rust is a programming language that is empowering everyone to build reliable and efficient software. If you have a previous version of Rust installed via rustup, getting Rust 1.98.1 is as easy as: rustup update stable If you don't have it already, you can get rustup from theâ¦

## 1 Sept

The rustup team is happy to announce the release of rustup version 1.29.1. Rustup is the recommended tool to install Rust, a programming language that empowers everyone to build reliable and efficient software. What's new in rustup 1.29.1 The headlines of this release are: Concurrency in certain rustup operations has been improved: When running rustup update, rustup will first checkâ¦

## 26 Aug

We are very happy to announce the Rust Project's first round of Maintainers in Residence: Gen Li (@rami3l), Chris Denton (@ChrisDenton), Alejandra GonzÃ¡lez (@blyxyas), LeÃ³n Liehr (@fmease), and Maintainer Grant recipients: Jason Newcomb (@Jarcho) and Jonas BÃ¶ttiger (@joboet). These contributors will be funded for their rust-lang maintenance activities for (at least) the following 12 months! The funding of the Maintainerâ¦

## 21 Aug

After nearly 4 years of active development, the next-generation trait solver is close to stabilization. We are enabling it by default on nightly to surface any remaining issues and plan to stabilize it in the next months. This is the largest single change to the Rust compiler since its initial release. It completely replaces how we prove where-clauses, normalize associatedâ¦

## 20 Aug

What happened On 2026-08-20 at 7:15 UTC we got a report that the proc-macro1 crate was malicious. The Rust Security Response Team verified this to be the case: the crate had a build script that was downloading a malicious payload. This crate proc-macro1 and others like it (proc-macro-en, aovine, arone, aronenao, tinymember) have been deleted. Furthermore, we discovered that theâ¦

The Rust team is happy to announce a new version of Rust, 1.98.0. Rust is a programming language empowering everyone to build reliable and efficient software. If you have a previous version of Rust installed via rustup, you can get 1.98.0 with: $ rustup update stable If you don't have it already, you can get rustup from the appropriate pageâ¦

## 4 Aug

TL;DR We are enabling the next iteration of the borrow checker (coined Polonius Alpha) on nightly in preparation for stabilization in the next few months. Whaaaaaat? Yes! You heard it right! The next iteration of the Rust borrow checker is coming! Rust's first borrow checker ("AST borrowck") was very limited and was phased out in 2019 in favor of NLL,â¦

## 16 Jul

The Rust team has published a new point release of Rust, 1.97.1. Rust is a programming language that is empowering everyone to build reliable and efficient software. If you have a previous version of Rust installed via rustup, getting Rust 1.97.1 is as easy as: rustup update stable If you don't have it already, you can get rustup from theâ¦

## 13 Jul

Another six months have passed since our last development update, and the crates.io team has been busy. Here's a summary of the most notable changes and improvements made to crates.io since then. Source Code Viewer Crate pages now have a "Code" tab that lets you browse the contents of published crate versions directly on crates.io. This shows you the exactâ¦

## 9 Jul

The Rust team is happy to announce a new version of Rust, 1.97.0. Rust is a programming language empowering everyone to build reliable and efficient software. If you have a previous version of Rust installed via rustup, you can get 1.97.0 with: $ rustup update stable If you don't have it already, you can get rustup from the appropriate pageâ¦

## 30 Jun

The Rust team has published a new point release of Rust, 1.96.1. Rust is a programming language that is empowering everyone to build reliable and efficient software. If you have a previous version of Rust installed via rustup, getting Rust 1.96.1 is as easy as: rustup update stable If you don't have it already, you can get rustup from theâ¦

## 25 Jun

This is another post in our series covering what we learned through the Vision Doc process. We previously described the overall approach and what we learned about doing user research, we explored what people love about Rust, dug into what it takes to ship safety-crticial Rust, and described some of the major challenges that people face when using Rust. Inâ¦

## 2 Jun

If you want to financially support the development of Rust, please consider donating to the Rust Foundation Maintainers Fund. A few months ago, the Rust Foundation announced the Rust Foundation Maintainers Fund (RFMF). Since then, the Rust Project has been closely cooperating with the Rust Foundation to determine how exactly this fund will be used to support Rust maintainers. Thisâ¦

## 28 May

The Rust team is happy to announce a new version of Rust, 1.96.0. Rust is a programming language empowering everyone to build reliable and efficient software. If you have a previous version of Rust installed via rustup, you can get 1.96.0 with: $ rustup update stable If you don't have it already, you can get rustup from the appropriate pageâ¦

## 25 May

The Rust Security Response Team was notified that Cargo incorrectly handled symlinks inside of crate tarballs downloaded from third-party registries, allowing a malicious crate to override the source code of another crate from the same registry. This vulnerability is tracked as CVE-2026-5223. The severity of the vulnerability is medium for users of third-party registries. Users of crates.io are not affected,â¦

The Rust Security Response Team was notified that Cargo incorrectly normalized the URLs of third-party registries using the sparse index protocol. If a hosting provider allowed multiple registries to be hosted with arbitrary names within the same domain, an attacker able to publish crates in a registry could obtain the credentials of others users of the same registry. This vulnerabilityâ¦

## 18 May

The 2025H2 Project Goal period has now concluded. Over these months, the Rust Project pursued 41 Project Goals, 13 of which were designated as Flagship Goals. This post contains curated updates on our progress since the last post and the final status for each of the goals (many of which continue as part of the 2026 period). Full details forâ¦

## 4 May

The Rust Project has been building up a good history of participating in various open-source mentorship programs, including Google Summer of Code for three years (including this year) and previously OSPP. We're happy to announce that this year we are also participating in Outreachy starting in the May 2026 cohort. Each of these mentorship programs has different criteria for eligibilityâ¦

## 1 May

The nvptx64-nvidia-cuda target is a compilation target for NVIDIA GPUs. When using this target, the final output is PTX. Two version choices shape that output: a GPU architecture (for example, sm_70, sm_80, â¦), which determines which GPUs can run the PTX, and a PTX ISA version, which determines which CUDA driver versions can load (and JIT-compile) the PTX. In Rustâ¦

## 30 Apr

As previously announced, the Rust Project is participating in Google Summer of Code (GSoC) 2026. GSoC is a global program organized by Google that is designed to bring new contributors to the world of open source. A few months ago, we published a list of GSoC project ideas, and started discussing these projects with potential GSoC applicants on our Zulip.â¦

## 16 Apr

The Rust team is happy to announce a new version of Rust, 1.95.0. Rust is a programming language empowering everyone to build reliable and efficient software. If you have a previous version of Rust installed via rustup, you can get 1.95.0 with: $ rustup update stable If you don't have it already, you can get rustup from the appropriate pageâ¦

## 4 Apr

Building fewer targets by default On 2026-05-01, docs.rs will make a breaking change to its build behavior. Today, if a crate does not define a targets list in its docs.rs metadata, docs.rs builds documentation for a default list of five targets. Starting on 2026-05-01, docs.rs will instead build documentation for only the default target unless additional targets are requested explicitly.â¦
