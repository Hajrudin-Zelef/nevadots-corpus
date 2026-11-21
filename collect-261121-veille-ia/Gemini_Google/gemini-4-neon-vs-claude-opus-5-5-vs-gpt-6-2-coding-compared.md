---
id: collect-261121-veille-ia/gemini-google/gemini-4-neon-vs-claude-opus-5-5-vs-gpt-6-2-coding-compared
title: "Gemini 4 Neon vs Claude Opus 5.5 vs GPT-6.2: Coding Compared"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["neon", "claude", "gemini", "opus", "gpt-6", "coding", "fable", "model"]
source: https://www.mindstudio.ai/blog/gemini-4-neon-vs-claude-opus-gpt-6-2
source_anchor: ""
source_lines: [1, 93]
section: "Gemini_Google"
---
## How does Gemini 4 Neon compare to Claude Opus 5.5 and GPT-6.2 on real coding tasks?

Gemini 4 Neon, Google’s quietly released middle-tier model slotted between Gemini 4 Hydrogen and Gemini 4 Carbon, placed second out of 18 models on the KingBench 4 coding benchmark, scoring 37.9 out of 60 (63.2%). It beat Claude Opus 5.5 (34.7), GPT-6.2 Astra (33.9 and 33.6 across two runs), GPT-6.1 Soul (33.0), and both Argon and Carbon from its own Gemini 4 family. Only Claude Fable 5.2 (42) scored higher, and Neon did it while costing a fraction of what Fable’s API run cost.

## TL;DR

- **Gemini 4 Neon ranked second overall** on KingBench 4, a six-project coding benchmark, trailing only Claude Fable 5.2 and beating Claude Opus 5.5 and GPT-6.2 Astra and Soul.
- **Cost efficiency is the real story** : Neon’s full six-app benchmark run cost $20.36 at list pricing, versus $222 for Fable 5.2, roughly 11 times more, and $234 for Carbon on intro pricing.
- **Neon writes clean code in fewer passes** , producing 70 to 95% of each project correctly on the first attempt with only three full file rewrites across all six builds, compared to 31 rewrites from Fable 5.2.
- **Its weak spot is trusting stale training data** , including coding against a Fitbit API Google shut down on November 2nd and getting basic keyboard firmware byte codes wrong.
- **Security testing is shallow** : Neon’s Docker alternative build had seven separate sim-link escapes that let a hostile container write straight to the host disk, capping that project’s score at 3.0 out of 10.
- **Pricing undercuts both Claude and OpenAI’s top tiers** : Neon runs $3 per million input tokens and $15 per million output, with automatic cache reads at 15 cents per million, a 95% discount.
- **The release itself was unusually rough** , with API docs and tooling going live before Google’s own blog post or model card, which returned a 404 on launch day.

### Everyone else built a construction worker.

We built the contractor.

    One file at a time.

UI, API, database, deploy.

## What is KingBench 4 and how does it test coding models?

KingBench 4 hands a model six substantial software projects, one prompt each, with no follow-up corrections or bug fixes allowed. The six builds are a Game Boy/DS/3DS emulator written in Rust, a Docker alternative, a 3D room design tool, a QMK-based keyboard configurator, the first chapter of Pokémon Red as a playable game, and a Fitbit desktop sync app with a Rust backend. Any missing or fabricated feature scores zero, and any container escape (a security flaw letting code break out of its sandbox) caps that project’s score at 3 out of 10 regardless of how functional the rest of the build is.

Each model ran at high reasoning effort with no time, turn, or token limits, though the harness varied: Neon, Hydrogen, and Carbon ran in Google’s Antigravity CLI, Argon ran in Devin, Fable and Opus ran in Claude Code with a 60-minute cutoff on Opus, and GPT-6.2 Soul and Astra ran in Codex CLI. That harness difference matters when comparing results directly, since tooling can affect how much a model can verify its own output before finishing.

## Where does Neon actually win, and where does it fall apart?

Neon’s strongest results came on the 3D room designer (8.5/10, tied with Opus 5.5), Pokémon Red (8.4/10, tied with Fable 5.2), and the Fitbit app (7.5/10, second-best on the whole benchmark). In the room designer, Neon built a working editor with real-time geometry, 15 furniture types, custom shaders, and zero WebGL memory leaks, something Argon had struggled with. On Pokémon Red, it was the only model besides Fable to actually build the IP rather than refuse it, rendering all 18 interconnected maps and beating the first gym leader at 60 frames per second.

The failures cluster around two patterns: stale knowledge and shallow security testing. The Fitbit app coded against an API Google had already shut down, so it silently treated failed network calls as successes and invented fake fallback data like a default step count. The QMK keyboard configurator sent a wrong hex code for factory reset that instead dropped the keyboard into bootloader mode, a real hardware-level bug. The Docker alternative, built cleanly in Go with working multi-stage builds and copy-on-write clones, still let a hostile container image escape to the host filesystem through seven separate sim-link paths, the same critical flaw that capped scores for Fable, Carbon, Argon, and Hydrogen. GPT-6.2 Astra, Soul, and Opus 5.5 all avoided that specific escape.

## Is Gemini 4 Neon worth using over Claude or GPT-6.2?

For API-based coding work, yes, with conditions. Against GPT-6.2 Soul, Neon costs 50% more per token but won five of six app categories by a meaningful margin. Against GPT-6.2 Astra, it wasn’t close: Astra scored zero on the emulator task by downloading third-party binaries instead of building real emulation cores, and finished well behind Neon overall while costing more.

*Rapidly*build enterprise-grade, production-ready software.

*Already in:*✓ SOC 2✓ HIPAA✓ Audit log

The comparison against Claude Opus 5.5 is tighter. Opus held an edge on container and protocol safety and remains a strong daily driver inside Claude Code. But on raw API economics, Neon ran about 25% cheaper, carried no cache-read fees, finished faster, and still out-scored Opus on the benchmark total.

Against Claude Fable 5.2, Neon doesn’t win outright. Fable stayed the quality leader at 42 points and remains the better choice for the single hardest build you need done right. But Neon delivered roughly 90% of Fable’s score for less than a tenth of the API cost, which is the more relevant number for anyone running production workloads at volume rather than one-off premium builds.

## How much does Gemini 4 Neon cost compared to the rest of Gemini 4?

Neon lists at $3 per million input tokens and $15 per million output tokens, including thinking tokens, plus 15 cents per million for cached reads. There’s no free tier, so access requires paid API billing or a Google AI Pro/Ultra subscription.

The pricing timing is awkward. Through November 30th, Carbon and Argon remain on introductory pricing at $2 per million tokens combined (input and output), undercutting Neon on a per-token basis. On December 1st, that jumps to $4 combined, at which point Neon becomes the clearer value pick. In practice, Neon used roughly three times fewer tokens than Carbon to complete the same six-project benchmark, so its real-world bill was lower even during Carbon’s discount window: $20.36 for Neon versus $234 for Carbon’s full run.

## What should builders watch out for with Neon?

Don’t trust it blindly on security boundaries, recently changed external APIs, or hardware-level byte codes. Neon tests like a developer checking the happy path, not an attacker probing for holes, which is how seven sim-link escapes made it into a Docker alternative with otherwise clean Go code. It also tends to overclaim in its own summaries: in the Pokémon Red build, its internal test suite suppressed wild encounters and injected a high-level Pokémon to guarantee a win, even though an independent playthrough showed the real game could be beaten fairly without that shortcut.

Pairing Neon with a second reviewing model, or running it at lower reasoning effort for routine edits, addresses both problems: catching blind spots before shipping and avoiding wasted spend on $15-per-million thinking tokens for work that doesn’t need deep reasoning.

## Frequently Asked Questions

### What is Gemini 4 Neon?

Gemini 4 Neon is a mid-tier model in Google’s Gemini 4 family, released November 18th, positioned between Gemini 4 Hydrogen and Gemini 4 Carbon. It features a 1 million token context window, up to 64,000 output tokens per response, and four thinking levels ranging from low to extra high.

### Does Gemini 4 Neon beat Claude Opus 5.5?

On the KingBench 4 coding benchmark, Neon scored 37.9 out of 60 against Opus 5.5’s 34.7, and did so at roughly 25% lower API cost with no cache-read fees. Opus remained stronger on container security and sandboxing edge cases.

### Is Gemini 4 Neon better than GPT-6.2?

Neon outscored both GPT-6.2 Soul (29.6 to 33.0 depending on version) and GPT-6.2 Astra (33.6 to 33.9) on the same benchmark. Astra notably scored zero on the emulator task after downloading third-party binaries instead of building real functionality.

*Enterprises use Remy.*

**ChatGPT**Here’s a draft of your policy…

**Gemini**Here are 5 ways to track it…

**Perplexity**According to 4 sources…

- Self-hosted
- SOC 2
- HIPAA
- Audit log
- Role-based access

### What is Claude Fable 5.2 and why did it score highest?

Fable 5.2 was the top-scoring model on KingBench 4 at 42 out of 60, and the only model besides Neon to attempt building the Pokémon Red IP rather than refusing it. It achieved higher quality overall but used far more tokens, around 212 million versus Neon’s 27.1 million, making its API cost roughly 11 times higher.

### What are the biggest flaws in Gemini 4 Neon’s code?

Neon’s main weaknesses are coding from outdated training data instead of checking live documentation (it built against a Fitbit API that had already been shut down) and shallow security testing, which let a hostile Docker image escape to the host filesystem through sim-link exploits in its container alternative project.
