---
id: collect-261121-veille-ia/gemini-google/gemini-4-argon-pricing-benchmarks-and-api-tested-computingforgeeks
title: "Gemini 4 Argon Pricing, Benchmarks and API Tested | ComputingForGeeks"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-10-13"]
keywords: ["gemini", "argon", "tokens", "output", "thinking", "pricing", "google", "model"]
source: https://computingforgeeks.com/gemini-4-argon-released-features-benchmarks/
source_anchor: ""
source_lines: [1, 382]
section: "Gemini_Google"
---
Every Kubernetes manifest Gemini 4 Argon wrote for us passed the linter. None of them started. We asked for the same nginx Deployment twelve times, at every thinking level Google offers, and all twelve crash-looped on a real k3s cluster.

Gemini 4 Argon went generally available on October 13. Google had announced it two weeks earlier for a small group of cyber defenders.

Below is the API pricing and the date it doubles, the specs, and Google’s benchmark table with what it leaves out. Then our own same-prompt test against Claude Opus 5.5 and GPT-6.1 Sol, plus the thinking-level errors and the free-tier wall we hit.

*Tested October 13, 2026 against the GA model ID `gemini-4-argon`, on launch day.*

## Gemini 4 Argon pricing

Argon costs $2 per million input tokens and $10 per million output tokens until November 30, 2026. From December 1 the price doubles to $4 and $20.

Google calls the lower figure an introductory price, and it’s printed that way on the Gemini API pricing page.

| Mode (per 1M tokens) | Input | Output | Cached input | From Dec 1, 2026 | 
|---|---|---|---|---|
| Standard | $2.00 | $10.00 | $0.10 | $4 / $20 / $0.20 | 
| Batch | $1.00 | $5.00 | $0.05 | $2 / $10 / $0.10 | 
| Flex | $1.00 | $5.00 | $0.05 | $2 / $10 / $0.10 | 
| Priority | $3.60 | $18.00 | $0.18 | $7.20 / $36 / $0.36 | 

There is one rate across the whole 1M-token window. Gemini 3.1 Pro charged more above 200K tokens, and Argon drops that tier.

Output price includes thinking tokens. That matters more here than it did on older Gemini models, because Argon at `high` spent three tokens thinking for every one it showed us.

Caching is implicit only. Repeated prefixes get the 95% discount automatically, but the explicit `cachedContents` API is gone for this model.

Grounding with Google Search gives 5,000 free requests a month across Gemini 3 and newer models, then costs $14 per 1,000.

There is no free tier. Google says so on the pricing page, and our free-tier key hit a quota limit of 0. The exact error is further down.

Vertex AI bills the same list prices on its global endpoint and adds 10% for regional endpoints. It delivers the intro discount as 50% credits back on net spend.

For comparison, Claude Opus 5.5 lists at $4 and $20, and GPT-6.1 Sol at $2 and $10. Until December, Argon matches Sol’s list price. After that it costs the same as Opus.

## Context window, output limit and thinking levels

Google’s announcement says Argon can write up to 1M output tokens. You will not get that from a normal request. The API’s own model entry reports a much smaller number.

`curl -s "https://generativelanguage.googleapis.com/v1beta/models/gemini-4-argon" -H "x-goog-api-key: ${GEMINI_API_KEY}"`
Our key got this back on launch day.

```
{
  "name": "models/gemini-4-argon",
  "version": "3.0",
  "displayName": "Gemini 4 Argon",
  "description": "Gemini 4 Argon",
  "inputTokenLimit": 1048576,
  "outputTokenLimit": 32768,
  "supportedGenerationMethods": [
    "generateContent",
    "countTokens",
    "batchGenerateContent"
  ],
  "temperature": 1,
  "topP": 0.95,
  "topK": 64,
  "maxTemperature": 2,
  "thinking": true
}
```
Google’s docs give 65,536 output tokens per synchronous request, and OpenRouter lists its Vertex route at the same figure. A 100,000-token request there didn’t error, so it gets clamped rather than rejected.

Beyond that you need long decode, a new mode that generates in 64K “hops” and stitches them together. On the Gemini API it runs through the Interactions API with `background: true`. On Vertex the SDK follows continuation tokens for you.

| Spec | Gemini 4 Argon | 
|---|---|
| Model ID | `gemini-4-argon` (GA, no preview suffix) | 
| Context window | 1,048,576 tokens, shared by input, thinking and output | 
| Output per request | 65,536 (32,768 in the model entry and for non-streaming Vertex calls) | 
| Output with long decode | Up to the 1M window, minus input | 
| Thinking levels | `low` ,`medium` (default),`high` | 
| Knowledge cutoff | March 2026 | 
| Input | Text, image, video, audio, PDF | 
| Output | Text only | 
| Architecture | Sparse mixture-of-experts, trained from scratch | 

Watch the thinking default. Gemini 3.1 Pro defaulted to `high`, and Argon defaults to `medium`. An app that never set the level will think less after the switch.

We couldn’t see medium thinking at all. On our three short tasks through OpenRouter, the default, `low` and `medium` runs all reported 0 reasoning tokens and near-identical output.

Only `high` thought, using 1,100 to 1,800 tokens per call. We did not test long or hard prompts, where the lower levels may well think. On routine DevOps files, the three lower settings were the same model to us.

## Gemini 4 Argon benchmarks

Google’s model card compares Argon against three models only. Every Argon number was run at its highest thinking setting.

| Benchmark | Gemini 4 Argon | GPT-6 Astra | Claude Fable 5.1 | Claude Opus 5.5 | 
|---|---|---|---|---|
| Vals Index | **68.9%** | 63.1% | 65.8% | 67.0% | 
| AutomationBench | **51.3%** | 41.4% | 31.4% | 42.5% | 
| Vals Finance Agent v2 | **65.4%** | 53.5% | 58.9% | 58.6% | 
| Harvey’s Legal Agent Benchmark | **19.6%** | 5.4% | 6.7% | 3.8% | 
| DeepSWE v1.1 | **77.9%** | 74.1% | 67.4% | 74.2% | 
| FrontierSWE v2 | 55.0% | **65.5%** | 56.3% | 62.3% | 
| Vibe Code Bench | **91.9%** | 89.6% | 90.3% | 90.3% | 
| Terminal-bench 4.0 | 57.4% | 58.2% | 57.9% | **66.4%** | 
| PostTrainBench | 45.3% | 44.3% | 40.2% | **49.3%** | 
| Terminal-Bench Science 0.1 | 57.6% | **68.1%** | 52.6% | 63.3% | 
| LABBench 2 | **88.8%** | 85.4% | 68.6% | 73.1% | 
| RiemannBench | **76.0%** | 72.0% | 65.6% | 69.6% | 
| GraphWalks BFS up to 128K (F1) | **99.7%** | 98.7% | 91.4% | 90.6% | 
| GraphWalks BFS 256K to 1M (F1) | **84.2%** | 71.8% | 65.0% | 66.8% | 
| Agent’s Last Exam | **39.5%** | 34.2% | n/a | 38.2% | 
| OSWorld-2.0 (offline subset) | 69.2% | **72.6%** | n/a | n/a | 
| Chartography | **71.6%** | 71.0% | 46.2% | 66.3% | 
| LVBench | **91.7%** | 87.5% | 79.7% | 83.7% | 
| CWE-bench v1 | **68.0%** | **68.0%** | 58.0% | 67.0% | 

Argon leads 13 of the 19 rows and ties one. Its biggest margin over Opus 5.5 is long context, with 84.2% on GraphWalks between 256K and 1M tokens. That is 17 points clear of Opus 5.5.

If your workload is a large codebase or a pile of logs in one prompt, that row matters more than the coding rows.

The coding picture is mixed. Argon wins DeepSWE but trails on FrontierSWE v2, and Opus 5.5 beats it by 9 points on Terminal-bench 4.0. Terminal work is the closest row to what our readers do all day.

### What the table leaves out

GPT-6.1 Sol shipped the day before Argon’s announcement and is not in the table. Neither is Claude Sonnet 5.5, which sits second on the Vals Index. GPT-6 Sol and Grok 4.8 show up only in Google’s Gray Swan prompt-injection chart, where Argon wins.

Some rows are self-computed. Google ran Terminal-Bench Science itself with a 6x verifier timeout and got 57.6%.

Vals measured 44.29% on its own harness. Google calls Argon the leader on Harvey’s Legal Agent Benchmark, but the Vals leaderboard for that benchmark puts Meta’s Muse Spark models above it, with Argon sixth.

LVBench gave Gemini 1 frame per second of video and capped the rivals at 300 to 800 frames because of their API limits. There’s no SWE-bench Verified, GPQA or HLE score in the table at all.

### Independent scores

Artificial Analysis scores Argon 53 on its Intelligence Index, tied with GPT-6 Astra and one point above GPT-6.1 Sol.

It uses about 62K output tokens per index task against 27K for Astra. Cost per task is $1.99 at the intro price and $3.98 after it.

Two Artificial Analysis numbers stand out. Argon’s hallucination rate on AA-Omniscience is 15%, against 51% for Astra and 54% for GPT-6.1 Sol.

Its Terminal Bench 4 score of 57% sits behind Sonnet 5.5 (64%) and Opus 5.5 (60%). On the Vals Index, Argon is first at 68.90%, two points ahead of Sonnet 5.5.

## Argon vs Opus 5.5 vs GPT-6.1 Sol on the same DevOps tasks

We reused the harness from our GPT-6 Sol tests, so every model got identical prompts. There were three tasks: a Kubernetes Deployment plus Service for nginx with `runAsNonRoot`, a PostgreSQL backup script that uploads to S3, and an OpenTofu VPC.

Each model ran every task three times.

Argon ran at four settings (default, `low`, `medium`, `high`) through OpenRouter, pinned to Google AI Studio. Opus 5.5 and GPT-6.1 Sol ran the same day on their own APIs at default effort, so latency includes different routes.

Linting wasn’t the test. We deployed every manifest on k3s v1.36.5 and curled the Service.

Every bash script ran against stubbed `pg_dump` and `aws` binaries, with and without its env vars, and with dump files aged 3 and 10 days.

| Result | Argon (default) | Argon (high) | Opus 5.5 | GPT-6.1 Sol | 
|---|---|---|---|---|
| kubeconform, shellcheck, tofu validate | 9/9 clean | 9/9 clean | 9/9 clean | 9/9 clean | 
| k8s pods Ready + HTTP 200 | **0/3** | **0/3** | 3/3 | 3/3 | 
| Backup script behaviour | 3/3 | 3/3 | 3/3 | 3/3 | 
| Avg output tokens | 463 | 1,836 | 1,362 | 658 | 
| Avg latency | 5.5 s | 15.3 s | 12.1 s | 18.7 s | 
| Cost, 9 calls (Argon at intro price) | $0.045 | $0.169 | $0.254 | $0.062 | 

The `low` and `medium` runs matched the default row almost exactly, so they’re left out of the table. Each scored 0/3 on Kubernetes and 3/3 on bash for about $0.044. Across all four settings, Argon’s twelve manifests went 0 for 12.

### Error: mkdir() “/var/cache/nginx/client_temp” failed (13: Permission denied)

Every Argon manifest set `runAsUser: 101` on the stock `nginx:1.27-alpine` image and mounted nothing writable.

The image expects root to create its cache directories at start, so nginx dies before it binds a port.

Below is the first default-level manifest, exactly as Argon returned it. Dropped capabilities, probes and limits are all there.

It just cannot run.

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web
  labels:
    app: web
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
    spec:
      securityContext:
        runAsNonRoot: true
        runAsUser: 101
        runAsGroup: 101
        fsGroup: 101
      containers:
        - name: web
          image: nginx:1.27-alpine
          ports:
            - containerPort: 80
              name: http
              protocol: TCP
          securityContext:
            runAsNonRoot: true
            runAsUser: 101
            allowPrivilegeEscalation: false
            capabilities:
              drop:
                - ALL
              add:
                - NET_BIND_SERVICE
          resources:
            requests:
              cpu: 100m
              memory: 128Mi
            limits:
              cpu: 250m
              memory: 256Mi
          readinessProbe:
            httpGet:
              path: /
              port: 80
            initialDelaySeconds: 5
            periodSeconds: 10
          livenessProbe:
            httpGet:
              path: /
              port: 80
            initialDelaySeconds: 15
            periodSeconds: 20
---
apiVersion: v1
kind: Service
metadata:
  name: web
  labels:
    app: web
spec:
  type: ClusterIP
  selector:
    app: web
  ports:
    - name: http
      protocol: TCP
      port: 80
      targetPort: 80
```
Opus 5.5 and GPT-6.1 Sol both mounted `emptyDir` volumes on `/var/cache/nginx` and `/var/run` in all three runs. GPT-6 Luna made Argon’s exact mistake in September. A model that writes a perfect-looking manifest without knowing what the image does at start is the reason we deploy instead of lint.

Then we gave Argon its own manifest and the error line and asked for a fix that keeps `runAsNonRoot`. All three fixes deployed and answered HTTP 200.

Two swapped the image for `nginxinc/nginx-unprivileged:1.27-alpine` on port 8080, which works but quietly drops the image and port we specified.

One kept both and added three `emptyDir` mounts, the answer Opus and Sol gave the first time. That volume section is the part that was missing.

```
          volumeMounts:
            - name: cache
              mountPath: /var/cache/nginx
            - name: run
              mountPath: /var/run
            - name: tmp
              mountPath: /tmp
...
      volumes:
        - name: cache
          emptyDir: {}
        - name: run
          emptyDir: {}
        - name: tmp
          emptyDir: {}
```
Bash and OpenTofu were clean for everyone.

All 12 Argon backup scripts exited 1 with a clear message when `DB_NAME` was unset, wrote a valid gzip, uploaded once and deleted only the 10-day-old dump.

Every Terraform file validated and passed `tofu fmt`. Every model on this prompt, Argon included, pinned the AWS provider to the 5.x line.

## How fast Gemini 4 Argon streams

Argon streams fast once it starts. We streamed the OpenTofu task three times at the default level and three at `high` from the lab VM.

The default level put out 258 to 299 visible tokens per second, with the first token after about 4.3 seconds. At `high` the first visible token took 12 to 13 seconds, because about 1,400 tokens of thinking came first. End to end, the default runs averaged about 110 tokens per second. GPT-6.1 Sol managed about 81 tokens per second after its first token in our earlier probe, on a different prompt through OpenAI’s own API, so treat that comparison as rough.

## Call Gemini 4 Argon from the API

You need a paid Gemini API project. Tier 1, the first paid tier, only needs billing linked.

Set the thinking level in `generationConfig` and give it room to think. Google recommends `maxOutputTokens` of at least 32,768. This is the request shape we sent; our free-tier key got the 429 error shown below instead of an answer.

```
curl -s "https://generativelanguage.googleapis.com/v1beta/models/gemini-4-argon:generateContent" \
  -H "x-goog-api-key: ${GEMINI_API_KEY}" \
  -H "Content-Type: application/json" \
  -d '{"contents":[{"role":"user","parts":[{"text":"Write a systemd unit for a Go binary at /usr/local/bin/api."}]}],"generationConfig":{"thinkingConfig":{"thinkingLevel":"high"},"maxOutputTokens":32768}}'
```
The same model is on OpenRouter as `google/gemini-4-argon`, which is how we ran most of our tests. Its `reasoning.effort` field maps to the thinking level.

```
curl -s https://openrouter.ai/api/v1/chat/completions \
  -H "Authorization: Bearer ${OPENROUTER_API_KEY}" \
  -H "Content-Type: application/json" \
  -d '{"model":"google/gemini-4-argon","reasoning":{"effort":"high"},"max_tokens":32000,"provider":{"only":["google-ai-studio"]},"messages":[{"role":"user","content":"Write a systemd unit for a Go binary at /usr/local/bin/api."}]}'
```
OpenRouter lists two AI Studio variants besides the standard one: `google-ai-studio/flex` at $1 and $5, and a priority route at $3.60 and $18.

If you already use Google’s SDK, our Vertex AI Python streaming guide works with Argon once you change the model ID. Google lists `google-genai` 2.30.1 as the minimum Python SDK.

### Error: Quota exceeded for metric generate_content_free_tier_input_token_count, limit: 0

Our first Argon call came back HTTP 429 on a key with no billing attached.

```
You exceeded your current quota, please check your plan and billing details. For more information on this error, head to: https://ai.google.dev/gemini-api/docs/rate-limits. To monitor your current usage, head to: https://ai.dev/rate-limit.
* Quota exceeded for metric: generativelanguage.googleapis.com/generate_content_free_tier_input_token_count, limit: 0, model: gemini-4-argon
* Quota exceeded for metric: generativelanguage.googleapis.com/generate_content_free_tier_requests, limit: 0, model: gemini-4-argon
* Quota exceeded for metric: generativelanguage.googleapis.com/generate_content_free_tier_requests, limit: 0, model: gemini-4-argon
* Quota exceeded for metric: generativelanguage.googleapis.com/generate_content_free_tier_input_token_count, limit: 0, model: gemini-4-argon
Please retry in 7h9m26.492488377s.
```
Waiting will not fix it. The limit is 0, so the retry timer means nothing here.

Link a billing account to the project in AI Studio, or use a key from a project that already has one. The model still shows up in `/v1beta/models` for free keys, which is what makes this confusing.

### Error: Invalid value at ‘generation_config.thinking_config.thinking_level’

Sending `"thinkingLevel": "max"` or `"xhigh"`, the names OpenAI and Anthropic use, fails validation with HTTP 400 before quota is checked.

`Invalid value at 'generation_config.thinking_config.thinking_level' (type.googleapis.com/google.ai.generativelanguage.v1beta.ThinkingConfig.ThinkingLevel), "max"`
Argon’s top level is `high`. Google’s Vertex migration guide also says `thinkingBudget` and `MINIMAL` now return 400, where Gemini 3.1 Pro mapped them to a level. On the Gemini API our free key got 429 for both, not 400, so we could not confirm it there.

OpenRouter translates both for you, so our calls with a reasoning token budget and with `effort: minimal` went through. Native Vertex code will not be that forgiving.

### Empty response with finish reason MAX_TOKENS

At `high` with `max_tokens` set to 1,024, Argon spent 1,021 tokens thinking and returned an empty string. The finish reason was `MAX_TOKENS`, and all 1,024 tokens counted as output.

Old Gemini code with an 8K output cap can hit the same thing on hard prompts. Raise the cap before you switch the model ID.

## Moving from Gemini 3.1 Pro

Changing the model ID is not the whole job. These changes come from Google’s migration notes for Argon (the Gemini API page and the Vertex guide), and the first three will break existing code.

- `thinkingBudget` and the`MINIMAL` level return HTTP 400 on Vertex, per its migration guide. Use`thinkingLevel` with`low` ,`medium` or`high` .
- Explicit context caching (`cachedContents` ,`caches.create` ) is unsupported. Implicit caching still gives 95% off repeated prefixes of 4,096 tokens or more on the Gemini API.
- On Vertex, `frequency_penalty` ,`presence_penalty` and`candidate_count` return 400, and a conversation cannot end on a prefilled model turn.
- `temperature` ,`top_p` and`top_k` are accepted and silently ignored.
- The default thinking level drops from `high` to`medium` . Set it explicitly if your prompts relied on deep reasoning.
- Hard prompts can run for minutes. Google suggests client timeouts of 15 minutes or more.

Long decode has a counting quirk too. The reported input token count grows with every hop, because each hop resends the conversation.

Do not trigger context compaction from that number. Google’s docs say to use `initial_prompt_token_count` instead.

## Where Gemini 4 Argon is available

The API went live first. Consumer access depends on your Google AI plan, and some developer tools haven’t caught up.

| Surface | Status on October 13, 2026 | 
|---|---|
| Gemini API (AI Studio) | GA, paid tier only | 
| Vertex AI | GA in `global` ,`us` and`eu` ; batch, flex, priority and provisioned throughput | 
| OpenRouter | `google/gemini-4-argon` via AI Studio and Vertex | 
| Gemini app and AI Mode in Search | Google AI Pro and Ultra, rolling out | 
| AI Studio web UI | Google AI Ultra; Pro plan not included | 
| Google Antigravity app and CLI | Google AI Ultra and eligible Gemini Enterprise plans | 
| Gemini CLI | Not yet (GitHub issue #29689) | 
| GitHub Copilot, Cursor | Not announced | 

Pro subscribers on launch day reported not seeing Argon in the Gemini app yet. If you live in the terminal, the Antigravity CLI has it today on Ultra and eligible Gemini Enterprise plans. The Gemini CLI still tops out at 3.1 Pro.

Fairwind Program partners, a vetted group of government, healthcare and telecom security teams, get a separate `gemini-4-argon-cyber-permissive` model without the offensive-security guardrails. Google’s model card says it could not rule out the cyber and biological critical capability levels for Argon, or a stealth and situational awareness threshold. Safety cases cover all three. Google described the two-week gap as “a phased approach” to releasing frontier capabilities.

## Should you switch before December 1?

For long-context work, yes.

Nothing else in Google’s table or ours comes close to 84% on million-token graph traversal, and at $2 and $10 it’s cheap to try. Load the whole repo or a week of logs into one prompt and see what it finds.

For agentic DevOps work, keep a human or a test cluster in the loop. Argon wrote twelve linter-clean manifests that never served a request.

It fixed them when shown the error, but Opus 5.5 and GPT-6.1 Sol got it right the first time. Our AI coding agent comparison covers the agents that close that loop for you.

Then check the bill. At default thinking, Argon cost less than GPT-6.1 Sol on our tasks and finished each call about three times sooner.

From December 1 the same nine calls cost about $0.09, and Sol’s $0.06 wins. Route the long-context jobs to Argon, and run your own numbers again in December.
