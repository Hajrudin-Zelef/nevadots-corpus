---
id: collect-261121-veille-ia/qwen/alibaba-qwen4-max-complex-professional-tasks-206b2071
title: "Alibaba qwen4 max complex professional tasks"
domain: qwen
role: reference
task: article
actors: ["Alibaba"]
dates: []
keywords: ["alibaba", "qwen4-max", "model", "november", "preview", "qwen4-max-preview", "token", "through"]
source: ""
source_anchor: ""
source_lines: [1, 48]
section: "Qwen"
---
On November 15–16, 2026, Qwen’s official release blog and Alibaba Cloud’s own documentation detailed two related but distinct models. Qwen4-Max-Preview is a Token Plan-only preview that runs from November 11 through November 25, 2026 (UTC+8). Separately, the production Qwen4-Max model now has its own Model Studio page, and the preview page says the production version is available and recommended.
That’s good news if you just want to know whether this is real, and it is, in two different forms. Here’s how the confirmed facts and the circulating claims separate out.
What Happened With Qwen4-Max
Alibaba opened Qwen4-Max-Preview on November 11, 2026. Its documentation says the preview is available through Token Plan only, pay-as-you-go access isn’t supported, and the preview model will be retired after November 25, 2026.
The production Qwen4-Max is a separate listing in Alibaba Cloud Model Studio, whose documentation was updated on November 16, 2026. The two are related but not identical: one is a time-boxed preview, the other is the pay-as-you-go model with its own regional pricing and limits.
The New Stack also covered the proprietary launch alongside independent Artificial Analysis scoring.
Access differs by model. Qwen4-Max is available through Alibaba Cloud Model Studio’s API in six listed regions: Singapore, China (Beijing), Hong Kong, Germany (Frankfurt), US (Virginia), and Japan (Tokyo). Qwen4-Max-Preview, by contrast, needs a Token Plan Personal or Team subscription and its dedicated API key, and that access ends when the preview closes on November 25.
The Qwen family itself is real. Alibaba has shipped models called Qwen, Qwen2, Qwen2.5, and Qwen3, plus a hosted Qwen-Max tier accessed through Alibaba’s cloud service.
Qwen4-Max and Qwen4-Max-Preview now extend that naming to Alibaba’s newest release and its preview counterpart.
Verified specifications from Qwen’s blog and Alibaba Cloud Model Studio include:
- Architecture and availability: Qwen4-Max is a sparse Mixture-of-Experts model with 2.4 trillion total parameters, 71B active parameters per forward pass, and a 328B N-gram embedding table. It launches as a proprietary API via Alibaba Cloud Model Studio and Vercel AI Gateway, while smaller open-weight Qwen4 releases are planned later.
- Two distinct release states: Qwen4-Max-Preview (Token Plan-only, November 11 through November 25) and Qwen4-Max (pay-as-you-go API in Model Studio).
- Context and output: Qwen4-Max has a 1,000,000-token context window, up to 131,072 output tokens in non-thinking mode, and 262,144 tokens shared by thinking and the final answer. It accepts text, image, and video input and returns text.
- Pricing: $1.50 per million input tokens and $4.50 per million output tokens in the Singapore (international) region, and $1.413 and $4.24 in the Beijing, Hong Kong, Frankfurt, Virginia, and Tokyo regions.
How It Stacks Up
Alibaba published side-by-side evaluations on the Qwen4-Max blog alongside independent tracking from Artificial Analysis. Qwen4-Max improves noticeably over Qwen3.8-Max and wins on legal and bioinformatics agent benchmarks, while trailing Claude and OpenAI’s flagship reasoning models on composite coding and terminal tasks.
Official Alibaba benchmark comparison (qwen.ai)
| Benchmark | Qwen4-Max | Qwen3.8-Max-0902 | Claude Opus 5.5 | GPT-6 Astra | Kimi K3.1 | 
|---|---|---|---|---|---|
| DeepSWE v1.1 (agentic coding) | 75.7 | 69.3 | 74.2 | 74.1 | 75.5 | 
| CodeArena (Elo, web dev) | 1750 | 1676 | 1816 | 1763 | 1686 | 
| Terminal-Bench 4.0 (terminal coding) | 48.5 | 38.9 | 59.6 | 59.1 | 67.2 | 
| AA-Briefcase v1.1 (Elo, knowledge work) | 1686 | 1605 | 1801 | 1569 | 1674 | 
| HLAB-Vals-1.1 (legal agent work) | 21.3 | 15.4 | 9.2 | 7.1 | 11.7 | 
| Automationbench-AA (SaaS workflow) | 72.5 | 56.2 | 69.5 | 68.5 | 73.3 | 
| BioMysteryBench (bioinformatics) | 85.0 | 84.1 | 81.9 | 78.9 | 80.6 | 
| Humanity’s Last Exam (reasoning) | 51.7 | 43.9 | 59.8 | 53.6 | 51.1 | 
| AgentArena (net improvement) | 7.5 | — | 18.2 | 13.9 | 3.9 | 
Alibaba also highlights efficiency gains on Work360 (52.69 vs. 47.74 for Qwen3.8-Max) with 24%–35% fewer output tokens at high reasoning efforts, cutting average task cost by 62% ($4.43 to $1.67) and trimming Terminal-Bench 4.0 token consumption by 48%–49%.
Independent Artificial Analysis Intelligence Index
| Model | Artificial Analysis Index | Rank (of 81) | Avg Cost / Task | Output Speed | 
|---|---|---|---|---|
| Claude Fable 5.2 (Max) | 65 | #1 | $26.54 | 110 tok/s | 
| Gemini 4 Carbon (High) | 60 | #4 | $5.48 | 62 tok/s | 
| Claude Opus 5.5 (Max) | 59 | #5 | $10.04 | 117 tok/s | 
| GPT-6 Astra (Max) | 57 | #8 | $6.24 | 42 tok/s | 
| Qwen4-Max | 43 | #38 | $3.27 | 53 tok/s | 
In independent testing highlighted by The New Stack and Artificial Analysis, Qwen4-Max clears the 41-point category median at 43 while averaging $3.27 per task and 127M output tokens. However, it still trails top US frontier models by 14 to 22 index points, making its lower API price ($1.50 input / $4.50 output in Singapore) its sharpest competitive hook.
The Reaction
If you come across a reposted Qwen4-Max spec sheet, check it against Alibaba Cloud’s own Model Studio pages, and treat any number that doesn’t match as unconfirmed.
A roughly two-week preview raises a fair question about whether it’s worth testing seriously. That applies to Qwen4-Max-Preview specifically, since the production Qwen4-Max isn’t on the same clock.
Our Take
If you rely on Claude Fable 5.2, Gemini 4 Carbon, or GPT-6 Astra for hard reasoning or terminal-heavy coding, Qwen4-Max’s score of 43 on the Artificial Analysis Intelligence Index (versus 57–65 for those leaders) means there is no performance reason to switch today. Where it stands out is cost efficiency — roughly half Gemini 4 Carbon’s per-task cost and nearly 8x cheaper per task than Claude Fable 5.2 — alongside strong showing on legal and bioinformatics agent suites.
Developers who already use Alibaba’s Qwen-Max tier have the most reason to watch this. Compare Alibaba’s published regional pricing and output limits for Qwen4-Max against your current setup before moving anything, and remember the Qwen4-Max-Preview access window closes November 25.
Wrapping Up
Alibaba’s Qwen models are real, and so is Qwen4-Max. The production model is listed in Alibaba Cloud Model Studio, while the separate Qwen4-Max-Preview remains Token Plan-only through November 25.
Terms can change, so your one next step is easy: check the Qwen4-Max Model Studio page directly instead of trusting reposted spec sheets.
If you do try the preview before it closes, keep your current AI tool set up as your default. That way, when the preview ends, nothing breaks and you won’t have to rebuild your workflow from scratch.
