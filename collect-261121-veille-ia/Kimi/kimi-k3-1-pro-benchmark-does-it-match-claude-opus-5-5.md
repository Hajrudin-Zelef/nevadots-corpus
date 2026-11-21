---
id: collect-261121-veille-ia/kimi/kimi-k3-1-pro-benchmark-does-it-match-claude-opus-5-5
title: "Kimi K3.1 Pro Benchmark: Does It Match Claude Opus 5.5?"
domain: kimi
role: reference
task: article
actors: ["Moonshot AI"]
dates: []
keywords: ["kimi", "opus", "task", "moonshot", "claude", "about", "does", "match"]
source: https://goremy.ai/blog/articles/kimi-k3-1-pro-kingbench-results
source_anchor: ""
source_lines: [1, 135]
section: "Kimi"
---
# Kimi K3.1 Pro Benchmark: Does It Match Claude Opus 5.5?

Kimi K3.1 Pro claims near-Opus results, but a six-task KingBench run scored it 5.37/10. See task scores, cost, and how it compares to rivals.

For Engineering and IT

## Does Kimi K3.1 Pro match Claude Opus 5.5?

Not in this independent test. Moonshot's launch post puts Kimi K3.1 Pro at 65.7% on Terminal Bench 4.0, next to Opus 5.5 at 66.4%. A hands-on run of six KingBench 4 coding tasks scored it 5.37 out of 10, against 5.78 for Opus 5.5. That placed it sixth of 11 models on the tester's chart, behind Opus, GPT6 Astra, GPT 6.1 Soul, Gemini 4 Argon and Haiku 5.5.

## TL;DR

- Moonshot's launch claims put K3.1 Pro **right next to Opus 5.5** , but the KingBench run put it sixth of 11 at 5.37/10.
- The model was **honest about what it skipped** . On the emulator task it said DS and 3DS were not implemented instead of faking them.
- Its best result was the **room designer at 7.4** , and its worst was the emulator at 1.8, because it built one system out of three.
- At official rates the full run cost about **$32** , driven mostly by roughly 110 million cached tokens reread across steps.
- Haiku 5.5 scored slightly higher for about $7.40, and DeepSeek V4.1 Flash scored close for under $2, so **the price-to-score case is weak** .
- Open weights are promised by November 4th, which could make it the **best-scoring open-weight model** on that chart.

## What did Moonshot claim at launch?

Kimi K3.1 Pro is the first model in Moonshot's K3.1 series. It is described as Kimi K3 with a lot more post-training. It is available in the Kimi app, in Kimi Code and through the API. A high-speed version runs two to three times faster. Context stays at 1 million tokens.

The headline numbers from the launch post:

- **Terminal Bench 4.0:** 65.7%, versus 66.4% for Claude Opus 5.5 and 58.2% for GPT6 Astra.
- **Hallucination rate** on Moonshot's hardest factual test: down from 23.4% on K3 to 8.3%.
- **Open weights:** promised by November 4th.

Those are vendor numbers on vendor-chosen tests. They say little about whether a model finishes a large, vague, multi-part project.

## How was K3.1 Pro tested?

The tester ran all six KingBench 4 tasks through Panda Code, an open-source (MIT) desktop app for running coding agents. Each task went to its own Kimi worker running K3.1 Pro at max effort, which is also the setting Moonshot uses for its own benchmarks. Each worker got a fresh project folder and the exact frozen prompt. All six ran in parallel and finished in under an hour, with the fastest task taking about 24 minutes and the slowest about 58. The run hit Moonshot's organization rate limit nine times, and Kimi Code retried each request without trouble.

A separate evaluator graded each task on a frozen copy. It built the project, ran tests, played it in a real browser and fed it bad inputs. It never edited the model's code. The rubric matches the one used for the other models on the chart.

One caveat: these runs did not use the sandbox from older runs. Isolation came from the prompt and Panda Code's write scopes. The tester doesn't expect that to change results, but it is a difference.

## How did K3.1 Pro score on each task?

| Task | Score (out of 10) | Short version | 
|---|---|---|
| Game Boy / DS / 3DS emulator (Rust) | 1.8 | Only the Game Boy part exists | 
| Docker alternative (Go) | 4.1 | Strong image handling, missing core features | 
| 3D room designer | 7.4 | Best result, but below average for the task | 
| Keyboard configuration app | 6.4 | Speaks the real VIA protocol | 
| Pokémon Red | 6.6 | Playable route to Brock, many rough edges | 
| Fitbit desktop app | 5.9 | Polished dashboard, weak auth and import | 

The average is 5.37.

### Emulator: honest but incomplete

K3.1 Pro built only the Game Boy core and said so in its final message. The emulator rejects DS and 3DS files instead of pretending to run them. The core passes all 11 classic CPU instruction tests, renders the standard rendering test pixel-perfect, restores save states to the exact frame and runs around 40 times real speed. A bug in a common interrupt pattern corrupts the stack, though, so a homebrew game crashed on its logo screen. In-game saves only write on a clean close.

### Docker alternative: secure, not finished

The Go tool, krun, pulled Alpine, Nginx, Redis, Ubuntu and BusyBox from Docker Hub, and the unpacked filesystems matched a reference extractor. It blocked all 20 deliberately malicious images (path traversal, symlink tricks, escaping hard links). Some other models on the chart lost points there.

The gaps: older schema 2 images download but fail at container creation, even though the readme says they work. There is no build, exec, port publishing or networking. The prompt asked for something more efficient than Docker, yet there is not a single benchmark. The Linux runtime could not run on the tester's Mac, so that part got source-level credit only.

### Room designer, keyboard app, Pokémon Red, Fitbit

The room designer worked well: 12 furniture types, resizing, retexturing, wall-aware dragging, working save, load and import, and no GPU memory leak after more than 100 edits. It looked like a clean architectural model rather than a realistic room, and new furniture always spawned in the same spot. DeepSeek Flash scored 8.8 on this task and Opus 8.5.

The keyboard app was the surprise. It used correct VIA command IDs and packet sizes, and a keymap save changed exactly the two edited keys. Macro bytes matched QMK's own encoding. Problems included an 11-key definition rendering as 14 keys, a thin keycode list, and a single non-English character in a macro blanking the app. Only Opus and Haiku did better.

Pokémon Red was walkable from the house to Brock without a crash, but a Squirtle jumped from level 5 to 34 before the first gym. The title screen defaults to "new game" even with a save, so one Enter press wipes progress. Astra and Argon scored around 8.5 here.

The Fitbit app is a real Tauri app with a Rust backend and a finished-looking dashboard. It crunched 10 years of daily data in about 3 milliseconds. But it connects by pasting an access token, targets the old Fitbit web API that Google is shutting down, and can't import a real Fitbit export. A partial import quietly erases existing data for those days.

## Where does K3.1 Pro rank against other models?

| Model | KingBench 4 score | 
|---|---|
| Claude Opus 5.5 | 5.78 | 
| GPT6 Astra | 5.65 | 
| GPT 6.1 Soul | 5.50 | 
| Gemini 4 Argon | 5.43 | 
| Haiku 5.5 | 5.42 | 
| **Kimi K3.1 Pro** | **5.37** | 
| Muse Wonderful | 5.30 | 
| DeepSeek V4.1 Flash | 5.17 | 
| GLM 5.3 | 4.58 | 
| Muse Wonder | 4.58 | 
| MiniMax M3.1 Flash | 3.63 | 

The spread among the top six is small. The real finding is that K3.1 Pro sits near Haiku 5.5, not near Opus. The blog's Terminal Bench figure had it well ahead of Astra, while here Astra leads. These are different benchmarks, so that is a contrast, not a contradiction.

## How much does Kimi K3.1 Pro cost?

Official API pricing per million tokens:

|  | Input | Cached input | Output | 
|---|---|---|---|
| K3.1 Pro | $2 | $0.20 | $10 | 
| K3.1 Pro high-speed | $4 | $0.40 (implied by doubling) | $20 | 
| K3 (previous) | $3 | $0.30 | $15 | 

The six tasks used about 112 million tokens over 793 requests. At official rates that is roughly $32, or about $42 on high-speed. Output was under 700,000 tokens, around $7. The bulk, about $22, came from roughly 110 million cached tokens that the agent rereads on every step.

For comparison on the same run set: Haiku 5.5 scored 5.42 for about $7.40, DeepSeek V4.1 Flash scored 5.17 for under $2, GPT 6.1 Soul scored 5.50 for at least $6, Gemini 4 Argon scored 5.43 for at least $10, and Opus 5.5 cost $46 or more. K3.1 Pro costs more than four times as much as Haiku for a lower score.

## Is Kimi K3.1 Pro worth using?

On pay-per-token API pricing, this run says no. Haiku scores a bit higher for a fraction of the cost, and DeepSeek Flash comes close for almost nothing. Opus costs more but delivers more.

It makes more sense in two cases. One is a Kimi subscription, where token count doesn't hit your wallet. The other is a tool already tuned for Kimi, like Panda Code, which talks to the Kimi Code CLI over ACP. Its strengths are real: it reports what it didn't build, it handled the malicious-image tests, and it got the keyboard protocol right. Its weaknesses are finishing the full scope of a request and its heavy context rereading.

## What about the open weights?

K3 is already open-weight on Hugging Face under Moonshot's license, which is described as close to MIT with extra conditions for very large companies. Moonshot says full K3.1 Pro weights arrive by November 4th. Other providers could then host it, which may push prices down. If the weights match the API model, it would be the top-scoring open-weight model on this chart, ahead of DeepSeek Flash and GLM.

Two caveats. The model is in the 3 trillion parameter class, so self-hosting means a rack of GPUs, not a laptop. And Moonshot says it removed cyber-related training data from the open release, so the download may not behave exactly like the API.

## Frequently Asked Questions

### What did Kimi K3.1 Pro score on KingBench 4?

It scored 5.37 out of 10 (53.7%) across six tasks, ranking sixth of 11 models. Opus 5.5 scored 5.78, and Haiku 5.5 scored 5.42.

### Is the 65.7% Terminal Bench 4.0 score wrong?

Nothing here shows it is wrong. It is Moonshot's own figure on a different benchmark, and the KingBench run measures large, open-ended projects. The two results answer different questions.

### Why was the run so expensive?

Mostly cached input. The agent reread about 110 million cached tokens across steps, costing roughly $22 of the $32 total. Output was under 700,000 tokens, about $7.

### When will Kimi K3.1 Pro weights be available?

Moonshot says the full open weights are coming by November 4th. Running it yourself would require serious GPU hardware, and the open release may differ slightly from the API version.

How we source, write, and correct our articles. Editorial standards
