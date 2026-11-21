---
id: collect-261121-veille-ia/glm-zhipu/glm-5-3-what-the-zhipu-signals-actually-say
title: "GLM-5.3: What the Zhipu Signals Actually Say"
domain: glm-zhipu
role: reference
task: article
actors: ["Zhipu AI"]
dates: []
keywords: ["glm-5", "zhipu", "august", "community", "model", "actually", "base", "every"]
source: https://kie.ai/blog/glm-5-3-zhipu-next-model
source_anchor: ""
source_lines: [1, 112]
section: "GLM_Zhipu"
---
TLDRPost-launch read of GLM-5.3: confirmed specs and benchmarks from Zhipu, availability across gateways, and the questions builders should still track.

## GLM-5.3: What the Zhipu Community Signals Actually Say

The community called it "cooking" for weeks. On August 14, 2026, Zhipu shipped it. GLM-5.3 is now live — with an official technical blog, a full benchmark table, and API access — and the surprise is what did *not* change: it runs on the exact same base model as GLM-5.2, with every gain coming from post-training alone.

**TLDR** GLM-5.3 launched on August 14, 2026, and the API went live on August 18. It uses the same base as GLM-5.2 — roughly 743B total parameters (not the ">1T" pre-launch rumor) in a Mixture-of-Experts design activating ~40B per inference — and Z.ai says every gain came from scaling post-training, worth roughly a 50% coding improvement. The name that started as Ivan Fioravanti's "GLM 5.3 is cooking" post on July 12 and the skeptical Teortaxes follow-up turned out to be the real product. It shipped text-only; the vision that dominated Jie Tang's June poll did not land in this release, and open weights are still pending a post-launch safety hold.

## Key Takeaways

- GLM-5.3 launched August 14, 2026, with an official Z.ai technical blog; the API went live August 18.
- The "GLM-5.3" name that circulated in community X posts turned out to be the shipping product.
- GLM-5.3 uses the same base as GLM-5.2 — every gain came from post-training, not a new base model.
- Confirmed specs: ~743B total MoE parameters, ~40B activated per inference, 1M-token context.
- Terminal-Bench 3.0 jumped from 4.6 to 28.3 and DeepSWE v1.1 from 46.2 to 66.9 over GLM-5.2.
- It shipped text-only; native vision — the top community ask — did not land, and open weights are still pending.

## What Was Actually Seen

The pre-launch trail for GLM-5.3 was thin, but it pointed the right way — and the launch has since confirmed it.

On July 12, 2026, Ivan Fioravanti — a regular voice in the local-LLM community — posted "GLM 5.3 is cooking, are we ready?". The post drew 644 views and four likes. It carried no attached screenshot of a leak, no HuggingFace commit hash, no repo reference. It was a vibe check from a well-known account — and it landed.

Two days later, on July 14, Teortaxes replied to a related image with "GLM 5.3 coming so quickly would be surprising, I'm *not* ready". That post included two attached images and hit 843 views. The skepticism was misplaced: by early August, documentation surfaced on ZCode, a GLM-5.3 commit appeared in Zhipu's official Java SDK repo before being deleted, and by August 14 the model was live.

That is the shape of the run-up. Community anticipation, then confirmed leaks — SDK commits, ZCode docs — and then a full launch. No countdown was needed; the model dropped roughly a week after the "1 week to GLM-5.3" chatter, with the API following four days after that.

## The GLM-5.2 Baseline That Sets Expectations

The most striking fact about GLM-5.3 is how much it leans on GLM-5.2. Understanding the launch requires the prior flagship as a floor, because GLM-5.3 *is* that floor with more post-training on top.

Per Tech Times' reporting on Z.ai's public numbers, GLM-5.2 is a Mixture-of-Experts model with a 1-million-token context window. It scored 81.0 on Terminal-Bench 2.1 — within four points of Claude Opus 4.8's 85.0 — and 62.1 on SWE-bench Pro, ahead of GPT-5.5 at 58.6. The model shipped with MIT-licensed open weights on HuggingFace on June 17.

The architecture built on DeepSeek Sparse Attention with a technique Zhipu calls IndexShare, which runs the sparse-attention indexer once every four transformer layers rather than at every layer. That is the efficiency mechanism that makes the 1M-token context economically viable — and because GLM-5.3 reuses the same base, it inherits all of it unchanged.

One nuance that shaped the vision debate: GLM-5.2 is text-only, and GLM-5.3 stayed text-only. Zhipu keeps vision in a separate GLM-V family (GLM-5V-Turbo, GLM-4.6V, GLM-4.5V, GLM-OCR). Feed either model a screenshot and it returns an error. That gap is the single thing the community kept asking to close — and it remains open.

## Why the Timing Matters

The GLM release cadence proved fast enough that "GLM-5.3 soon" was right. GLM-5 → GLM-5.1 → GLM-5.2 all shipped within months per Z.ai's documentation summarized by SeaWork's own tracking, and GLM-5.3 arrived on August 14, 2026, roughly two months after GLM-5.2. GLM-5.2 itself landed 24 hours after Anthropic withdrew Claude Fable 5 under a US Commerce Department export-control directive, per Tech Times' background. That timing was widely read as intentional positioning.

Zhipu is clearly running a rolling-release strategy against US frontier labs, and GLM-5.3 fits the pattern: a point release within roughly two months, this time built entirely on post-training rather than a bigger base-model jump. The rumors of a larger ">1T" release were wrong on scale — the actual model is the same ~743B base — but right that another release was imminent.

The point that the launch settled: the improvement path was post-training, not parameter growth. Zhipu's founder Jie Tang has since framed GLM-5.3 explicitly as a controlled experiment on exactly that claim.

## Community Read: What the June 29 Poll Told Us

The single most-watched pre-launch signal was Jie Tang's June 29 poll. Per Tech Times, Tang asked on X: "Any new features we must have in the next version of glm?" The thread hit 466,000+ views, 3,400+ likes, and 1,400+ replies within days.

The answer was near-unanimous: vision. Developers wanted GLM to natively process screenshots, PDFs, UI designs, and error messages without piping them through Qwen-VL first. Zixuan Li, a Z.ai team lead, publicly acknowledged in the thread that "vision is taking over the comment section."

GLM-5.3 did not deliver it. The model shipped text-only, and the launch instead poured its gains into coding and long-horizon agentic work. Native vision remains an open request — community speculation now ties an unreleased "Ox Alpha" checkpoint to a possible GLM-5.3 vision variant, but Zhipu has confirmed no such multimodal release.

Secondary asks from the poll, summarized in Tech Times' recap and cross-referenced with ExplainX's community writeup, included shorter default reasoning chains, smaller MoE variants runnable on consumer hardware, day-one inference stack support (llama.cpp, vLLM, SGLang), and computer-use capabilities. GLM-5.3's headline gains landed elsewhere — in coding and cyber-defense — so those asks, vision included, carry into whatever comes next.

## GLM-5.3 vs Claude Opus 4.8: What the Reports Say

The community keeps benchmarking GLM releases against Claude's frontier tier, and with GLM-5.3 launched, there are now published numbers to compare — from Z.ai's own release table.

- **Confirmed release status** : Both are shipped and publicly available. GLM-5.3 launched August 14, 2026, with API access live since August 18.
- **Terminal-Bench 2.1** : GLM-5.3 scores 88.2, ahead of Claude Opus 4.8's 85.0, per Z.ai's published comparison table.
- **Terminal-Bench 3.0** : GLM-5.3 scores 28.3, up from GLM-5.2's 4.6, versus Opus 4.8's 21.1 — though both trail Fable 5 (33.7) and GPT-5.6 Sol (34.6).
- **Native vision** : Claude Opus 4.8 handles images in one pass per Tech Times' context. GLM-5.3 shipped text-only; a vision variant is unconfirmed.
- **License** : Claude Opus 4.8 is proprietary API only. GLM-5.3 is an open-weights model, but the public weights are still pending a post-launch safety hold, whereas GLM-5.2's MIT weights are already out.

The honest read: on measured coding capability, GLM-5.3 now edges Opus 4.8 on Terminal-Bench and closes much of the remaining distance to the very top tier — remarkable for a model that added no new parameters. What it still lacks against Opus is native multimodality.

## What We Know vs. What We Don't

The known-versus-unknown split is the point of this post. Here is the honest ledger, updated for the launch.

**What we know:**

- Zhipu launched GLM-5.3 on August 14, 2026, with an official technical blog; the API went live August 18.
- The "GLM-5.3" name, seeded by Ivan Fioravanti's July 12 post and reinforced by Teortaxes on July 14, turned out to be the shipping product.
- GLM-5.3 uses the same base model as GLM-5.2 — every gain came from post-training, per Z.ai.
- Confirmed specs: roughly 743B total parameters in a MoE design, ~40B activated per inference, 1M-token context — not the pre-launch ">1T" rumor.
- Z.ai credits roughly one additional month of long-horizon reinforcement learning for a ~50% coding improvement on its in-house Code Bench.
- Terminal-Bench 3.0 rose from 4.6 to 28.3 and DeepSWE v1.1 from 46.2 to 66.9 over GLM-5.2, per Z.ai and OpenRouter.
- GLM-5.3 is state of the art on CyberGym (84.5) for vulnerability discovery, per Z.ai's published numbers.
- Pricing was stated to match GLM-5.2, with access via the Z.ai API, ZCode, and partner gateways including OpenRouter and ChatLLM.
- ZCode is running a promotion giving 50,000 new users 100M free GLM-5.3 tokens each, through August 23 at 6 PM PT.

**What we don't know:**

- Whether and when the open weights drop — Z.ai said about two weeks after launch, pending safety evaluation, and as of August 22 that release has not landed.
- Whether GLM-5.3 will gain native vision; a rumored "Ox Alpha" checkpoint is speculated to be a GLM-5.3 vision variant, but Zhipu has not confirmed it.
- Whether the eventual open weights will carry the MIT license GLM-5.2 uses; Zhipu has not restated the license for 5.3.
- Whether the independently reported benchmark figures (cybersecurity CVE-rediscovery, Artificial Analysis Intelligence Index, KernelBench) hold up under third-party reproduction — several remain single-source.
- Whether a single authoritative price schedule exists across all endpoints; the official post said "matches GLM-5.2," while provider-specific figures vary by gateway.

## What Builders Should Do Today

With GLM-5.3 live, the posture shifts from watchful to hands-on — but with a few caveats.

First, you can wire it into a real workflow now. The API is live, ZCode is offering free tokens through August 23, and the model is available across gateways including OpenRouter and ChatLLM. Point a genuine coding or long-horizon agentic task at it rather than relying on the vendor's comparison chart.

Second, run your own coding evaluation against both GLM-5.2 and GLM-5.3. Because 5.3 shares 5.2's base and differs only in post-training, a side-by-side is the cleanest way to measure what an extra month of RL actually bought on your workload — the reported ~50% coding gain may or may not show up on your tasks.

Third, treat vision as still unresolved. GLM-5.3 shipped text-only despite the community poll, and Tang has architectural reasons — he has publicly said text-based reasoning is what raises the upper bound of machine intelligence per Tech Times' recap — to keep the flagship text-only. If your workflow needs vision, plan for a two-model pipeline through the GLM-V family or a separate vision model, and don't count on an unconfirmed "Ox Alpha" variant.

## The Week Ahead: Signals to Watch

Three concrete signals will tell you where GLM-5.3 goes from here:

- **Watch the Zhipu / Z.ai HuggingFace organization** for the open-weights upload. Z.ai said the weights would follow roughly two weeks after the August 14 launch, pending safety hardening — that drop is the next milestone.
- **Watch the zai-org/GLM-5 GitHub repository** for issue activity referencing the weights release or a vision variant — repository maintainers often surface release context in issue replies.
- **Pin Jie Tang's X account and the @Zai_org handle** for confirmation on the "Ox Alpha" speculation and any multimodal GLM-5.3. Tang has been the source of the last several GLM release framings and will likely clarify directly.

If the open weights land on schedule, the story becomes a self-hosting and quantization guide. If a confirmed vision variant appears, that is the next deep dive. Either way, the leak cycle is over — GLM-5.3 is a shipped, benchmarked model now.

Try related models on Kie.ai: Claude Opus 4.8, GPT-6 Astra and Gemini 3 Pro.
