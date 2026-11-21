---
id: collect-261121-veille-ia/gemini-google/gemini-4-carbon-beats-opus-5-5-and-gpt-6-astra-the-catch
title: "Gemini 4 Carbon Beats Opus 5.5 and GPT-6 Astra: The Catch"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["carbon", "gemini", "google", "argon", "gpt-6", "astra", "opus", "model"]
source: https://www.edenai.co/post/gemini-4-carbon-release-benchmarks-pricing-availability
source_anchor: ""
source_lines: [1, 155]
section: "Gemini_Google"
---
Summarize this article with:

Gemini 4 Carbon is Google's new frontier model, ahead on 15 of 17 benchmarks in Google's table. Here is what changes, what it costs and how to be ready when it opens.

On 28 October 2026, Google announced Gemini 4 Carbon, which it calls its strongest model. It arrives two weeks after Gemini 4 Argon, with a clear focus: long-running coding and agent work, and cybersecurity defense.

Google has shipped fast this year, from Gemini 3.5 to Argon and now Carbon. This one comes with a twist: you cannot use it yet. Carbon goes to a small group of cyber defenders first, and Google has not given a date for developers.

This guide covers what Carbon changes, how it compares with GPT-6 Astra and Claude Opus 5.5, what it will cost, and how to prepare your stack so that adopting it takes one line of code.

## Gemini 4 Carbon at a glance

| Announced | 28 October 2026, by Google DeepMind | 
| Position | Google's strongest model, successor to Gemini 4 Argon (released two weeks earlier) | 
| Focus | Long-horizon coding and agents, cybersecurity defense, science, web development, multimodal understanding | 
| Access today | A subset of cyber defenders, then trusted partners through the Fairwind Program | 
| Developers | "Coming soon", no date announced | 
| Price | $2 / $10 per 1M input / output tokens at launch, then $4 / $20. Cached input 95% off | 
| Not published yet | API model name, context window, rate limits | 
| On Eden AI | Not yet. Gemini 4 Argon is available now, via the Gemini API and Vertex AI, including on the EU endpoint | 

## What changes compared with Gemini 4 Argon

Carbon builds on Argon rather than replacing its foundations. Google points to five areas of progress.

- **Long-horizon work:** Carbon is built for sustained agentic workflows. Google says its internal teams have seen it run for days on multi-step engineering tasks, such as tracking down and patching subtle bugs across large codebases.
- **Coding and web development:** Carbon gains 89 Elo points on WebDev Arena and ranks fourth there, at 1768.
- **Cybersecurity:** Google reports more than twice Argon's success rate at generating secure code on its internal benchmarks. The Chrome team reports patches more than ten times better than those of another lab's large frontier model, and Wiz sees Carbon leading on web penetration testing.
- **Long context:** on GraphWalks, which tests reasoning over 256k to 1M tokens, Carbon scores 96.2%, against 84.2% for Argon.
- **Behaviour:** Google says Carbon cuts unsupported claims by about half compared with Argon, reduced successful cheating on Terminal-Bench 4.0 to 0.0%, and resisted every attack in Gray Swan's prompt injection benchmark (0.0%, against 0.7% for Argon). It also reports 26% less overreach and 29% less under-delivery on multi-step plans.

For teams running agents unattended, that last point may matter as much as the scores: a model that flags what it could not verify, instead of filling the gap, is easier to trust in production.

## Benchmarks: Carbon vs GPT-6 Astra and Claude Opus 5.5

Google published a table of 17 benchmarks comparing Carbon with Argon, GPT-6 Astra, Claude Opus 5.5 and Claude Fable 5.1. Carbon has the best score on 15 of them. Here is a selection.

| Category | Benchmark | Gemini 4 Carbon | Gemini 4 Argon | GPT-6 Astra | Claude Opus 5.5 | 
|---|---|---|---|---|---|
| General | AA Intelligence Index v5.0 | **59** | 50 | 57 | 58 | 
| General | Vals Index v2.2 | **70.4%** | 69.2% | 65.0% | 70.2% | 
| Agentic coding | FrontierSWE 2 | 61.2% | 55.0% | **65.5%** | 62.3% | 
| Agentic coding | Terminal-Bench 4.0 | **65.2%** | 57.1% | 59.1% | 59.6% | 
| Computer use | OSWorld-2.1 (offline subset) | 81.5% | 74.7% | 80.9% | **87.2%** | 
| Knowledge work | AutomationBench (Zapier) | **53.9%** | 51.3% | 41.4% | 42.5% | 
| Multimodal | Blueprint-Bench 2 | **58.6%** | 54.4% | 49.7% | 51.2% | 
| Long context | GraphWalks (256k to 1M) | **96.2%** | 84.2% | 71.8% | 66.8% | 
| Science | RiemannBench | **80.0%** | 76.0% | 72.0% | 69.6% | 
| Cybersecurity | CWE-bench (patching) | **71.0%** | 68.0% | 68.0% | 67.0% | 
| Cybersecurity | ExploitGym (exploitation) | **54.8%** | 42.0% | 42.4% | 34.5% | 
| Cybersecurity | SEC-bench Pro (finding flaws) | **88.2%** | 83.4% | 85.4% | not reported | 

Three things stand out:

- **Carbon leads in general intelligence, but narrowly.** It is first on the Artificial Analysis Intelligence Index v5.0 at 59, one point ahead of Claude Opus 5.5 and two ahead of GPT-6 Astra.
- **The gaps are widest on long context and cybersecurity.** On GraphWalks, Carbon is 12 points ahead of Argon and more than 24 points ahead of GPT-6 Astra. On ExploitGym, it is more than 12 points ahead of every other model in the table.
- **Carbon does not win everything.** GPT-6 Astra keeps the lead on FrontierSWE 2, and Claude Opus 5.5 on OSWorld-2.1, a computer-use benchmark. According to VentureBeat, Opus 5.5 also stays ahead on the Artificial Analysis Coding Agent Index, where Carbon ranks second.

These are Google's own numbers, and the cybersecurity results were measured without guardrails. They show where Carbon is strongest, not how it will perform on your tasks.

## Pricing and availability

Carbon will launch at the same introductory price as Argon. Google has not said when the introductory period will end.

| Model | Input (per 1M tokens) | Output (per 1M tokens) | Usable through an API today | 
|---|---|---|---|
| Gemini 4 Carbon (introductory) | $2 | $10 | Not yet (staged rollout) | 
| Gemini 4 Carbon (after introductory period) | $4 | $20 | Not yet | 
| Gemini 4 Argon | $2 | $10 | Yes, on Eden AI | 
| Claude Opus 5.5 | $4 | $20 | Yes, on Eden AI | 
| GPT-6 Astra | $10 | $50 | Yes, on Eden AI | 
| Gemini 3.8 Flash | $0.75 | $3.75 | Yes, on Eden AI | 

At its introductory price, Carbon would cost half as much as Claude Opus 5.5 and a fifth of GPT-6 Astra per token. Even at its full price, it matches Opus 5.5.

Availability is the catch. Google is rolling Carbon out in stages:

1. this week, a subset of cyber defenders;

2. then additional trusted partners, through Google's Fairwind Program;

3. then developers, businesses and consumers, "as soon as possible".

Google is also taking part in the US government's voluntary pre-release testing process. The API model name and the context window have not been published yet. For reference, Argon was opened to developers two weeks after its announcement, and Ars Technica expects Carbon could follow a similar path.

## Should you wait for Carbon?

It depends on what you are building.

- **Security tooling, vulnerability triage, code review:** this is Carbon's strongest area. If you qualify, apply to the Fairwind Program. Otherwise, build your pipeline now on a model you can already use, and plan to test Carbon on it.
- **Long-running agents and large codebases:** Carbon's long-context and behaviour gains are the most relevant here. Argon already offers a 1M-token context window at the same price, and is a natural starting point.
- **Computer use and desktop automation:** Claude Opus 5.5 still leads OSWorld-2.1 in Google's own table. No need to wait.
- **High volume, cost-sensitive work:** frontier models are rarely the right default here. Gemini 3.8 Flash costs $0.75 / $3.75 per million tokens on Eden AI.

In every case, the safest move is not to bet on one model, but to make switching cheap.

## How to get ready with Eden AI

Carbon is not on Eden AI yet, because Google has not opened it to developers. Everything around it already is: Gemini 4 Argon, Claude Opus 5.5, GPT-6 Astra and more than 1,000 other models, behind one API key and one OpenAI-compatible endpoint. When Google opens access, we will add Carbon to the catalogue, and switching to it will be a change of model name.

The best preparation is to measure the models you can use today on your own tasks, so that Carbon has a baseline to beat when it arrives:

```
from openai import OpenAI
client = OpenAI(api_key="YOUR_EDENAI_API_KEY", base_url="https://api.edenai.run/v3")
task = (
    "Find the security bug in this function and return a fixed version:\n"
    'def get_user(db, user_id):\n'
    '    return db.execute(f"SELECT * FROM users WHERE id = {user_id}")'
)
# add Gemini 4 Carbon to this list when Google opens it to developers
for model in ["gemini-4-argon", "anthropic/claude-opus-5-5", "openai/gpt-6-astra"]:
    response = client.chat.completions.create(
        model=model,
        messages=[{"role": "user", "content": task}],
    )
    print(model, response.choices[0].message.content)
```
We ran this on 30 October 2026. All three models found the SQL injection and proposed a parameterised query. Argon cost under half a cent, Opus 5.5 about 1.7 cents and GPT-6 Astra under 1 cent, for this prompt.

Once you have picked a model, Eden AI handles the production side:

- **Fallbacks between Google's own platforms:** Argon is served on Eden AI both by the Gemini API and by Vertex AI. With`extra_body={"routing": {"order": ["google", "vertex"]}}` , requests go to the Gemini API first and to Vertex AI if it fails. We tested both orders.
- **EU data residency:** the same code sent to the EU endpoint,`https://api.eu.edenai.run/v3` , was served by Vertex AI in Europe.
- **One bill and one key** for Gemini, Claude, GPT and the rest, so testing a new model never starts with a new contract.
- **Easy comparisons:** prices for every model are in the catalogue, and provider routing lets you sort by cost or set your own order.

## What to keep in mind

- **The benchmarks are Google's:** they come from the announcement, and the cybersecurity results were measured without guardrails. Independent evaluations will follow once the model is broadly available.
- **No date yet:** Google says "as soon as possible". Do not plan a migration around a date that has not been announced.
- **The introductory price is temporary:** $2 / $10 will become $4 / $20 per million tokens, at a date Google has not shared.
- **Some details are missing:** the API model name, the context window and the rate limits will only be known at launch.

## FAQ

### What is Gemini 4 Carbon?

Gemini 4 Carbon is Google's newest frontier model, announced on 28 October 2026. Google describes it as its strongest model, with state-of-the-art results in cybersecurity and long-horizon tasks. It succeeds Gemini 4 Argon, released two weeks earlier.

### Can I use Gemini 4 Carbon today?

Not unless you are part of Google's first cohort. Carbon is available to a subset of cyber defenders this week, then to trusted partners through the Fairwind Program. Google says it will reach developers, businesses and consumers as soon as possible, without a date.

### How much does Gemini 4 Carbon cost?

$2 per million input tokens and $10 per million output tokens at launch, the same introductory price as Argon, with cached input 95% cheaper. After the introductory period, the price will be $4 and $20.

### Is Gemini 4 Carbon better than Claude Opus 5.5 and GPT-6 Astra?

On Google's benchmark table, Carbon has the best score on 15 of 17 tests. GPT-6 Astra leads on FrontierSWE 2 and Claude Opus 5.5 on OSWorld-2.1. The scores are Google's own, so test on your own tasks before switching.

### Is Gemini 4 Carbon available on Eden AI?

Not yet, because Google has not opened it to developers. Gemini 4 Argon is available on Eden AI now, through the Gemini API and Vertex AI, including on the EU endpoint. When Carbon opens, switching will be a change of model name.
