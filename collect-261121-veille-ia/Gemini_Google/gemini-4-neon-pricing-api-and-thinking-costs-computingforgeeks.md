---
id: collect-261121-veille-ia/gemini-google/gemini-4-neon-pricing-api-and-thinking-costs-computingforgeeks
title: "Gemini 4 Neon Pricing, API and Thinking Costs | ComputingForGeeks"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-11-18"]
keywords: ["gemini", "neon", "vertex", "tokens", "thinking", "pricing", "carbon", "global"]
source: https://computingforgeeks.com/gemini-4-neon-pricing-api/
source_anchor: ""
source_lines: [1, 224]
section: "Gemini_Google"
---
Gemini 4 Neon costs $3.00 per million input tokens and $15.00 per million output tokens. That is 50% more than Gemini 4 Carbon today and 25% less than Carbon from December 1, 2026. The per token comparison hides the number that decides the bill. On one fixed debugging prompt, Gemini 4 Neon (measured November 19, 2026 on the Vertex AI global endpoint, model ID `gemini-4-neon`) spent 6,766 thinking tokens at its default HIGH level to write a 739 token answer, so 90% of what we paid for was reasoning nobody reads.

We ran the same prompt at every thinking level, captured the exact HTTP 400 errors that break Gemini 3 era code, tested whether temperature still does anything, and checked implicit caching above the 4,096 token floor. Prices below are dated, because three of them change before mid January.

Google launched Neon on November 18, 2026 as a coding model adapted from Carbon. Its benchmark claims appear at the end as vendor numbers we did not reproduce.

## 1. Gemini 4 Neon pricing per 1M tokens

Neon has one price on the Gemini API and the same price on the Vertex AI global endpoint. Regional endpoints (`us` and `eu`) add 10%. Thinking tokens bill at the output rate, which is the line that grows fastest in practice. The table reads the Google AI Studio pricing page and the Agent Platform (Vertex AI) pricing page as of November 19, 2026.

| Tier (per 1M tokens) | Input | Cached input | Output incl. thinking | 
|---|---|---|---|
| Standard, Gemini API and Vertex global | $3.00 | $0.15 | $15.00 | 
| Standard, Vertex non-global (us, eu) | $3.30 | $0.165 | $16.50 | 
| Priority, Vertex global | $5.40 | $0.27 | $27.00 | 
| Batch, Vertex global | $1.50 | $0.075 | $7.50 | 
| Cache write, Vertex global (from 2027-01-12) | $3.75 | n/a | n/a | 
| Free tier | None. Neon is paid only on the Gemini API. |  |  | 

Grounding with Google Search is billed on top: 5,000 search queries per month are free, shared across Gemini 3 and newer models, then $14 per 1,000 queries. One prompt can run several queries, and AI Studio also caps grounding at 1,500 requests per day. Two billing rules on the Vertex pricing page matter more for Neon than for older models:

- **Failed requests are free.** Only HTTP 200 responses are charged, so the 429 and 400 responses shown later cost nothing. The one exception is a long completion that times out, covered next.
- **Long completions bill even when they fail.** Since November 13, 2026, a completion over 32K tokens is charged for the tokens already generated if the request dies on a client or server timeout. Our XHIGH run produced 22,909 output and thinking tokens on a two sentence question, so a harder prompt at XHIGH can cross that line. That call took 3.4 minutes, longer than many HTTP client defaults, so raise the client timeout well past that before using XHIGH or you pay for an answer you never receive.

The Gemini API lists the same discounted tiers for Neon: Batch and Flex at $1.50/$7.50 with $0.075 cached input, and Priority at $5.40/$27 with $0.27 cached input. At half the Standard rate, Batch is the obvious home for offline jobs such as nightly code review or log triage.

## 2. Neon vs Hydrogen, Carbon and Argon before and after December 1

Carbon and Argon carry launch pricing of $2/$10 through November 30, 2026. On Vertex AI that discount arrives as 50% credits back on net spend rather than a lower line rate, so expect a credit line on the invoice instead. From December 1 both move to $4/$20, while Neon stays at $3/$15. So the answer to “is Neon expensive” flips in twelve days: it is the pricier choice this month and the cheaper one per token next month.

| Model (Standard, per 1M) | Through 2026-11-30 | From 2026-12-01 | Neon vs it, from Dec 1 | 
|---|---|---|---|
| Gemini 4 Neon | $3.00 / $15.00 | $3.00 / $15.00 | n/a | 
| Gemini 4 Carbon | $2.00 / $10.00 | $4.00 / $20.00 | 25% cheaper | 
| Gemini 4 Argon | $2.00 / $10.00 | $4.00 / $20.00 | 25% cheaper | 
| Gemini 4 Hydrogen | $1.50 / $7.50 | $1.50 / $7.50 | 2x the price | 
| Gemini 3.8 Flash | $0.75 / $3.75 (intro to 2026-12-31) | $0.75 / $3.75, then $1.50 / $7.50 from 2027-01-01 | 4x the price, 2x from Jan 1 | 

Per token is not per task, though. Google’s own launch post claims Neon has a 32% lower cost per task than Carbon at the default HIGH level, without saying which Carbon price it used. We did not run Carbon, so treat that figure as Google’s. Our own ladder in the next section shows why the per task cost is the one to watch: thinking volume moves the bill far more than the rate card does.

For Anthropic’s side, Claude Opus 5.5 lists at $4 input and $20 output per million on Anthropic’s pricing page, so Neon is 25% cheaper per token. We did not run Opus here; a matched Opus and Fable comparison is a separate piece of work. Our Gemini 4 Argon release notes cover the model Neon sits beside in the lineup.

## 3. What each thinking level cost on one prompt

Neon accepts four thinking levels: LOW, MEDIUM, HIGH and XHIGH. HIGH is the default, which is a change from Argon and Gemini 3.8 Flash where omitting the level gave you MEDIUM. We sent one prompt at each level: a FastAPI service with httpx and asyncpg leaking file descriptors until EMFILE, asking for three root causes and one confirming Linux command each. It is the same prompt we used for the GPT-6.2 Astra and Sol pricing ladder, so the two sets of numbers line up.

Our lab harness (a short Python loop over the same `generateContent` call shown in section 4) saved each response as JSON. The `summarize.py` and `jq` commands in this article read those saved result files, so they are our records rather than steps to repeat; the curl blocks are the part you can run as is. This summary prints one line per run with latency, token counts and list cost at $3/$15:

`python3 summarize.py ladder1-low ladder1-medium ladder1-high ladder1-xhigh`
Four calls, one prompt, and the total already passes half a dollar:

Here are the four runs in one table. Neon’s latency is wall time for the successful attempt only, not the 429 retries before it.

| Level | Thinking | Answer | Latency | Cost | 
|---|---|---|---|---|
| LOW | 1,924 | 550 | 20.7 s | $0.0373 | 
| MEDIUM | 5,032 | 624 | 49.6 s | $0.0851 | 
| HIGH (default) | 6,766 | 739 | 72.0 s | $0.1128 | 
| XHIGH | 21,951 | 958 | 205.8 s | $0.3439 | 

Three things stand out from these runs:

- **Thinking is the bill.** Thinking tokens were 78% of billed output at LOW, 90% at HIGH and 96% at XHIGH. The visible answer only grew from 550 to 958 tokens across the whole ladder.
- **Check GPT-6.2 on the same prompt.** Its effort costs for this exact question are in the pricing post linked above, so set the two tables side by side before you pick a model on list price.
- **Throughput is flat, so latency tracks thinking.** Every level streamed at 104 to 120 tokens per second. XHIGH took 3.4 minutes because it generated about three times as many tokens as HIGH (22,909 against 7,505).

We also sent the prompt with no thinking config at all, on the `us` endpoint. It used 6,118 thinking tokens and 615 answer tokens in 63.0 s, close to our HIGH run and well above MEDIUM, which fits Google’s note that HIGH is now the default. So for interactive use, set `thinkingLevel` to LOW explicitly and raise it per request. Leaving it at the default HIGH cost us three times more per call, and the answer barely moved. Every level named the same top two causes: a new `httpx.AsyncClient` per request, and asyncpg connections or pools opened per request without closing. Only the third changed, from unclosed `UploadFile` temp files at LOW to leaked inbound client sockets at MEDIUM and above. One prompt is a small sample, and thinking volume varies run to run, so budget from your own traffic once it is flowing.

## 4. Calling gemini-4-neon on Vertex AI

The model ID is `gemini-4-neon` on both the Gemini API and Vertex AI. On Vertex it is GA in three locations:

- **global:** the cheapest at $3/$15, served from`aiplatform.googleapis.com` . Our ladder in section 3 ran here.
- **us:** the US multi-region at $3.30/$16.50, served from`aiplatform.us.rep.googleapis.com` .
- **eu:** the EU multi-region at the same 10% premium as`us` .

We moved the captures below to `us` because the global endpoint answered every request with HTTP 429 for more than 20 minutes on launch morning while `us` served the same project at once. Section 5 shows that 429. The project needs the Vertex AI API enabled and a logged in gcloud. Set the project once so the requests can reference it:

`export PROJECT_ID="$(gcloud config get-value project 2>/dev/null)"`
This is the exact `generateContent` request we captured, with `thinkingLevel` set to LOW and jq trimming the response to the model, the visible text and the usage block:

```
curl -s "https://aiplatform.us.rep.googleapis.com/v1/projects/${PROJECT_ID}/locations/us/publishers/google/models/gemini-4-neon:generateContent" \
  -H "Authorization: Bearer $(gcloud auth print-access-token)" -H "Content-Type: application/json" \
  -d '{"contents":[{"role":"user","parts":[{"text":"Name the systemd unit file that runs sshd on Debian. Reply with the file name only."}]}],"generationConfig":{"thinkingConfig":{"thinkingLevel":"LOW"}}}' \
  | jq '{model: .modelVersion, text: ([.candidates[0].content.parts[] | select(.thought != true) | .text] | join("")), usage: .usageMetadata}'
```
Neon answered `ssh.service`, which is correct for Debian. The usage block shows 7 answer tokens and 193 thinking tokens even at LOW, so this one word reply cost $0.0035 at the `us` rate, almost all of it reasoning:

Watch `thoughtsTokenCount` in that usage block, not `candidatesTokenCount`. It is billed at the output rate but is reported in its own field, so a cost dashboard that only sums candidates will undercount Neon by the ratios shown in section 3. For SDK based code with streaming and tool calls, our Vertex AI Gemini Python guide covers the client setup; only the model string and the thinking config change for Neon.

### The OpenAI compatible endpoint

Vertex also serves Neon through its OpenAI compatible chat completions endpoint, with the model written as `google/gemini-4-neon` and `reasoning_effort` in place of the thinking config:

```
curl -s "https://aiplatform.us.rep.googleapis.com/v1/projects/${PROJECT_ID}/locations/us/endpoints/openapi/chat/completions" \
  -H "Authorization: Bearer $(gcloud auth print-access-token)" -H "Content-Type: application/json" \
  -d '{"model":"google/gemini-4-neon","reasoning_effort":"low","messages":[{"role":"user","content":"Name the systemd unit file that runs sshd on Debian. Reply with the file name only."}]}' \
  | jq '{model, text: .choices[0].message.content, usage}'
```
Same answer, and the thinking shows up as `reasoning_tokens` (188) inside `completion_tokens_details`, while `completion_tokens` counts only the 7 visible tokens. Any OpenAI style cost tracker that multiplies `completion_tokens` by the output price will miss most of the Neon bill:

Google’s Gemini 4 Neon developer guide warns that this compatibility layer cannot retrieve long completions, so keep XHIGH work on `generateContent`. Even there, each foreground segment stops at 65,536 output tokens, which is also the default output budget. Going past it means raising `maxOutputTokens` and calling the v1beta1 API, either directly or through the Gen AI SDK on its default v1beta1 version (google-genai 2.30.1 or later); the Interactions API is the other route. One difference from the native API surprised us: `reasoning_effort: "minimal"` is accepted here. It returned “ok” with 25 reasoning tokens, close to the 26 the same “Say ok” prompt used at `low`. That matches the Gemini API’s OpenAI compatibility docs, which map `minimal` to `low` on Gemini 4 models, so the compatibility layer accepts it while native `generateContent` returns the 400 shown in the next section. The Vertex copy of that page still describes the mapping only for Gemini 3, so test it on your own project before code depends on it.

## 5. Errors that break Gemini 3 era code

Neon drops two thinking controls that earlier Gemini models accepted. Both fail hard with HTTP 400 rather than being ignored, which is the better behaviour, but it means any shared client config needs a per model branch.

### “Thinking level is unsupported: THINKING_LEVEL_MINIMAL”

Neon’s lowest level is LOW. A shared client config that sends MINIMAL to every Gemini model gets INVALID_ARGUMENT back from Neon:

```
curl -s "https://aiplatform.us.rep.googleapis.com/v1/projects/${PROJECT_ID}/locations/us/publishers/google/models/gemini-4-neon:generateContent" \
  -H "Authorization: Bearer $(gcloud auth print-access-token)" -H "Content-Type: application/json" \
  -d '{"contents":[{"role":"user","parts":[{"text":"Say ok"}]}],"generationConfig":{"thinkingConfig":{"thinkingLevel":"MINIMAL"}}}' \
  | jq .error
```
The error body names the enum value exactly:

Map MINIMAL to LOW for Neon in whatever layer picks the level, because LOW is the closest setting the model accepts and it still thinks: our one word answers at LOW used between 25 and 402 thinking tokens.

### “Thinking budget is not supported for this model. Use thinking level instead.”

Code still sending a token count in `thinkingBudget` gets this 400. We also sent a budget of 0 and it failed the same way, so there is no way to run Neon without thinking:

```
curl -s "https://aiplatform.us.rep.googleapis.com/v1/projects/${PROJECT_ID}/locations/us/publishers/google/models/gemini-4-neon:generateContent" \
  -H "Authorization: Bearer $(gcloud auth print-access-token)" -H "Content-Type: application/json" \
  -d '{"contents":[{"role":"user","parts":[{"text":"Say ok"}]}],"generationConfig":{"thinkingConfig":{"thinkingBudget":1024}}}' \
  | jq .error
```
The response points to the thinking docs:

Google’s developer guide adds that `frequency_penalty`, `presence_penalty` and `candidate_count` above 1 also return 400 on Neon. We did not send those three, so that part is from the guide.

### Temperature is accepted and ignored

The model page says temperature is fixed at 1.0, topP at 0.95 and topK at 64, and that custom values are ignored. Our test agrees, and the important detail is that Neon does not reject an in-range value (out-of-range values still return 400). A request with `temperature` set to 0 returns HTTP 200 like any other, so nothing in the response body says the setting was dropped. The Gemini API adds deprecation warning headers for it and says a future release will return 400. We asked for a Linux distribution name three times at temperature 0 and LOW thinking through the lab harness, then printed the saved results:

`jq -r '[input_filename, .thoughts, .text] | @tsv' runs/temp0-1.json runs/temp0-2.json runs/temp0-3.json`
The three saved responses show the thinking tokens and the answer for each run:

Two of three runs said “Vespera” and one said “Vespera Linux”, and thinking ran from 285 to 402 tokens. If temperature 0 applied, the same prompt would usually come back the same each time. Three runs is a small sample, but it lines up with the documented fixed temperature. Code that depends on temperature 0 for repeatable output, such as test fixtures, eval harnesses or cached classifiers, needs another mechanism on Neon: check the output shape in code and compare answers by meaning, not by exact string.

### “TPM quota exhausted. No spare capacity is currently available”

Launch capacity was the biggest obstacle in this lab. On a Standard pay as you go project, most global endpoint calls came back HTTP 429 on the first attempt, and the MEDIUM ladder run needed seven 30 second retries before it was served. Later in the morning the global endpoint refused every request for over 20 minutes, even a two word prompt:

```
curl -s "https://aiplatform.googleapis.com/v1/projects/${PROJECT_ID}/locations/global/publishers/google/models/gemini-4-neon:generateContent" \
  -H "Authorization: Bearer $(gcloud auth print-access-token)" -H "Content-Type: application/json" \
  -d '{"contents":[{"role":"user","parts":[{"text":"Say ok"}]}],"generationConfig":{"thinkingConfig":{"thinkingLevel":"LOW"}}}' \
  | jq .error
```
It returned the 429 body below. During the same outage our sshd test prompt failed on global and was served on the `us` endpoint, which is how we finished the lab:

Most 429 bodies carry only that generic text. One response our harness saved during the ladder spelled out the real cause, read here from that saved file:

`jq -r .error.error.message runs/429-tpm-quota.json`
The message points at the quota page and the sales team rather than at a retry header:

For now, retry with backoff and keep the `us` or `eu` endpoint as a fallback; the 10% premium is cheaper than a stalled pipeline. If Neon is on a production path, Priority tier at $5.40/$27 is the paid route Google prices for it. The 429s themselves cost nothing, since only HTTP 200 responses are billed.

## 6. Implicit caching and the 4,096 token floor

Neon supports implicit caching only. There is no explicit cache object to create until TTL cache control arrives on January 12, 2027, which is also when the $3.75 cache write price starts. Implicit hits need a prompt prefix of at least 4,096 tokens, and a hit bills that prefix at $0.15 instead of $3.00 per million. That is a 95% cut on the rate card, even though Google’s caching overview still describes implicit hits as a 90% discount, so check the cached line on your first invoice. Neon is also on the short list of models with durable caching, where an evicted prefix moves to slower storage and can still hit, though no entry lives past 24 hours and most are evicted sooner.

We tested the floor with a system prompt built from the bash builtins manual and one short question, sent twice about 35 to 45 seconds apart on the `us` endpoint (one 429 retry each). The harness saved both runs, and the summary script shows the prompt was 2,295 tokens, under the minimum:

`python3 summarize.py cache-small-1 cache-small-2`
Both calls report `cached=0`, so the second identical request paid full input price:

The second half of the test used the same document at nearly three times the length, so the prefix clears 4,096 tokens. It sat in 429s for most of the morning and went through on the global endpoint once capacity eased. Run 2 followed run 1 by about five seconds, and run 3 came a little over a minute later after two 429 retries:

`python3 summarize.py cache-large-1 cache-large-2 cache-large-3`
The first call cached nothing, which is expected because it wrote the prefix. Both repeats then reported `cached=4433` out of 6,487 prompt tokens:

The hit is real, but it covers about 68% of the prompt, not all of it. At global list prices the input side of the call fell from $0.0195 to $0.0068, a 65% cut, and the whole call from $0.0251 to $0.0121, because thinking and output tokens are never cached. So the 95% figure applies only to the cached block, and caching does nothing for the answer side of the bill. The hit also survived the gap of more than a minute before run 3. Log `cachedContentTokenCount` on your own long prompts before a budget counts on a ratio, since the cached block size is the cache’s decision, not yours. Google’s own advice for raising the hit rate is to put large, common content at the start of the prompt and to send requests with the same prefix close together, so keep the changing question last.

## 7. Is Gemini 4 Neon free?

No. Neon has no free tier on the Gemini API, and every consumer surface that offers it sits behind a paid plan. Here is where you can reach it as of November 19, 2026:

- **Gemini API in Google AI Studio:** paid tier only, at the $3/$15 rate. Hydrogen is the Gemini 4 model with a free tier.
- **Vertex AI:** GA in`global` ,`us` and`eu` . The developer guide describes it as available to verified enterprise customers in select regions; our lab project reached it on the global endpoint without a separate approval step.
- **Google Antigravity:** rolling out to paid Google AI Pro and Ultra subscribers in the desktop app, the CLI and the IDE extensions. Setup for the CLI is in our Antigravity CLI install guide.
- **Gemini app:** Google AI Pro and Ultra subscribers.
- **Gemini for business:** listed in the launch post as a Neon surface.

AI Pro and Ultra subscribers reach Neon through the Antigravity CLI (run `agy models` to see the Neon IDs your plan exposes), because subscription access moved off Gemini CLI on June 18, 2026. For older Gemini models on an API key, the Gemini CLI cheat sheet still has the model selection flags. If you are deciding between clouds for Neon and its rivals, our Bedrock vs Azure AI Foundry vs Vertex AI comparison covers quotas and regional pricing in more depth.

## 8. What “coding score tripled” actually measured

The launch headline that spread fastest was The New Stack’s “Google’s Gemini 4 Neon tripled its coding score without changing the model.” The source is one row in Google’s model card. On SWE-sweep, Neon scored 12.8% with the neutral mini-SWE harness and 39.0% inside Google Antigravity. Same model, different agent harness. Google’s launch post says so directly: Neon “more than triples to a state-of-the-art 39.0% in Google Antigravity showing what the Antigravity harness unlocks.”

These are vendor reported figures from Google’s model card, which we did not reproduce:

| Benchmark (vendor reported) | Gemini 4 Neon | Claude Opus 5.5 | 
|---|---|---|
| Artificial Analysis Intelligence Index | 61 | 59 | 
| Terminal-Bench 4.1 | 65.2% | 65.8% | 
| FrontierCode 2.0 | 46.2% | 47.3% | 
| SWE-sweep, provider harness | 39.0% | 10.1% | 
| SWE-sweep, mini-SWE harness | 12.8% | 3.6% | 
| CursorBench 4.1 | 65.4% | 58.1% | 
| AutomationBench | 56.3% | 42.5% | 

Read the SWE-sweep rows together. The provider harness row compares each vendor’s own agent, so it measures Antigravity as much as Neon. Opus 5.5 still edges Neon on Terminal-Bench and FrontierCode. The “#3 overall” claim in the launch post refers to the Artificial Analysis Intelligence Index at XHIGH, where Neon scores 61; it also ranks first on that firm’s Coding Agent Index at 58.6.

The Hacker News post titled “Gemini 4 Neon looks like an Opus 5.5 competitor” is a projection, not a test. It estimates an Agent Duel Elo of 1,066 for Neon against 1,078 for Opus 5.5 by assuming Neon behaves like a faster Carbon. Treat it as a guess about where Neon will land, nothing more. For a non Google comparison point at a lower price, our Qwen4 Max API guide runs DevOps tasks against a cheaper model.

## 9. Numbers to watch on your first Neon bill

Our lab covers one prompt per level on one launch day, under heavy 429 throttling, so it shows the shape of Neon’s costs rather than a stable average. These are the metrics we would put on a dashboard before routing real traffic to it:

- **Thinking to answer ratio:**`thoughtsTokenCount` divided by`candidatesTokenCount` . We saw 3.5 at LOW and 9.2 at HIGH. If yours sits near 9, most requests are running at the default level without anyone choosing it.
- **Cost per task, not per token:** Neon’s rate is 25% under Carbon’s from December 1, but its thinking volume can erase that. Compare a week of real requests before migrating.
- **429 rate and retry count:** launch capacity on Standard tier was thin. Log retries so you can tell when Google adds capacity and whether Priority tier pays for itself.
- **Completions over 32K tokens:** these bill even on timeout. Alert on any XHIGH request whose latency approaches your client timeout.
- **Cached token share:**`cachedContentTokenCount` over`promptTokenCount` for prompts above 4,096 tokens. Ours settled at 68% on a 6,487 token prompt, which halved the cost of the call. It is the only caching lever you have until January 12, 2027.

Three dates change this page: Carbon and Argon’s price rise on December 1, 2026, Gemini 3.8 Flash’s on January 1, 2027, and explicit cache control on January 12, 2027. Recheck the rate cards against your own token logs on each.
