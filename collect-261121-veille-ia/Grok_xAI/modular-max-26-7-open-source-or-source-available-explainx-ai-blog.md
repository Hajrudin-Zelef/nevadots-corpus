---
id: collect-261121-veille-ia/grok-xai/modular-max-26-7-open-source-or-source-available-explainx-ai-blog
title: "Modular MAX 26.7: Open Source or Source-Available? | explainx.ai Blog"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: ["2026-10-22"]
keywords: ["modular", "open", "source", "compiler", "says", "license", "mojo", "blog"]
source: https://explainx.ai/blog/modular-max-source-available-26-7-graph-compiler-runtime-max-license-claimed-vs-verified-2026
source_anchor: ""
source_lines: [1, 83]
section: "Grok_xAI"
---
On October 22, 2026 Modular shipped **MAX 26.7** and opened up the last big pieces of its inference stack. Social posts summarizing the release, attributed to Chris Lattner, Modular's co-founder, described Mojo and MAX as "fully open - for development, extension, and commercial use" (we could not open the original post). The company's own blog says something narrower: "MAX is now source available and open to everyone." Developers "can now explore, modify, and extend the entire MAX stack," including the graph compiler, runtime and hardware backends for CPUs and GPUs.

The gap between "fully open" and "source available" matters if you plan to build a product on it. This post sets the claim against the license text in Modular's public repository, so you know what is confirmed, what you can do, and what to read before committing.

## TL;DR: claim versus what the repo says

| Question | What is claimed | What we could verify | 
|---|---|---|
| What opened? | "The full Mojo language, the MAX graph compiler, MLRT runtime internals, hardware abstraction" (social summary, not verified directly) | The repo README lists the graph compiler ( `/GraphCompiler` ), runtime (`/MLRT` ), driver, framework and pipelines as source available | 
| Is it "open source"? | "Fully open" (Lattner on X) | Modular's blog says "source available"; the LICENSE file says portions of MAX use the MAX License, the rest Apache 2.0 with LLVM Exceptions | 
| Commercial use? | "Yes" | Yes, with conditions: derivative works in object code, redistribution inside applications with "material additional functionality" | 
| Is Mojo itself open? | Yes | Yes. The compiler has been open since August 2026 (Apache 2.0 + LLVM Exceptions); the standard library since March 2024 | 
| Any catches? | Not mentioned | Telemetry rights, a no-reimplementation-via-AI clause, trademark rules for hosted services | 
| Release | MAX 26.7 | The MAX docs list v26.7 as the latest stable release, dated October 22, 2026 | 

## What opened, according to Modular

Modular's blog post of October 22 says the release builds on earlier open kernels, models and serving infrastructure, and opens the "remaining core components that power MAX's heterogeneous capabilities: the graph compiler, runtime, and hardware backends." A companion post titled "Modular 26.7: Opening up MAX" accompanies it. The Modular blog is the primary source here; we could read its index summaries and the public repository but not the full posts, so read them yourself. The 26.7 summary calls the new MAX License "intentionally permissive" and says you can use, modify, extend and build commercial products with MAX without licensing fees or royalties, and that source for MAX plugins for other chips, including Qualcomm Dragonfly and Google TPU, is planned "in the coming months."

The `modular/modular` repository README gives a clear map of what is available and since when:

- Mojo compiler: since August 2026
- Mojo standard library: since March 2024
- MAX GPU programming abstractions and accelerator library (compute kernels): since May 2025
- MAX AI framework (model authoring in Python): since November 2025
- MAX graph compiler, runtime (`MLRT` ), driver and model pipelines: listed as available now

The README also says "we're actively open-sourcing more of the Modular Platform over time," which tells you the staged approach continues. Our earlier post on the Mojo compiler going open source covers the first half of this story, and the AI chip architectures guide explains why a portable compiler and runtime matter across GPUs, TPUs and accelerators.

## What the MAX License actually says

This is the part the short posts skip. The repository's LICENSE states that Mojo and its contributions are under Apache 2.0 with LLVM Exceptions, while "portions of the MAX source code and its binary distribution are licensed under the MAX License," and the rest under Apache 2.0. The MAX License file is marked as last modified October 21, 2026, a day before the release, and names the licensor as "Modular Inc, a Qualcomm Company."

Key points from the license text, which we read directly:

1. **Grant.** A perpetual, worldwide, non-exclusive, no-charge, royalty-free license to reproduce and prepare derivative works, distribute derivative works in object code, and use documentation internally. Section 1.1 calls it a "Limited Source License."
2. **Downstream terms.** You may license your own derivative work under terms you choose, but any portion of MAX inside it stays under the MAX License, and you must include a human-readable copy of the terms.
3. **Applications.** You may redistribute parts of MAX inside an application, in object code, provided the application has "material additional functionality beyond the included portions of MAX."
4. **Forks.** Section 1.3 allows publishing modified source in a fork of Modular's repository to contribute to a Modular project. Distributing object code and applications stays governed by the grant above.
5. **No substitute built with AI.** Section 1.4(d) says you shall not use MAX as training data, fine-tuning data, or input to any AI system to produce software that reimplements or substitutes for MAX. It adds that using AI tools to read, analyze, improve or explain MAX, or to build software that runs on or interoperates with MAX, is not restricted.
6. **Telemetry.** Section 2.1 says use of MAX may involve collection of telemetry and usage data, which Modular and its affiliates may use "for any purpose", and that Modular owns it.
7. **Trademarks for hosted services.** If you are a legal entity commercially deploying MAX as a hosted or managed AI training or inference service for other legal entities, you must use the Modular Accelerated mark under Modular's brand guidelines and get written approval for materials bearing it.
8. **Patents.** A royalty-free patent license applies, ending if you file patent litigation alleging MAX infringes.

None of this makes MAX unusable. It does make "fully open" imprecise: by common definitions such as the Open Source Definition, a license that restricts what you may do with the code (here, the reimplementation and branding clauses) is source available rather than open source. Both statements are true in different senses, and the license file is what a lawyer will read.

## What this means if you build on it

**If you run inference in-house.** You can inspect and modify the compiler and runtime, and ship your own derivatives in compiled form. Read the telemetry section with your security team, because it applies to your use of MAX.

**If you sell a hosted inference service.** The trademark clause is the one to check. It does not forbid hosting, but it requires prominent Modular branding and approval workflows, which many providers would treat as a business term.

**If you ship an app.** Embedding runtime pieces is allowed when your app has substantial functionality of its own, in object code with the license copy included.

**If you work on a competing runtime.** Do not feed MAX source to a model to produce a lookalike. The clause is explicit, and the exception for interoperability is narrow.

**If you work on kernels in Mojo.** The compiler and standard library remain Apache 2.0, a much simpler position. The README notes that a single Bazel command builds the Mojo compiler and standard library from source.

Compare how other projects frame openness: Kimi K3.1 Pro's promised open weights and Reflection's Beam release under Apache show how model licenses differ from runtime licenses. The same questions apply on the local side, for example llama.cpp-based Windows apps, which sit on a very permissive stack. On hardware diversity, see Cerebras CS-4 for the kind of non-GPU target a portable compiler tries to reach.

## What are developers likely to ask?

**Does "source available" change anything for contributors?** Yes, in a practical way. Because the license lets you publish modified source in a fork of Modular's official repository to contribute upstream, pull requests are welcome, but a private fork you distribute as a competing product is not. The repo also ships contribution guidelines and an AI tool policy file, which suggests Modular expects agent-assisted contributions and wants them disclosed.

**Is it a timeline of steady opening or a sudden flip?** Steady. The README dates show the standard library in March 2024, kernels and GPU abstractions in May 2025, the Python framework in November 2025, the Mojo compiler in August 2026, and now the compiler and runtime for MAX in October 2026. Each step moved the line of what stays closed, and the license wording changed one day before this release.

**Will Mojo and MAX diverge on licensing?** Possibly. Mojo sits on a plain Apache 2.0 with LLVM Exceptions footing, while MAX mixes two licenses. If you only need the language, the simpler license applies.

## What is still unconfirmed

- **Qualcomm's role.** The license calls Modular a Qualcomm company. Modular's blog index lists "Qualcomm to Acquire Modular" (June 24, 2026) and "Qualcomm Completes Acquisition of Modular" (July 29, 2026). We do not describe the deal terms.
- **Performance and hardware support.** The announcement lists CPUs and GPUs. We found no new benchmark numbers tied to the opening, so we are not comparing speed.
- **Exact component boundaries.** The README says "portions" of MAX use the MAX License. Which directories carry it is indicated by per-file headers and the`Licenses` folder; check the specific files you depend on.
- **Lattner's phrase.** The "fully open" line is a social post. It is a fair shorthand for "you can read, change and commercially use it", but not a license term.

## How to check it yourself

1. Read `LICENSE` and`Licenses/MAX_LICENSE` in the modular/modular repository.
2. Open the MAX release list to confirm v26.7 and the changelog.
3. Read the October 22 posts on modular.com/blog.
4. Grep for license headers in the directories you will touch, since Apache and MAX License files can sit side by side.

*Details are accurate as of October 23, 2026, based on Modular's blog index, docs and public repository. Licenses change; read the current file before shipping.*
