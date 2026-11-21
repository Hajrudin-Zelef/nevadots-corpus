---
id: collect-261121-veille-ia/muse/muse-spark-open-review-benchmarks-pricing-and-license-catches
title: "Muse Spark Open review: benchmarks, pricing, and license catches"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: ["2026-10-15"]
keywords: ["muse", "model", "meta", "spark", "open", "license", "eesel", "first"]
source: https://www.eesel.ai/blog/meta-muse-spark-open-review
source_anchor: ""
source_lines: [1, 174]
section: "Muse"
---
Muse Spark Open is Meta's first open-weight Muse model, released on October 15, 2026: 816B total parameters, 42B active, a 1M-token context window, and text, image, video and audio input. It is excellent at calling tools and very hard to jailbreak, leading Meta's own comparison table on MCP-Atlas (`89.2`) and JobBench (`61.8`).

The catch is twofold. It is not the smartest open model: it scores `39` on the Artificial Analysis Intelligence Index, behind GLM 5.3 (`45`), Kimi K3 (`44`) and Qwen 3.8 (`40`), and it trails both leaders on coding. And it is priced like a closed model: every host charges `$1.25` in and `$4.25` out per million tokens, the same as Meta's closed Muse Spark 1.3 API, because the license gives Meta a say over large hosts.

My verdict: `3.5/5`. Pick it if you need self-hostable weights for a tool-heavy agent and want conservative safety behavior. Skip it if you want the best open coding model or the cheapest tokens. And if what you actually want is an agent answering support tickets, the model is the smallest part of the job; eesel's AI helpdesk teammate handles the rest.

## What is Meta Muse Spark Open?

I build AI agents for a living at eesel, and the first thing I do with any new model is read the model card before the launch post. The Muse Spark Open model card on Hugging Face describes an 816B-parameter mixture-of-experts model that activates 42B parameters per token, with 256 experts and 8 active per token, a 1M-token context window, and a 1.8B-parameter vision encoder bolted on for images and video.

It is the open-weight sibling of the closed Muse Spark line, not a separate family. The same Muse name also covers Muse Code, Meta's coding harness, and the Muse personal agent.

Meta's research post calls it "the most capable US open-weight model to date" and says it ships with extra safety post-training because, once the weights are out, nobody can add guardrails on the server side. The developer post adds a detail that matters for my world: Meta trained it for long-horizon agent work "such as coding and customer service agents."

A few practical specs from the card:

| Spec | Muse Spark Open | 
|---|---|
| Parameters | 816B total, 42B active | 
| Context | 1M tokens | 
| Input | Text, image, video, audio | 
| Output | Text | 
| Reasoning effort | low, medium, high, xhigh (default xhigh) | 
| Weights | BF16 (about 1.6TB) and NVFP4 (about 530GB) | 
| Release | October 15, 2026 | 

One thing to note: reasoning is always on. Groq's model page says it "always reasons before answering," and the default effort is the most expensive one, xhigh. If you want short, cheap replies, you have to ask for `low` or `medium` yourself.

## How I evaluated it

I haven't run this model on a support queue for a year, and I won't pretend I have. This review is built from Meta's own benchmark table, the independent Artificial Analysis results, the live price and speed data on every launch host, the full license text, and what developers said in the first 72 hours on Hacker News. Where Meta's numbers and independent numbers disagree, I say so.

The lens I bring is the one I use at eesel when picking models for the helpdesk teammate: does it call tools reliably, does it stay inside the rules, and what does a month of real traffic cost. I've spent years at eesel watching confident models give wrong answers on live queues, which is why every eesel rollout gets simulated against historical tickets first. A benchmark win only matters if it shows up there, which is the whole idea behind AI teammates that are tested on your data before they go live.

## Where Muse Spark Open leads

Meta compares the model against seven other open-weight models: Inkling, Nemotron 3 Ultra, Reflection Beam, Mistral Large 4, Qwen 3.8, GLM 5.3 and Kimi K3. Here is the scorecard as Meta published it.

The wins are real and they cluster in one place: agents that call tools.

- **MCP-Atlas `89.2`** , the top score in the table. This tests calling tools exposed through MCP servers, the plumbing most agents now use to reach a helpdesk, CRM or database.
- **JobBench `61.8`** , edging GLM 5.3's`61.4` and well ahead of Kimi K3's`54.3` .
- **HLE-Diamond `26.1`** without tools and**AA-Omniscience `25`** , the best in the set on hard knowledge questions where making things up is penalized.
- **Multimodal** : it leads ZeroBench (`50.0` ) and SimpleVQA (`73.6` ) against the only other model that supports those tests, Kimi K3.

The two launch partners Meta quoted line up with that picture.

"Muse Spark Open consistently selected the right next tool and kept its responses compact, exactly the decisiveness production agents need."

Zimu Li, Member of Technical Staff at Factory, quoted in Meta's developer post


Independent data agrees on efficiency. Artificial Analysis measured `110M` output tokens to finish its whole index, under the `130M` median, and clocked `130.7` tokens per second. That matters because its closed sibling went the other way: as I covered in my Muse Spark 1.3 review, 1.3 burned through `120M` tokens and drew complaints about verbosity.

## Where it trails

The same table that crowns it on tool use shows it losing the categories most developers shop on.

- **Overall intelligence.** On the Artificial Analysis Intelligence Index row of Meta's own table, it scores`39` , behind GLM 5.3 (`45` ), Kimi K3 (`44` ) and Qwen 3.8 (`40` ), and one point ahead of Mistral Large 4 (`38` ). Artificial Analysis ranks it`#10` of 120 models it tracks.
- **Coding.** On DeepSWE 1.1 it scores`60` against Kimi K3's`68` and GLM 5.3's`67` . On Terminal-Bench 2.1 it scores`76.4` against Kimi K3's`85.0` . Even Reflection Beam beats it there with`80.1` .
- **Real-world work tasks.** On GDPval-AA v2 it scores`1401` , below Mistral Large 4 (`1424` ), Kimi K3 (`1537` ), Qwen 3.8 (`1613` ) and GLM 5.3 (`1653` ).

Developers noticed. The most-cited early impression on the launch thread was a short one:

"Feels dumber than GLM 5.3 (only open model I use extensively). But is really fast (fireworks.ai)."


Another commenter put it between its two closed siblings: "somewhat worse benchmarks than Muse Spark 1.3, closer to 1.2" (modeless, Hacker News). That matches what the numbers say. If your main job is writing code, Kimi K3 or GLM 5.3 is the better open pick today.

## The safety trade-off nobody mentions

Meta leads with safety, and the headline number is striking. On StrongREJECT v2, an adaptive jailbreak test where lower is better, Muse Spark Open scores `0.5`. Kimi K3 scores `66.6`, GLM 5.3 `62.9`, Qwen 3.8 `65.6`. The card also reports `99.2%` refusal of cyber jailbreaks on Meta's internal benchmark and a `98.1%` honesty rate on MASK.

But the same table carries a second number most coverage skipped. FORTRESS reports two scores: an adversarial risk score (ARS, how often it gives harmful help) and an over-refusal score (ORS, how often it refuses things it should answer). Lower is better on both.

Muse Spark Open's ORS is `17.4`. Kimi K3's is `2.7`, GLM 5.3's is `3.2`. In plain terms: it is the hardest model in the table to trick and the most likely to say no to a fair request. For a customer service agent, that is a real trade. A refund question phrased awkwardly, or a customer describing a medical device fault, is exactly the kind of benign message an over-cautious model can decline. I would test it on your own messy tickets before trusting the safety headline in either direction.

## Pricing: open weights, closed-model rates

Here is the part that surprised me most. Every launch host on OpenRouter charges exactly the same rate.

| Provider | Input / 1M | Output / 1M | Cached input / 1M | Notes | 
|---|---|---|---|---|
| Baseten | $1.25 | $4.25 | $0.15 | NVFP4, 1M context, about 200 tokens/sec, see Baseten pricing | 
| Fireworks AI | $1.25 | $4.25 | $0.15 | Serverless LoRA fine-tuning, see Fireworks pricing | 
| Together AI | $1.25 | $4.25 | $0.15 | Dedicated endpoints with 99.9% SLA, see Together pricing | 
| Modal | $1.25 | $4.25 | $0.15 | Shared or dedicated endpoints | 
| Nebius Token Factory | $1.25 | $4.25 | n/a |  | 
| Groq | Not listed | Not listed | n/a | Limited preview, 131K context, ~350+ tokens/sec | 

That `$1.25 / $4.25` rate is identical to what Meta charges for its closed Muse Spark 1.3 at xhigh. Artificial Analysis flags it directly: the model is "particularly expensive when comparing to other open weight models of similar size," against a median of `$0.43` in and `$1.68` out. Its cost to run one index task is `$0.84`.

Why would five competing hosts land on the same price? The license is the likely reason. One developer on the launch thread spelled it out:

"The model license requires large inference providers to contact Meta separately to obtain a license to serve the model... The usual effect of separate licensing is decreased price competition from public providers."


The fair counterpoint from the same thread: a fixed price also keeps hosts from serving a broken or degraded version to undercut each other (dannyw, Hacker News). Both can be true.

What does that cost in practice? A rough worked example for an agent that reads a lot and writes a little (output priced at `$4.25` per million):

| Monthly volume | Input | Output | Approx. monthly bill | 
|---|---|---|---|
| Small pilot | 10M tokens | 2M tokens | $21 | 
| One busy agent | 100M tokens | 20M tokens | $210 | 
| Team-wide rollout | 1B tokens | 200M tokens | $2,100 | 

Caching repeated context at `$0.15` instead of `$1.25` can cut the input side a lot if your agent reuses the same knowledge base on every call. Remember that xhigh reasoning is the default, so drop to `medium` for short replies.

## Running it yourself: what the hardware really takes

The pitch that got the most attention was Apple's: run a frontier-class model on your desk. Apple's MLX write-up is honest about what that means. The NVFP4 weights take about `493GB`, and with working memory on top, "the model cannot fit on a single machine."

Apple's measured decode speeds at an 8,192-token context:

| Setup | Decode speed | 
|---|---|
| 2 Mac Studios (M5 Ultra), pipeline split | 15.74 tokens/sec | 
| 2 Mac Studios (M5 Ultra), tensor split | 24.99 tokens/sec | 
| 4 Mac Studios (M5 Ultra), tensor split | 36.73 tokens/sec | 

The two-machine setup needs the 512GB M5 Ultra, which Apple's store lists as "coming late October," with no price shown yet. M5 Ultra configurations start at `$5,499` with much less memory. One commenter summed up the reaction:

"A single Mac Studio w/ M5 Ultra and half that RAM (256 GB) is around $11k, and the 512 GB option isn't even available yet. So.... yeah."


On GPUs, the model card asks for at least `700GB` of VRAM for NVFP4 and `2TB` for BF16; Meta's developer post says NVFP4 serves on four Blackwell Ultra GPUs or eight H200s. Unsloth's 2-bit build gets it down to roughly `300GB` of combined memory, at some quality cost.

To Meta's credit, day-one support is unusually complete: SGLang, vLLM, TensorRT-LLM, Transformers, MLX, llama.cpp and Unsloth all worked at launch, with fine-tuning through TorchTitan, Fireworks and Together. As one developer noted, Meta "did the actual work of testing and documenting deployments on several platforms" (bahmboo, Hacker News).

So who should self-host? The answer from the thread matches mine: teams with a contractual or legal reason to keep data on their own hardware (mbreese, Hacker News). For everyone else, at `$210` a month for a busy agent, a hosted API stays cheaper than a two-Mac setup that commenters already price above `$20,000` for a long time.

## The license fine print

The Muse Model License is permissive on the surface: free use, fine-tuning, distillation and resale. The conditions that matter:

1. **Attribution in your product.** You must "prominently display 'Built with Muse Spark Open'" in the UI, docs, website or settings of any product using it or anything derived from it, including models trained on its outputs.
2. **A $20 million line for hosts.** If you sell model access as a service and your company's revenue tops`$20 million` in any 12 months, you need a separate written agreement with Meta first.
3. **No under-18 use.** The usage policy says the model "is not intended for individuals under the age of 18," and deployers carry the responsibility if minors can reach it.
4. **A broad usage policy.** It bars unlicensed professional practice (legal, medical, financial), using it to process sensitive personal data without the right to, and presenting its outputs as human-written.

That fourth point matters for support teams in regulated industries. An agent that drafts replies for a health or finance company needs a human-in-the-loop design anyway, and Meta's own card recommends "human-in-the-loop confirmation for irreversible actions." Whether it is "open source" is a fair debate; on the launch thread several developers called it "open-weight" at best (isityettime, Hacker News).

## Muse Spark Open vs the alternatives

| Model | AA Index (Meta's table) | DeepSWE 1.1 | MCP-Atlas | StrongREJECT (lower is better) | Best for | 
|---|---|---|---|---|---|
| Muse Spark Open | 39 | 60 | 89.2 | 0.5 | Tool-heavy agents, cautious deployments | 
| Kimi K3 | 44 | 68 | 82.3 | 66.6 | Open coding and terminal work | 
| GLM 5.3 | 45 | 67 | 84.2 | 62.9 | Best overall open score | 
| Qwen 3.8 | 40 | 57 | 84.5 | 65.6 | Broad general use | 
| Mistral Large 4 | 38 | 62 | n/a | n/a | EU-hosted deployments | 

If you can use a closed model, the gap widens. Meta's own frontier tier is covered in my Muse Wonder review, and Claude Opus 5.5 gets the same treatment in my Opus 5.5 review. Both score well above `39` on the same index.

If price matters more than tool use, start with my DeepSeek V4 Flash review. Qwen 3.8 is the other open model worth a look, covered in my Qwen 3.8 review.

## Verdict: who should use Muse Spark Open

**Rating: `3.5/5`.**

- **Pick it** if you need downloadable weights, your agent lives or dies on calling the right tool, and you would rather it refuse too often than too rarely. The MCP scores and the safety tuning are real strengths.
- **Skip it** if you mainly write code (Kimi K3 or GLM 5.3), want the cheapest open tokens (the`$1.25 / $4.25` floor is set), or expected to run it on one machine.
- **Test before trusting** the over-refusal rate on your own traffic.

The bigger lesson for support teams is that the model is rarely what breaks. In eesel's own sales calls, the failure I hear about most is a bot that answered real customers with invented details when the knowledge base had nothing relevant. Another buyer, a technical evaluator at a hardware company, asked straight out whether the AI would fall back on general web knowledge when it didn't know, and whether that could be turned off. Those are retrieval, guardrail and handoff problems. Swapping in a smarter or safer model does not fix them on its own. My guide on automating support with AI agents walks through the layers that do.

## Try eesel for your helpdesk

If you were looking at Muse Spark Open because you want an AI agent working your support queue, eesel's AI helpdesk teammate skips the GPU shopping. It plugs into Zendesk, Freshdesk, Gorgias and more in minutes, learns from your past tickets and help center, and gets simulated against your historical tickets before it replies to a single customer. You decide which tickets it answers, which it drafts as internal notes, and when it hands off to a person.

If you would rather wire a model in yourself, my guide to connecting Claude to your helpdesk shows the work involved. Meta's own route is covered in Muse for customer support.

Plans start at `$299` a month on the eesel pricing page, and you can try it free first. Try eesel and see what it would have said on last month's tickets.
