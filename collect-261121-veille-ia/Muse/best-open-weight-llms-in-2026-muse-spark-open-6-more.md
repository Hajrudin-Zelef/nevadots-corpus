---
id: collect-261121-veille-ia/muse/best-open-weight-llms-in-2026-muse-spark-open-6-more
title: "Best Open-Weight LLMs in 2026: Muse Spark Open & 6 More"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: ["2026-10-15"]
keywords: ["muse", "spark", "open", "meta", "model", "open-weight", "licence", "flash"]
source: https://www.edenai.co/post/best-open-weight-llms
source_anchor: ""
source_lines: [1, 132]
section: "Muse"
---
Summarize this article with:

Meta Muse Spark Open leads a busy month for open weights. Here are the seven open-weight LLMs worth knowing in October 2026, and where to run them in Europe.

October 2026 has been the busiest month for open-weight models in a long time. In a single week, Meta released Muse Spark Open, DeepSeek shipped a new checkpoint of V4.1 Flash, MiniMax launched M3.1 Flash and published its weights, and Reflection, a new US lab, put its first model, Beam, on Hugging Face.

Open weights matter for a practical reason: you are not tied to one company's API. You can run the model yourself, pick the provider that hosts it in the region you need, or switch providers when prices move. This guide covers the seven open-weight LLMs worth knowing right now, what each one is good at, under which licence, and where you can run it, including in Europe.

## At a glance

| Model | Maker | Best for | Licence | From, per 1M tokens (in / out) | EU route on Eden AI | 
|---|---|---|---|---|---|
| Muse Spark Open | Meta | Agents, tool calling, long context | Muse Model License | $1.25 / $4.25 | Via Meta's API (Muse Spark) | 
| Kimi K3 | Moonshot AI | Hard coding and research tasks | Kimi K3 licence | $1.95 / $9.75 | Yes | 
| GLM-5.3 | Z.ai | Agentic coding, tool use | GLM-5.3 licence (Flash: MIT) | $0.90 / $3.00 | Yes | 
| DeepSeek V4.1 Flash | DeepSeek | Best value for agents and coding | MIT | $0.097 / $0.39 | Yes | 
| MiniMax M3.1 Flash | MiniMax | Cheap, fast agentic work | MiniMax community licence | $0.15 / $0.60 | Not yet | 
| Beam 1.0 | Reflection | Permissive licence at large scale | Apache 2.0 | $0.25 / $1.00 | Not yet | 
| Apertus v1.5 70B | Swiss AI | Fully open, Swiss hosting | Apache 2.0 | $0.78 / $2.80 | Switzerland | 

## 1. Meta Muse Spark Open

Released on 15 October 2026, Muse Spark Open is the open-weights version of Muse Spark, Meta's model for agents. It is a sparse mixture-of-experts model with 816 billion parameters, about 42 billion of them active per token, a one million token context window, and text, image, video and audio as input. Artificial Analysis scored it 39 on its Intelligence Index, which makes it the strongest US open-weights model on that index at launch.

Meta trained it for long-horizon agentic work such as coding and customer service agents, and early users point to its tool calling: picking the right next tool, multi-step and parallel calls, and clean structured outputs. It is also unusually portable for its size: Meta has verified it on NVIDIA, AMD, Intel and Apple silicon, down to a pair of Mac Studios.

Two details before you adopt it. The licence is Meta's own Muse Model License, not a standard open-source licence, so read it before shipping. And if you do not need the weights themselves, Meta's API also serves the closed Muse Spark models and Muse Wonder, its most capable model, launched on 12 October.

On Eden AI, Muse Spark Open runs on Nebius, Together AI and Fireworks AI, and the Muse Spark and Muse Wonder API models run in the EU region.

## 2. Kimi K3

Moonshot AI's Kimi K3 has been one of the strongest open-weight models since its release in June, and it remains a reference point. In the comparison Reflection published with Beam, Kimi K3 posts the best open-weight scores on SWE Bench Pro v2-Hard (88.2) and BrowseComp with context management (91.2).

It is also the model with the widest choice of European routes. On Eden AI you can run it on TensorX, which runs its own data centres in Dublin and Helsinki, on GreenPT, on sference, which runs on compute inside the EEA according to sference, and on Arkane Cloud, among others. Kimi K3 is not cheap by open-weight standards, so keep it for the tasks where its quality pays off.

## 3. GLM-5.3

Z.ai's GLM-5.3, released in August, is a favourite for agentic coding and tool use: in Reflection's table it scores 84.2 on MCP Atlas and 88.2 on Terminal Bench v2.1. The smaller GLM-5.3 Flash is released under the MIT licence, which makes it one of the easiest models in this list to adopt.

GLM-5.3 has more routes on Eden AI than any other model in this guide, and many of them are European: Mistral's own API, TensorX, GreenPT, Greenference, Scaleway and Arkane Cloud all serve it from the EU. sference serves it too. If European hosting is a requirement, GLM-5.3 is the safest place to start.

## 4. DeepSeek V4.1 Flash

DeepSeek released a new checkpoint of V4.1 Flash on 17 October, under the MIT licence. It is the best value in this list: from $0.097 per million input tokens, with strong agentic results, including 90.6 on Terminal Bench v2.1 and 74.2 on DeepSWE in Reflection's comparison. sference's benchmark notes on the new checkpoint report 20 to 30% less reasoning effort for the same correctness.

It is also everywhere. On Eden AI it runs on around twenty routes, from FlexAI, sference and TensorX to GreenPT, Greenference, Scaleway and Google Vertex in the EU. That makes it the easiest model in this list to move between providers on price or region.

## 5. MiniMax M3.1 Flash

MiniMax launched M3.1 Flash on 19 October and published its weights on 23 October. It is built for fast, cheap agentic work: MiniMax claims nearly three times faster inference and 70% lower usage cost than the previous generation, and it starts at $0.15 per million input tokens. Artificial Analysis scored it 42 on its Intelligence Index.

There is a catch: VentureBeat reported a high hallucination rate. Use it for tasks where the answer can be checked, such as code with tests or extraction against a schema, before trusting it with open questions. On Eden AI it runs on MiniMax's own API, Nebius and Together AI, with no European route yet.

## 6. Reflection Beam 1.0

Beam is the first model from Reflection, a new US lab. It has 501 billion parameters with 23 billion active, a 256K context trained to stretch to one million tokens, and it comes under Apache 2.0, the most permissive licence among the large models in this list.

Reflection's own comparison is candid: Beam trails GLM-5.3, Kimi K3 and DeepSeek V4.1 Flash on most coding benchmarks, but holds its own on GPQA Diamond (90.5) and AIME 2026 (97.8). Pick it when the licence matters as much as the scores. On Eden AI it runs on Nebius and Together AI, from $0.25 per million input tokens.

## 7. Apertus v1.5 70B

Apertus comes from the Swiss AI initiative, a public research effort, and ships under Apache 2.0. At 70 billion parameters it does not compete with the giants above on raw capability, but it answers a different question: a model you can audit, served from Switzerland.

On Eden AI, Apertus runs on Infomaniak, in the Swiss region, alongside other open models Infomaniak hosts there.

## How to choose

- **Best overall for agents:** Muse Spark Open or Kimi K3, depending on whether you value tool calling or raw coding scores.
- **Best value:** DeepSeek V4.1 Flash, with the widest choice of providers.
- **European hosting:** GLM-5.3, Kimi K3 or DeepSeek V4.1 Flash, all with several EU routes.
- **Most permissive licence at scale:** Beam (Apache 2.0) or DeepSeek V4.1 Flash (MIT).
- **Swiss hosting and full openness:** Apertus v1.5.
- **Lowest price for checkable tasks:** MiniMax M3.1 Flash, with a verification step.

## Where to run them in Europe

| Model | European routes on Eden AI | 
|---|---|
| Muse Spark, Muse Wonder (API) | Meta (EU region) | 
| Kimi K3 | TensorX, GreenPT, Arkane Cloud | 
| GLM-5.3 | Mistral, TensorX, GreenPT, Greenference, Scaleway, Arkane Cloud | 
| DeepSeek V4.1 Flash | TensorX, GreenPT, Greenference, Scaleway, Google Vertex, Arkane Cloud | 
| Apertus v1.5 70B | Infomaniak (Switzerland) | 

sference also serves Kimi K3, GLM-5.3 and DeepSeek V4.1 Flash on compute inside the EEA, according to sference.

## Switching is one string

On Eden AI, every model in this guide sits behind the same API and the same key, so trying several on your own prompts is a matter of changing one field. Sending calls to the European endpoint keeps them on EU routes:

```
import requests
for model in [
    "meta/muse-spark-1.3",
    "tensorx/moonshotai/kimi-k3",
    "mistral/zai-glm-5-3",
    "greenpt/deepseek-v4.1-flash",
]:
    response = requests.post(
        "https://api.eu.edenai.run/v3/chat/completions",
        headers={"Authorization": "Bearer YOUR_EDENAI_API_KEY"},
        json={
            "model": model,
            "messages": [{"role": "user", "content": "Summarise this ticket in one line: ..."}],
        },
    )
    print(model, response.json()["choices"][0]["message"]["content"])
```
The European endpoint refuses a model that has no EU route rather than serving it from somewhere else, so a residency rule holds even when someone changes the model name.

## FAQ

### What is the best open-weight LLM in October 2026?

It depends on the job. Meta Muse Spark Open is the strongest US open-weights model on the Artificial Analysis Intelligence Index, Kimi K3 leads several coding and research benchmarks, and DeepSeek V4.1 Flash offers the best value.

### Is Meta Muse Spark Open open source?

Its weights are public, but under Meta's own Muse Model License rather than a standard open-source licence. Beam and Apertus use Apache 2.0, and DeepSeek V4.1 Flash and GLM-5.3 Flash use MIT.

### Can I run these models in the EU?

Yes for most of them. On Eden AI, Kimi K3, GLM-5.3 and DeepSeek V4.1 Flash each have several EU routes, Meta's Muse API models run in the EU region, and Apertus runs in Switzerland. MiniMax M3.1 Flash and Beam have no European route yet.

### Which open-weight model is the cheapest?

DeepSeek V4.1 Flash, from $0.097 per million input tokens on Eden AI, followed by MiniMax M3.1 Flash at $0.15.

### Do I need my own GPUs to use open-weight models?

No. You can download the weights and run them yourself, or call them through a hosted provider. On Eden AI, all seven are available through one API.
