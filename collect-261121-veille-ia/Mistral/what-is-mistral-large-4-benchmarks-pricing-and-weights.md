---
id: collect-261121-veille-ia/mistral/what-is-mistral-large-4-benchmarks-pricing-and-weights
title: "What Is Mistral Large 4? Benchmarks, Pricing, and Weights"
domain: mistral
role: reference
task: article
actors: ["Mistral AI"]
dates: []
keywords: ["mistral", "model", "weights", "large", "october", "license", "billion", "open-weight"]
source: https://www.lindy.ai/blog/mistral-large-4
source_anchor: ""
source_lines: [1, 373]
section: "Mistral"
---
Mistral released the weights for Mistral Large 4 on October 29, three weeks after launching it as a paid preview nicknamed "le Chonk."

The model you can download is a better one than the preview. Mistral kept training it for those three weeks and now calls it the best open-weight model for coding, with some of the strongest cyber-defense scores of any open model.

I went through Mistral's announcement, model card, license file, and pricing page, then checked each claim against Artificial Analysis and Vals AI on November 3. Here's what Mistral Large 4 is, how it scores outside Mistral's own charts, what it costs, and what it takes to run the open weights yourself.

**TL;DR:**

- **What it is:** Mistral AI's largest model so far, a natively multimodal mixture-of-experts model aimed at coding agents, business workflows, document and image work, and cyber defense.
- **Open weights:** released October 29, 2026, on Hugging Face as mistralai/Mistral-Large-4-1T-A52B, plus a smaller NVFP4 checkpoint for NVIDIA Blackwell GPUs.
- **License:** the Mistral Ecosystem Open-Weight License, a modified MIT license. Commercial use is allowed, but companies that run a model-as-a-service business need a separate license from Mistral.
- **Size:** about 1 trillion total parameters (1.05 trillion on the model card), with 52 billion active per token and a 1.6-billion-parameter vision encoder.
- **Context window:** 1 million tokens.
- **Price:** $0.70 per million input tokens, $0.07 per million cached input tokens, and $2.10 per million output tokens on Mistral's API.
- **How good it is:** 26 on Artificial Analysis's new Intelligence Index v5, which ranks it 7th among open-weight models and first among those built outside China, but far behind closed leaders like Claude Opus 5.5 (58.4). Its best results are in coding agents, cyber defense, and legal work.

## What is Mistral Large 4?

**Mistral Large 4 is the flagship open-weight model from Mistral AI,** first released as an API preview on October 6, 2026, with downloadable weights following on October 29. It's built to handle coding, agentic workflows, and image understanding in one model.

Mistral's launch post calls it "our largest and most capable model to date." The company trained it from scratch on 3,800 NVIDIA Grace Blackwell GPUs in its own European datacenters.

A big share of the training data was multilingual, covering more than 160 languages, including every official language of the European Union.

The weights Mistral released on October 29 come from the same model after three more weeks of reinforcement learning, and Mistral's open-weight announcement says "gains continue to accelerate." Mistral's docs still label the API version a public preview, so expect more updates.

**The specs, from Mistral's** **model card****, Hugging Face page, and announcements:**

| 🔧 Spec | 📋 Mistral Large 4 | 
|---|---|
| Made by | Mistral AI | 
| Released | October 6, 2026 (API preview, v26.10); open weights October 29, 2026 | 
| API model ID | mistral-large-4 (or mistral-large-4-0) | 
| Architecture | Granular mixture-of-experts (128 experts, 4 active per token, plus 1 shared) | 
| Total parameters | 1 trillion (1.05 trillion on the model card) | 
| Active parameters | 52 billion per token | 
| Vision encoder | 1.6 billion parameters | 
| Inputs and output | Text and images in, text out | 
| Context window | 1 million tokens | 
| Reasoning settings | Low, medium, or high (default) | 
| API features | Function calling, structured outputs, document Q&A, batch, and the Agents API | 
| Price | $0.70 input, $0.07 cached input, $2.10 output per 1M tokens | 
| Open weights | Yes, on Hugging Face (FP8 and NVFP4 checkpoints) | 
| License | Mistral Ecosystem Open-Weight License (modified MIT) | 

### 49 billion or 52 billion active parameters?

**Both numbers come from Mistral.** Its Hugging Face page says Large 4 uses 49 billion active parameters per token, or 52 billion if you count the embedding and output layers.

The launch post said 49 billion for its first day and a half, then Mistral switched it to 52 billion overnight between October 7 and 8, without a note. That's why a lot of launch coverage still lists 49 billion, while Artificial Analysis and the repo name (A52B) use 52 billion.

If "active parameters" is new to you, think of a hospital with a thousand specialists where each patient only sees a few. The whole building has to exist (that's the 1 trillion), but each visit only uses a small team (the 52 billion), which keeps answers faster and cheaper than the size suggests. In Large 4's case, each token gets routed to 4 of 128 expert blocks plus one shared expert.

### Why is it called "le Chonk"?

**"Le Chonk" is a nod to a fan meme.** In June, after Mistral renamed its Le Chat assistant to Vibe, fans on X and Reddit invented a fake Mistral model called "Le Chaton Fat" (roughly "the fat kitten"), complete with joke benchmarks, as Decrypt reported.

CEO Arthur Mensch played along, posting on X that "It's actually le gros chaton," French for "the big kitten." Four months later, the launch post introduced the real model as "unofficially ML4, very officially: le Chonk."

"Chonky" is informal for plump or chubby in an endearing way, usually describing an animal, per Merriam-Webster, and it can also just mean bulky or oversize. For a 1-trillion-parameter model, the name lands as a joke about size too.

### How it compares to Mistral Large 3

**Mistral Large 4 is a much bigger model than Large 3.** Large 3, released in December 2025, has 675 billion total parameters, 41 billion active, and a 256K context window, according to its model card.

The jump showed up in independent scores right away. On the older version of Artificial Analysis's Intelligence Index, Large 3 scored 9 and the Large 4 preview scored 38 when I checked on October 15.

It costs more, but the gap has shrunk. Large 3 is $0.50 per million input tokens and $1.50 per million output tokens, so Large 4 is 1.4 times the price.

Large 3 is also on its way out. Mistral's models page lists both Large 3 and Mistral Medium 3.5 as deprecated since October 31, 2026, with retirement set for April 30, 2027, and names Large 4 as the replacement for both.

## What changed since the preview

**The open-weight Large 4 is a stronger and leaner model than the one Mistral previewed on October 6.** Mistral spent three more weeks on reinforcement learning, and its October 29 post lists these changes:

- **Better terminal coding:** Terminal-Bench 4.0 rose 13 points to 41% on Artificial Analysis's coding agent setup, with 30% fewer output tokens.
- **Mistral-reported top score on a new coding test:** 19.7% on Cognition's private FrontierCode 2.0, which checks whether a repository maintainer would merge the agent's change.
- **Coding agent parity with GLM-5.3:** 48% on Artificial Analysis's Coding Agent Index, effectively tied with GLM-5.3, the top open-weight model on that index.
- **Cyber defense:** a score of 51 on Artificial Analysis's Cyber Index, with a 96.6% average refusal rate on malicious cyber requests.
- **Safer agents:** 97.1% resistance to attacks on the B3 agent security benchmark, up from 93.3% for the preview.
- **New reasoning settings:** low, medium, and high, where low replaces the old "none" option.

The model also writes less than it used to. Artificial Analysis counted 110 million output tokens to run its full Intelligence Index, a bit above the 100 million median for similar open models, down from 150 million for the preview.

The price changed too. The preview launched at a $1.36 / $4.18 list price with a 50% launch discount that ended around October 20. With the open-weight release, Mistral cut the price to $0.70 input and $2.10 output per million tokens, about half the preview's list price.

## Mistral Large 4 benchmarks: Mistral's numbers vs. independent tests

**Mistral Large 4 looks like the open-weight coding leader on Mistral's charts and like an upper-mid open model on broad independent leaderboards.**

### What Mistral claims

Here are the headline numbers from Mistral's October 29 post (plus one legal claim from its October 6 launch post), with what I found when I checked each one on November 3:

| 📊 Benchmark | 🧪 What it tests | 🏆 Mistral's number | 🔍 Independent check | 
|---|---|---|---|
| AA Coding Agent Index | Coding agents on three software tasks | 48%, tied with GLM-5.3 | 48.3% vs. GLM-5.3's 48.9% (AA) | 
| Terminal-Bench 4.0 | Multi-step terminal work | 41% | 41.4% in Mistral's own agent (AA), 24.7% in AA's standard test, 26.3% (Vals AI) | 
| FrontierCode 2.0 | Merge-ready code changes | 19.7%, top score | 15.0% in AA's run, behind GLM-5.3 (21.6%) and DeepSeek V4.1 Flash (28.5%) | 
| AA Cyber Index | Finding, reproducing, and patching security flaws | 51 | 50.9, 6th among publicly available models | 
| Malicious cyber refusals | Saying no to attack requests | 96.6% | Mistral's own run | 
| B3 agent security | Resisting prompt injection and data leaks | 97.1% | Mistral's own run | 
| Finch | Finance spreadsheet work | 73.3% | Mistral's own run | 
| BrowseComp | Web research | 84.5% | Mistral's own run | 
| Harvey Legal Agent | Legal agent tasks | Beats all open models (October 6 launch post) | 15.8%, 3rd of 16 open-weight models (Vals AI) | 

Mistral also ran a blind coding review with Surge AI, where graders compared answers side by side without knowing which model wrote them. Graders preferred Large 4 over GLM-5.3, Kimi K3, and DeepSeek V4.1 Flash, and preferred only Claude Opus 5.5 over it.

### What independent testing shows

**On** **Artificial Analysis****, Mistral Large 4 scores 26 on the Intelligence Index v5,** a blend of nine tests covering agentic work, coding, science, document reasoning, and knowledge.

Artificial Analysis now lists Large 4 as an open-weight model, and on its open-weights leaderboard it ranks 7th. All six models ahead of it come from Chinese labs:

1. DeepSeek V4.1 Flash 1017: 32.5
2. GLM-5.3: 31.7
3. MiMo-V2.6-Pro: 29.9
4. Ling 3.1 Flash 1019: 29.5
5. MiniMax-M3.1-Flash: 28.9
6. Step 5 Preview: 28.5

That makes Large 4 the highest-scoring open-weight model from outside China, just ahead of Meta's Muse Spark Open (24.7) and well ahead of Alibaba's open Qwen 3.8 27B (20.1). The closed leaders sit far above it, including Gemini 4 Carbon at 59.4, Claude Opus 5.5 at 58.4, and GPT-6 Astra at 56.9. (Our Gemini 4 Carbon vs. Claude Opus 5.5 comparison covers the top two.)

One thing to know before you compare numbers: Artificial Analysis switched to v5 of its index after the preview launched, with a different and harder set of tests. The preview's 38 on the old index and the released model's 26 on the new one can't be compared directly. On v5, Artificial Analysis now scores the preview at 25, so on this broad index the released model is about a point higher.

Vals AI tells a similar story. Its October 29 review of the released model put Large 4 17th of 30 on the Vals Index (56.4%) and 5th of 15 open-weight models, with its best results on Vibe Code Bench and Harvey's Legal Agent Benchmark.

Simon Willison, who tested the preview on launch day, summed it up as "back to being maybe about 6 months behind the frontier" in his first look. The released model narrows that gap on coding and cyber work more than on general knowledge.

### Why the two pictures look so different

**Mistral's coding numbers depend on which agent runs the model.** The 41% Terminal-Bench score comes from Artificial Analysis's coding agent table, where Large 4 runs inside Mistral Vibe, Mistral's own coding agent. When Artificial Analysis runs the same test through its standard harness for the Intelligence Index, Large 4 scores 24.7%, and Vals AI got 26.3%.

Paired with Mistral's tooling, Large 4 leads the open-weight field on Terminal-Bench (GLM-5.3 scores 39.9% running in the Opencode agent), but in a generic harness it trails GLM-5.3, which scores 41.9% in the same standard test.

FrontierCode works the same way. Mistral's 19.7% comes from Cognition's private version of the test, while Artificial Analysis's own run puts Large 4 at 15.0%, behind GLM-5.3 (21.6%) and DeepSeek V4.1 Flash 1017 (28.5%).

The cyber numbers hold up best. Artificial Analysis confirms the Cyber Index score of 51, and Large 4 completed 79.4% of its reproduce-and-patch tasks without refusing a single one.

Claude Opus 5.5 and GPT-6 Astra score close to zero on that same test because they refuse 98.5% and 100% of the tasks. Those refusals come from the labs' safety policies, and they're the opening Mistral is pitching Large 4 into.

## Mistral Large 4 pricing

**Mistral Large 4 costs $0.70 per million input tokens, $0.07 per million cached input tokens, and $2.10 per million output tokens.** Mistral set that price with the open-weight release on October 29, roughly halving the preview's $1.36 / $0.14 / $4.18 list price. A 50% launch discount on that list ran for about the first two weeks of the preview.

A few other pricing details from Mistral's pricing page and docs:

- **Batch jobs:** Mistral's batch API runs at half the standard price across its models.
- **Regional endpoints:** the EU and US regional endpoints cost 10% more than the global one.
- **OpenRouter:** Mistral is the only provider serving it there, at the same $0.70 and $2.10 prices, plus an EU endpoint at $0.77 and $2.31 with a 512K context window.

### How it compares with other open-weight models

Here's how Large 4's price stacks up against the open-weight rivals Mistral benchmarks against, plus one closed model for scale. All prices are per million tokens from each vendor's own pricing page, checked November 3:

| 🤖 Model | 📥 Input | 💾 Cached input | 📤 Output | 🔓 Open weights | 
|---|---|---|---|---|
| Mistral Large 4 | $0.70 | $0.07 | $2.10 | Yes | 
| Kimi K3 | $3.00 | $0.30 | $15.00 | Yes | 
| GLM-5.3 (Z.ai) | $1.40 | $0.26 | $4.40 | Yes | 
| DeepSeek V4 Pro (peak hours) | $1.32 | $0.044 | $3.96 | Yes | 
| DeepSeek V4.1 Flash (peak hours) | $0.30 | $0.006 | $1.20 | Yes | 
| Mistral Large 3 | $0.50 | $0.05 | $1.50 | Yes | 
| Claude Opus 5.5 (closed) | $4.00 | $0.20 | $20.00 | No | 

**Large 4 now costs about half as much as GLM-5.3, and about half of DeepSeek V4 Pro's peak-hour rate.** It charges less than a quarter of what Kimi K3 charges for input, and about a seventh of Kimi's output price. Closed models cost more too: GPT-6.1 Sol lists at $2 input and $10 output per million tokens.

DeepSeek's rates drop by half outside peak hours (the pricing page lists the windows), and DeepSeek V4.1 Flash is still the cheap option by a wide margin.

### What it costs on a real job

**A call with 100,000 input tokens and 10,000 output tokens costs about $0.09.** If 90,000 of those input tokens come from cache, the cost drops to about $0.03.

For full agent tasks, Artificial Analysis's coding agent runs give a clearer picture. Here's the average cost per coding task, with each model running in its usual agent:

- **DeepSeek V4.1 Flash 1017:** $0.30
- **Mistral Large 4 (in Mistral Vibe):** $1.26
- **Kimi K3:** $5.17
- **GLM-5.3:** $5.23
- **GPT-6 Astra (in Codex):** $7.04
- **Claude Opus 5.5 (in Claude Code):** $11.42

That's where Large 4's price cut and leaner reasoning pay off. It matches GLM-5.3 on the Coding Agent Index at about a quarter of the cost per task.

The API's low, medium, and high reasoning settings are the easiest lever on output costs. Reasoning can't be switched off completely, so use low for quick jobs and save high for hard ones.

**Mistral's Vibe app can now run Large 4 for coding.** Mistral's announcement points to Vibe Code, the coding agent inside Vibe (the chat app formerly called Le Chat), and the open-source Vibe CLI can run Large 4 with an API key once you select it as the model. Mistral hasn't said which Vibe plans include Large 4, so check before you count on it. Vibe Pro costs $14.99 a month.

## What Mistral Large 4 is best at

**Mistral pitches Large 4 at a handful of specific jobs,** and the independent results back some of them more than others.

### Cyber defense

**Cyber work is where Large 4 stands out most.** It ranks 6th among publicly available models on Artificial Analysis's Cyber Index and 3rd among open-weight models, behind DeepSeek V4.1 Flash 1017 (56.8) and MiMo-V2.6-Pro (56.1). It's also far ahead of every open-weight model built outside China.

Mistral argues that defending software often starts with proving a flaw is real, and closed models with strict safety filters tend to refuse that work.

In Mistral's testing, Large 4 also helped with analyzing malware, prioritizing vulnerabilities, and writing detection rules. Mistral says it refuses 96.6% of malicious cyber requests on average, the highest rate it measured among open models.

Mistral is also candid about the risk. After watching live traffic during the preview, it said it saw potential for the model to help with real attacks and is tightening its guardrails. Mistral is giving enterprise design partners such as Orange Cyberdefense preview access to a specialized variant for penetration testing, vulnerability assessment, and red-teaming, subject to vetting and contractual safeguards. Google took a similar gated route with Gemini 4 Argon, which went to cyber defenders before anyone else.

### Business workflows and coding agents

**Large 4 is built to run multi-step agent jobs that end in a finished file or a completed task.** On AutomationBench, which runs 657 business workflows across apps like Gmail, Google Sheets, Slack, and Salesforce, Artificial Analysis scores the released model at 57.8%.

For coding, it hits 48.3% on Artificial Analysis's Coding Agent Index, including 58.9% on SWE-Atlas-QnA, inside Mistral Vibe. The score drops when you use it outside Mistral's own agent, so test it in the setup you plan to use. Our roundup of AI coding agents covers the tools you'd plug it into.

### Legal and finance work

**Legal work is Large 4's best independent result outside cyber and coding.** At release, it placed 8th of 31 models and 3rd of 16 open-weight models on Harvey's Legal Agent Benchmark on Vals AI, at 15.8%.

On Vals AI's Finance Agent test, it scores 61.7%, 6th among 19 open-weight models. Mistral's own numbers are stronger for spreadsheet-heavy finance work, with 73.3% on the Finch benchmark, second only to Kimi K3 among open models.

### Documents, charts, and images

**Mistral calls Large 4 a step change in how well its models understand images.** Its demos show it reading engineering drawings, pulling evidence from PDFs, and scanning gigapixel satellite images for small objects.

On visual grounding, Mistral reported 42% on Dense 200 at launch, just ahead of GPT-6 Astra's 41%, and its model card says Large 4 surpasses frontier closed models on that skill. Independent multimodal scores for the released model haven't been published yet.

### Sovereign and European deployments

**For teams that need their AI under European control, Large 4 is the strongest option from a European lab.** Mistral trained it from scratch in Europe and offers it in multiple regions, including a European deployment it operates end-to-end under European law.

Now that the weights are out, you can also run it on private cloud or on-premise hardware, so security teams and regulated industries can keep data in-house. For teams that want a managed option, Microsoft Foundry offers private deployments on Azure.

### Where it's weaker

**Large 4 is less reliable on general knowledge questions.** On Artificial Analysis's Omniscience test, which penalizes confident wrong answers, it scores 5.1, against 29.9 for Kimi K3.1 Pro and 48.1 for Claude Opus 5.5.

It also trails the closed leaders by a wide margin on broad reasoning. Its Intelligence Index score of 26 is less than half of what Claude Opus 5.5 and Gemini 4 Carbon post, so it works best as a specialist pick for coding, cyber, and legal work.

## Mistral Large 4 open weights and self-hosting

**Mistral Large 4's open weights went live on October 29, 2026, on** **Hugging Face****.** The main repo, mistralai/Mistral-Large-4-1T-A52B, holds the FP8 checkpoint, split into 54 weight files totaling about 1.06 TB.

There's also an NVFP4 version at about 610 GB, built for NVIDIA's Blackwell GPUs. Mistral says it delivers the same performance with a smaller memory footprint and faster inference.

Mistral added a technical report to the Hugging Face repo on November 2, four days after the weights, and the checkpoint ships with a built-in speculative decoding head that Mistral says can raise serving throughput by up to 1.9 times.

### What license does it use?

**Mistral Large 4 uses the Mistral Ecosystem Open-Weight License,** which the license file describes as a modified version of the MIT license.

Here's what it allows and where the catch sits:

- **Allowed for everyone:** using, copying, modifying, fine-tuning, and distributing the model, building derivative models, and selling products built on it, for commercial and non-commercial work.
- **The one condition:** if your company (or an affiliate) runs a model-as-a-service business, you need a separate commercial license from Mistral before using Large 4 commercially.
- **What counts as model-as-a-service:** giving outside parties access to the model for inference or post-training, such as through an API or hosted endpoint, in a way that gives them meaningful control over the inputs, parameters, or training data.
- **What doesn't count:** internal use that never exposes the model to third parties, end-user products where the model sits inside specific features, relaying requests to a model someone else hosts, and using it through Mistral's own products.

In practice, any company can run Large 4 internally, and a company that doesn't run a model-as-a-service business can use it commercially under the open license. Reselling Large 4 inference as an API is the clear case that needs Mistral's sign-off first.

That's a step back from Large 3, which shipped under Apache 2.0 with no such condition. It's also why Mistral and most coverage describe Large 4 as "open-weight."



### What hardware you'll need

**Mistral Large 4 still needs data center hardware for full-quality serving.** Here's what the released downloads look like, with the setups Mistral and Hugging Face have tested:

| 🧮 Version | 💾 Download size | 🖥️ Tested on | 
|---|---|---|
| FP8 (official) | About 1.06 TB | One 8-GPU server; Hugging Face's recipe uses 8 NVIDIA H200s | 
| NVFP4 (official) | About 610 GB | NVIDIA Blackwell GPUs; Dell's deploy example uses 4 | 
| Q4_K_M GGUF | About 635 GB | Several machines or GPUs with distributed inference | 
| Q2_K and IQ2_XXS GGUF (2-bit) | About 381 GB and 298 GB | A single Apple Silicon Ultra machine with 512 GB of memory | 

Download sizes come from the Hugging Face repos, and the tested setups come from Mistral's model card and Hugging Face's launch blog. Long contexts need extra memory on top of the weights.

The 2-bit versions are the closest thing to a home setup. Hugging Face measured about 15 tokens per second running the Q2_K file on a single M3 Ultra, but squeezing a model this hard costs some quality. It still won't run on a laptop or a gaming PC.

If you want something local on normal hardware, Mistral's Ministral 3 models are the realistic option (Mistral says Ministral 3 8B fits in 12 GB of VRAM in FP8).

### Which tools can run it

**Large 4 had day-one support in the major open-source serving tools.** Mistral's model card lists:

- **vLLM (recommended):** an official Docker image, vllm/vllm-openai:mistral-large-4, serving the model across 8 GPUs with tensor parallelism.
- **SGLang:** its own Docker image (lmsysorg/sglang:mistral-large-4) with the same 8-GPU layout.
- **Transformers:** supported; the model card says to install the library from its main branch.
- **llama.cpp:** GGUF files from the llama.cpp team (2-bit up to BF16), plus Unsloth GGUFs that go down to 1-bit.
- **Fine-tuning:** Axolotl, Unsloth, and Hugging Face's TRL.

Ollama lists it too, but only as a cloud model: running ollama run mistral-large-4:cloud sends your prompts to Ollama's servers at the same $0.70 and $2.10 prices.

## How to try Mistral Large 4 today

**You can use Mistral Large 4 through Mistral's API, a handful of partner platforms, or by downloading the weights.** Here's the quickest path:

1. **Open Mistral Studio:** sign up at console.mistral.ai. New accounts start in Free mode, which lets you create API keys and use a limited monthly allowance; the Limits page shows what's included.
2. **Test it in the playground:** pick Mistral Large 4 in the Studio playground and try your own prompts before writing any code.
3. **Call the API:** use the model ID mistral-large-4 with the chat completions or Agents endpoints. Set reasoning to "high" for hard problems and "low" for quick ones.
4. **Or go through OpenRouter:** it's listed as mistralai/mistral-large-4-0, served by Mistral at the same prices.

Beyond the API, Mistral's announcement lists Vibe Code, NVIDIA's hosted endpoints, and Microsoft Foundry, which Microsoft says makes it the first cloud provider to offer Large 4. Hugging Face also offers a ready-to-deploy Inference Endpoints recipe.

One gotcha: the mistral-large-latest alias still points to Large 3, so call Large 4 by its own ID. As of November 3, I couldn't find Large 4 on AWS Bedrock or Google Vertex AI.

## Should you use Mistral Large 4?

**Mistral Large 4 is worth testing for coding agents, security, legal, and European-hosting work, and easy to skip if you need the strongest general-purpose model.**

**Try Mistral Large 4 if you:**

- Run security work where closed models refuse to reproduce or patch real vulnerabilities
- Want a strong open-weight model you can host yourself in Europe, today
- Build coding agents and want GLM-5.3-level results at a lower cost per task
- Handle legal or finance agent tasks and want a capable open-weight option
- Work with technical drawings, dense documents, or satellite images

**Skip it for now if you:**

- Want the best general model available (Claude Opus 5.5 and Gemini 4 Carbon score more than twice as high on Artificial Analysis)
- Need the cheapest capable open model (DeepSeek V4.1 Flash charges $0.30 input and $1.20 output at peak hours, half that off-peak, and scores higher on Artificial Analysis)
- Plan to resell model access as an API without a commercial license from Mistral
- Want to run it on a laptop or a single GPU

If you'd rather not track which model wins which benchmark each week, Lindy, an AI teammate that works in Slack, lets you pick your model for each task from most major models, so you can switch when a new one pulls ahead.



## FAQ

### Is Mistral Large 4 open source?

**Mistral Large 4 is open-weight, under a modified MIT license with one commercial condition.** Anyone can download the weights from Hugging Face and use, modify, fine-tune, and sell products built on them under the Mistral Ecosystem Open-Weight License. The one exception is model-as-a-service businesses, which need a separate commercial license from Mistral.

### Is Mistral Large 4 on GitHub?

**No, the Mistral Large 4 weights are on Hugging Face.** Mistral posted them on October 29, 2026, at mistralai/Mistral-Large-4-1T-A52B, with an NVFP4 version alongside. Mistral's GitHub hosts related tools, including the open-source Vibe CLI coding agent, which can run Large 4 once you select it as the model.

### Is Mistral Large 4 free?

**The weights are free to download, and Mistral's API is paid, with a limited monthly allowance in Mistral Studio's Free mode.** The API costs $0.70 per million input tokens and $2.10 per million output tokens. Running the weights yourself is free under the license, but you'll need hundreds of gigabytes of GPU or unified memory.

### Where can I use Mistral Large 4?

**You can use Mistral Large 4 in Mistral Studio, through the Mistral API, on OpenRouter, and in Vibe Code.** It's also on Microsoft Foundry, NVIDIA's hosted endpoints, Ollama's cloud, and Hugging Face, where you can download the weights or deploy them on Inference Endpoints.

### Can I run Mistral Large 4 locally?

**You can run Mistral Large 4 locally only on serious hardware.** The official FP8 checkpoint is about 1.06 TB and runs on an 8-GPU server, and the llama.cpp team's 2-bit GGUF versions (about 298 and 381 GB) run on a single Apple Silicon Ultra machine with 512 GB of memory. Ollama's listing runs in the cloud, so it doesn't put the model on your computer.

### Which Mistral model is best?

**Mistral Large 4 is Mistral's most capable model,** and Mistral now names it as the replacement for both Mistral Large 3 and Mistral Medium 3.5, which were deprecated on October 31, 2026. For cheaper or smaller jobs, Mistral Small 4 and the Ministral 3 models are the lighter options, and its OCR and Voxtral models cover documents and audio.

### What is the latest version of Mistral?

**The latest Mistral flagship is Mistral Large 4,** launched as an API preview (version 26.10) on October 6, 2026, with open weights released on October 29. It's the successor to Mistral Large 3, a 675-billion-parameter model from December 2025 that's now deprecated and set to retire on April 30, 2027.

### How does Mistral compare to ChatGPT?

**Mistral's chat app, Vibe (formerly Le Chat), costs less than ChatGPT:** Pro is $14.99 a month, while ChatGPT Plus is $20 (our ChatGPT pricing guide covers the other plans). On raw ability, OpenAI's top model still leads, with GPT-6 Astra at 56.9 on Artificial Analysis's Intelligence Index against 26 for Mistral Large 4. Mistral's advantages are open weights, European hosting, and fewer refusals on security work.

### What is Mistral AI used for?

**Companies use Mistral AI for coding assistants, document processing, customer support, and private deployments** where data has to stay in-house or in Europe. Mistral says it supports more than 125 global enterprises, including Airbus, ASML, and HSBC.

### Which country is Mistral AI from?

**Mistral AI is from France.** It was founded in Paris in 2023 by Arthur Mensch, Guillaume Lample, and Timothée Lacroix. In September 2026, it raised a €3 billion Series D led by Samsung, which Mistral says is the largest equity round ever raised by a European tech company.

### Is Mistral Large 4 good?

**Mistral Large 4 is very good at specific jobs and middle of the pack overall.** It effectively ties the top open-weight model on Artificial Analysis's Coding Agent Index at a much lower cost per task, scores 51 on the Cyber Index without refusing legitimate defense work, and places 3rd among open-weight models on Harvey's legal benchmark. As a general model, it ranks 7th among open-weight models on Artificial Analysis and trails the closed leaders by a wide margin.

## About the editorial team

**Flo Crivello**Founder and CEO of Lindy

Flo Crivello is the founder and CEO of Lindy. Before that, he founded Teamflow and was a product manager at Uber. He writes about technology, startups, and the future of work on his blog.
