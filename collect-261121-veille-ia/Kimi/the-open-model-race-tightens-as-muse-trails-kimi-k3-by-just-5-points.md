---
id: collect-261121-veille-ia/kimi/the-open-model-race-tightens-as-muse-trails-kimi-k3-by-just-5-points
title: "The Open-Model Race Tightens as Muse Trails Kimi K3 by Just 5 Points"
domain: kimi
role: reference
task: article
actors: ["Moonshot AI"]
dates: []
keywords: ["deepseek", "kimi", "muse", "federal", "security", "index", "together", "model"]
source: https://www.beri.net/article/kimi-qwen-alternatives-us-open-weights-muse-spark-open-reflection-beam-gpt-oss-chinese-origin-policy-2026
source_anchor: ""
source_lines: [1, 143]
section: "Kimi"
---
**If security or a customer has told you to pull Kimi, Qwen, GLM or DeepSeek weights, swap the big model for Meta's Muse Spark Open; in defense, nuclear or critical infrastructure, use Reflection's Beam.** Muse is the strongest US open-weight model on the Artificial Analysis Intelligence Index at 39, five points behind Kimi K3 and seven behind the top Chinese entry. Its licence bans military and nuclear uses and the operation of critical infrastructure. Beam is Apache 2.0 with no such strings, but it scores 30, and that gap is large enough to show up in coding-agent work. For small, cheap workloads, gpt-oss-120b and Gemma 4 cover what Qwen's small models and DeepSeek Flash were doing.

The workload used for every cost figure below: 1 billion input tokens and 200 million output tokens a month, uncached, on Together AI serverless list prices from Together's pricing page, checked October 19, 2026. Index scores are Artificial Analysis Intelligence Index v4.3.2, checked the same day.

| Model | Origin | Licence | AA Index | Together price (in / out per 1M) | Monthly cost at our workload | Verdict | 
|---|---|---|---|---|---|---|
| Kimi K3 (what you are replacing) | China | Kimi K3 License | 44 | $3.00 / $15.00 | $6,000 | Baseline | 
| GLM-5.3 | China | GLM-5.3 License | 45 | $1.40 / $4.40 | $2,280 | Baseline | 
| DeepSeek V4.1 Flash 1017 | China | MIT | 45 | $0.30 / $1.20 | $540 | Named in a federal order | 
| Muse Spark Open | US (Meta) | Muse Model License | 39 | $1.25 / $4.25 | $2,100 | Default replacement | 
| Reflection Beam 1.0 | US | Apache 2.0 | 30 | $0.40 / $1.20 | $640 | Pick for defense and critical infrastructure | 
| Nemotron 3 Ultra | US (NVIDIA) | OpenMDW-1.1 | Not on current index page | Not on Together serverless | Self-host only | Strong if you own Blackwell | 
| gpt-oss-120b | US (OpenAI) | Apache 2.0 | 12 | $0.15 / $0.60 | $270 | Small-model swap | 
| Gemma 4 31B | US (Google) | Apache 2.0 | 15 | $0.39 / $0.97 | $584 | Small-model swap | 
| Mistral Large 4 (preview) | France | Not published | 38 at launch | $1.36 / $4.18 list (Mistral API) | $2,196 | Wait for the licence | 
| Llama 4 | US (Meta) | Llama 4 Community License | Not listed | Not on Together serverless | n/a | Loser | 

## What Is Actually Restricted Today?

Government networks are restricted, and only DeepSeek is named at the federal level. No US law bars a private company from downloading or running Chinese-origin weights.

The biggest change landed this week. DHS has ordered federal civilian agencies to stop buying DeepSeek products and remove the ones they have, under the Federal Acquisition Supply Chain Security Act of 2018; the notice went up for public inspection on October 19, 2026, for publication on October 20, per PYMNTS. Security Magazine quotes Exabeam's Gabrielle Hempel saying the restriction "extends beyond DeepSeek itself to third-party products and services developed using its implementations." Tech Insider's reading is that the order does not reach state governments or private businesses, and that contracting officers will be checking task orders for subcontracted AI services built on DeepSeek. If you sell to a federal civilian agency, that is now your problem even if you only fine-tuned a DeepSeek checkpoint and renamed it.

The order names DeepSeek. Kimi, Qwen and GLM are not in it.

States moved first, and only on their own equipment. Texas barred DeepSeek from state-issued devices on January 31, 2025, New York followed on February 10 and Virginia on February 11, all by executive action, per the Fordham IP Law Journal. These cover government devices and networks, not vendors.

The broad bill has not moved. The No Adversarial AI Act (S.2177) would bar executive agencies from AI developed in China, Russia, Iran and North Korea, with the Federal Acquisition Security Council keeping the list (ThePrint). Its last recorded action is a June 25, 2025 referral to the Senate Homeland Security committee.

So most of the pressure you are feeling is contractual: a customer's security questionnaire or your own CISO's policy. The security case behind those policies has evidence. A CNAS fellow writing in Just Security notes that the government's CAISI has published five assessments of Chinese open-weight models, and that a DeepSeek model complied with every hacking and scam request CAISI tested. The same piece argues against blanket bans, because downloaded weights cannot be recalled.

## Is the Licence the Problem?

For internal use, no. The Chinese flagships and Meta's flagship now carry the same kind of custom licence, with nearly the same revenue trigger, and all of them exempt internal use.

The older assumption that Chinese weights ship under MIT or Apache 2.0 now holds only for some of them. DeepSeek still does: the V4-Pro model card says "the model weights are licensed under the MIT License," and Artificial Analysis lists V4.1 Flash 1017 as MIT. The other flagships moved to custom terms:

- Kimi K3 ships under the Kimi K3 License, which requires a separate deal with Moonshot if you run a model-as-a-service business above $20 million in revenue, per Simon Willison.
- Qwen3.8's 2.4T checkpoint uses the Qwen3.8-Max License: a separate licence for model-as-a-service businesses above $50 million in 12-month revenue, and a model-name display rule past 100 million monthly users, with internal use carved out (Let's Data Science).
- GLM-5.3 requires a Z.ai security review only for inference providers above $10 billion in revenue, and GLM-5.3-Flash is plain MIT (noze).

Meta's Muse Model License has the same $20 million model-as-a-service trigger as Kimi K3, plus a "Built with Muse Spark Open" attribution rule (model card) and a usage policy that Kimi's licence does not have. Swapping Kimi K3 for Muse makes your licence more restrictive.

The provenance question is separate: where the weights came from, who trained them, and whether your customer's policy names the country. That is what you are actually being asked to fix, so say so in the memo to security. A licence review will not answer it.

## How Big Is the Capability Gap?

On the shared index, the best US open model trails the best Chinese one by seven points, and the Apache options trail by 16 to 34. Artificial Analysis ranks MiMo-V2.6-Pro at 46, DeepSeek V4.1 Flash 1017 and GLM-5.3 at 45, and Kimi K3 at 44. The first US entry is Muse Spark Open at 39, then Beam at 30. gpt-oss-120b scores 12 and Gemma 4 31B scores 15.

The task-level numbers say where the gap hurts. On Meta's own card, Muse scores 76.4 on Terminal-Bench 2.1 against Kimi K3's 85.0, and 58 on SWE-Atlas-QnA against 66. It leads all three Chinese models on MCP-Atlas tool use, 89.2 against 82.3 for Kimi K3 (Hugging Face). Beam's gap is wider on code: 44.4 on DeepSWE v1.1 against Kimi K3's 68.0 in Reflection's launch table, which we broke down row by row. If your Kimi deployment was a coding agent, Beam will feel like a downgrade. If it was RAG or tool calling, Muse may match it.

Nemotron 3 Ultra is the hard one to place. Artificial Analysis gave it 47.7 at its June 4 release, on an earlier index version that does not compare with today's numbers, and it is not on the current open-weights page. NVIDIA's own card compares it with MiniMax-M2.7, GLM-5.1, Kimi-K2.6, Qwen-3.5 and DeepSeek V4: it trails them on SWE-Bench Verified (70.7 against 73.5 to 76.2) and leads every one with a reported score on 1M-token RULER, at 94.7 (model card). Those comparisons are against the previous generation of Chinese models. Run it on your own tasks before you trust either number.

## Which Model Should Replace Yours?

Match the replacement to the job the Chinese model was doing, because no single US model covers both ends.

### Muse Spark Open Replaces the Flagships

Muse Spark Open is the closest swap for Kimi K3, GLM-5.3 or Qwen3.8-Max. It is an 816B mixture-of-experts model with 42B active and a 1M-token context, and on our workload it costs $2,100 a month against Kimi K3's $6,000. It is also cheaper per finished task: Artificial Analysis puts it at $0.84 per index task against $2.00 for Kimi K3. Hosted APIs are on Together, Fireworks AI and others.

Who should not pick it: anyone who operates critical infrastructure, transport or heavy machinery, or touches "military, warfare, nuclear industries or applications," which the usage policy bans outright. We covered those clauses in Meta's Muse Spark Open License Bans Running Critical Infrastructure. That rules out defense agencies, their suppliers and many utilities. Also skip it if you resell a raw endpoint above $20 million in revenue, or if self-hosting is the point: the NVFP4 build needs at least 700GB of VRAM (model card).

### Reflection Beam Wins When the Licence Must Be Clean

Reflection Beam 1.0 is the pick when the licence has to be clean. Reflection released the weights on October 16 under Apache 2.0, in BF16, FP8 and NVFP4, with on-premises builds from Dell and HPE and hosting on AWS, Google Cloud, Microsoft Foundry and Oracle. At 501B total parameters, the FP8 weights come to roughly 500GB, which fits one eight-GPU node of 80GB cards. Apache 2.0 has no field-of-use list, no attribution label and no revenue trigger. On our workload it costs $640 a month.

Who should not pick it: teams whose Kimi or GLM deployment is an autonomous coding agent. A 24-point DeepSWE gap is too large for prompt work to close. Also note that Reflection was reported in acquisition talks with Nvidia; the Apache weights you mirror today stay yours, but the roadmap may not.

### Nemotron 3 Ultra Needs Blackwell

Nemotron 3 Ultra suits teams that already own Blackwell hardware and need long-context retrieval. Its OpenMDW-1.1 licence is "ready for commercial and non-commercial use." The recommended single node is eight B200s with about 1.5TB of HBM, and TensorRT-LLM supports it only on Blackwell. Skip it if you want a serverless API: Together does not list it.

### gpt-oss-120b and Gemma 4 Cover the Small Models

gpt-oss-120b and Gemma 4 replace small Qwen models and DeepSeek Flash on classification, extraction and routing. gpt-oss-120b is Apache 2.0, 117B total with 5.1B active, and built to "fit into a single 80GB GPU" (Hugging Face); it costs $270 a month on our workload. Gemma 4 31B is the first Gemma under Apache 2.0, a dense model with a 256K context. Neither belongs in an agentic coding seat. gpt-oss-120b dates from August 5, 2025 and OpenAI has not shipped a successor, so plan for it to age.

### Granite and Mistral Large 4 Are Side Bets

IBM Granite 4.2 comes in 3B, 8B and 30B sizes under Apache 2.0, released August 25, 2026 (IntuitionLabs). Use it where IBM's support contract matters more than capability. It is not a Kimi replacement.

Mistral Large 4 is the European candidate, and the one to wait on. The preview scored 38 at launch and costs $1.36 input and $4.18 output per million at list, currently discounted to $0.68 and $2.09 (Mistral docs). The model page shows no licence, and Let's Data Science relays a third-party expectation of a custom licence rather than the Apache 2.0 used for Large 3. Do not commit until the licence file exists; the weights are due at the end of October.

### The Loser: Llama 4

Llama 4 is the one to drop from the shortlist. Its licence requires any model you fine-tune from it or its outputs to have a name that begins with "Llama," a "Built with Llama" label, and a separate Meta licence above 700 million monthly users (Llama 4 Community License). It is absent from Artificial Analysis's current open-weights ranking, and Meta's own better model, Muse Spark Open, has a licence that is no more restrictive for most buyers. Mistral Large 3 belongs in the same bin: Artificial Analysis marks it deprecated, at an index score of 9.

## What Does the Migration Actually Cost?

Most of the cost is engineering time on evaluation, prompts and fine-tunes. At list prices the token bill goes down for every replacement in the table.

Re-running your eval set comes first. Public benchmarks tell you where to look, and your own 100 to 200 tasks tell you whether a five-point index gap matters. Our vendor benchmark guide has the replication method. Run the new model through the same host you will use in production, since results for the same open model differ by provider; we measured that in Together vs Fireworks vs Groq.

Prompts do not transfer cleanly. Qwen models kept rewarding few-shot examples after GPT-family models stopped needing them, which we tested in Few-Shot Stopped Paying on GPT-4o. Qwen Still Wants It. Expect to strip or rebuild example blocks.

Fine-tunes have to be retrained. A LoRA adapter is tied to the base model's weights, so an adapter trained on Qwen or Kimi does nothing on Muse or Beam. Budget the GPU hours and the labelling time again, and check the new licence on derivatives: Muse's attribution rule follows any model you distill from its outputs.

Hardware changes with the size class. Kimi K3 needs about 1.4TB resident at its native MXFP4 (Digital Applied). Muse needs 700GB at NVFP4, Beam about 500GB at FP8, and gpt-oss-120b a single 80GB GPU. Moving from Kimi K3 to Beam can free most of a node, and moving down to gpt-oss frees almost all of it.

## Does a US Host Satisfy the Policy?

Sometimes, and it depends on which of two policies you have. Origin of weights says nothing about where inference runs or who holds your data.

A policy written about data flow (no prompts to servers in China, no Chinese-headquartered processor) is satisfied by a US provider serving the Chinese model. Together serves Kimi K3, GLM-5.3, Qwen3.8 and DeepSeek V4.1 on its serverless menu. Under that policy you may not need to migrate at all, only to move the endpoint.

A policy written about provenance (no model developed by a Chinese entity) is not satisfied by any host. The DHS order looks like this kind of rule. Exabeam's Hempel reads it as reaching "third-party products and services developed using" DeepSeek's implementations (Security Magazine), and early coverage summarises it the same way, though none of it quotes the order's text on that point. Read that way, a self-hosted DeepSeek copy in your own data centre fails the test just as the DeepSeek API does.

Get the policy text before you pick a model. If it is written about data location, the fix may be an endpoint change and no migration at all.

## The Decision: What Predicts Regret

Five things decide whether you will be redoing this in a year.

1. The policy's wording. A rule about origin of weights, one about location of inference and a named vendor list each lead to a different model.
2. Your sector against the usage policy. Utilities, transport operators, defense and nuclear-adjacent buyers cannot use Muse, so their choice is Beam or Nemotron.
3. The gap on your own eval, measured per finished task. A model with a lower index score can still cost less per task, as Muse does against Kimi K3.
4. The hardware you already own. Nemotron 3 Ultra makes sense on Blackwell and is expensive to stand up anywhere else. Beam is the only US option above an index score of 25 that fits one node of 80GB cards.
5. Vendor continuity. Mirror the weights you approve; our weight-mirroring playbook covers how.

What changes the answer: Mistral Large 4's licence (due with the weights at the end of October), any extension of the DHS order beyond DeepSeek, and the No Adversarial AI Act moving out of committee.

## What to Do Before Your Next Customer Review

**This Week:**

1. Get the restriction in writing from security or the customer, and classify it as provenance, data location or a named list. If it is data location, price moving your Kimi or GLM endpoint to a US host first.
2. If you sell to a federal civilian agency, inventory every product and subcontracted service that uses a DeepSeek model, including fine-tunes and renamed checkpoints.
3. Have legal read Exhibit A of the Muse Model License against your industry before any engineer downloads it.

**This Month:**

1. Run your 100 to 200 task eval against Muse Spark Open and Beam on the same host, and record cost per finished task next to the score.
2. Move the small-model workloads to gpt-oss-120b or Gemma 4 31B, since that part of the migration is cheap and the risk is low.
3. Mirror the approved weights into your own registry and pin the hash.

**Before Renewal:**

1. Retrain your fine-tunes on the chosen base, and update the attribution labels the new licence requires.
2. Re-check Mistral Large 4 once its licence is published, and add it to the eval if it is Apache 2.0.

## The Bottom Line

A policy-compliant replacement costs you index points and some coding quality, and at list prices it lowers the token bill. Muse Spark Open is the closest match and cheaper per task than Kimi K3. Beam carries the cleanest licence among the larger models. Most commercial teams will be fine with Muse for the hard work and gpt-oss for the cheap work; defense, nuclear and critical-infrastructure buyers need Beam. Before you migrate anything, ask security whether the rule is about where the weights came from or where your prompts go.
