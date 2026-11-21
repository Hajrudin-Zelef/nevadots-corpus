---
id: collect-261121-veille-ia/mojo-modular/modular-opens-mojo-and-max-compiler-and-runtime-internals
title: "Modular opens Mojo and MAX compiler and runtime internals"
domain: mojo-modular
role: reference
task: article
actors: []
dates: []
keywords: ["modular", "compiler", "internals", "license", "runtime", "hardware", "code", "according"]
source: https://www.ai-primer.com/engineer/stories/modular-mojo-max-open-stack
source_anchor: ""
source_lines: [1, 73]
section: "Mojo_Modular"
---
## TL;DR

- MAX's graph compiler, MLRT runtime internals and hardware abstraction code are publicly available for development and commercial use, according to the announcement.
- NVIDIA, AMD and Apple support is already public; the hardware update also names Qualcomm, TPUs and Trainium as portability targets.
- MAX's custom license permits royalty-free use but restricts modified-source publication and feeding its code to AI to build replacements. Modular left further opening unspecified.

The license has teeth: B2B hosted inference providers face a branding requirement with prior written approval. The 26.7 release notes also contain a useful debugging detail: the same custom-op declaration works in eager and compiled modes.

## Graph compiler and MLRT

Mojo's compiler and toolchain were already open-sourced in August 2026, according to Modular's October 22 announcement. This release opens the remaining MAX internals.

Modular cofounder Chris Lattner announced the newly public layers and outlined their capabilities:

- **Graph compiler:** automatic kernel fusion and generation.
- **MLRT:** runtime internals.
- **Accelerator programming:** tile-based programming abstractions.
- **Hardware layer:** abstraction logic and CPU/GPU backends.

## MAX License

Existing Apache-licensed components retain their terms, including the Mojo kernel library, accelerator-programming interfaces, Python serving and modeling APIs, and MAX models. The custom MAX License covers the newly exposed internals and distributed MAX binaries, according to Modular's component breakdown.

Lattner invited questions on the forums, where Modular's licensing FAQ details the distribution rules:

- **Commercial binaries:** royalty-free distribution is allowed. Included MAX portions retain the MAX License, and distributions must include a copy. Applications bundling MAX must add material functionality beyond MAX itself.
- **Modified source:** publication is permitted only in a fork of Modular's official repository for contributing changes upstream. The restriction applies to MAX-licensed code, not separately licensed Apache components.
- **Hosted services:** commercial hosted or managed AI training and inference services sold by one legal entity to another must use the “Modular Accelerated” mark and obtain written approval for its use.
- **License changes:** each release retains the terms that accompanied it. Later terms apply to later releases.

Community contributions initially cover bug fixes, with refactors and broader changes planned later, according to the same announcement.

## AI-assisted development

Coding agents have an explicit boundary in the license's AI clause:

- **Allowed:** reading, analyzing, improving or explaining MAX, and developing software that runs on or interoperates with it.
- **Restricted:** using MAX-licensed components as training data, fine-tuning data or AI inputs to generate software that reimplements or substitutes for MAX. The restriction includes replacements containing no literal copies of MAX code.

## Hardware backends

Device extensibility hooks are public alongside reference implementations, according to the 26.7 release notes. Availability breaks down as follows:

- **Public implementations:** NVIDIA, AMD, Apple Silicon and CPUs.
- **Plugin source planned in the coming months:** Qualcomm Dragonfly and Google TPU.
- **Additional portability target:** Trainium appears in Lattner's hardware list without a source-release date.

## Eager mode

Custom ops now run eagerly on a `Tensor` and can be reused unchanged inside a compiled `Module`. Modular says eager mode shares a single code path with the graph compiler in its release notes.

For a slice of Gemma 4 31B, Modular reports approximately **30 seconds to the first token in eager mode**, compared with **110 seconds compiled**. Those figures describe the company's development-loop example.

## Alliance conformance

Modular, now a Qualcomm company, describes its proposed alliance as “LLVM, but for AI,” including conformance testing for compatibility across the ecosystem.

Modular's release announcement ties contributions of engineering resources, compute or early model checkpoints to these promised partner benefits:

- A voice in governance and conformance standards.
- Roadmap input and priority for upstream contributions.
- Early access to model checkpoints.
- Ownership of dedicated hardware backends.
- More permissive licensing.

The FAQ targets a formal launch in early 2027 and says the alliance will offer greater flexibility for organizations maintaining substantial customizations, including proprietary hardware support without upstreaming changes. Lattner said partnership announcements would accompany the launch.

## Telemetry switches

MAX may collect usage telemetry. The FAQ documents two separate controls:

- `MODULAR_TELEMETRY_ENABLED=0` disables Mojo and low-level MAX usage telemetry.
- `MAX_SERVE_DISABLE_TELEMETRY=1` disables MAX model-serving telemetry.
