---
id: collect-261121-veille-ia/modeles-open/browse-models-ai-reserve-developer-documentation
title: "Browse models - AI Reserve Developer Documentation"
domain: modeles-open
role: reference
task: article
actors: []
dates: []
keywords: ["models", "model", "same", "retired", "only", "tokens", "date", "kimi-k2"]
source: https://docs.aireserve.com/models/index
source_anchor: ""
source_lines: [1, 430]
section: "Modeles_Open"
---
`model` / `modelId` string works across the Converse SDKs, the OpenAI-compatible
endpoints, the Anthropic-compatible `/v1/messages` route, and the Bedrock wire-format
routes. Pick a modality, narrow by maker or capability, and every card gives you the id,
the aliases that resolve to it, and a request you can paste straight into a terminal.
**Prefer a plain list?**The same catalog is published as static pages per modality — Text & chat, Image, Video, Audio & voice, Embeddings, Decisions, Extractions, Live search — and every model has its own page under the

**Models**tab with SDK examples, limits and same-weights alternatives.

`GET /v1/models` returns the OpenAI-shaped list, filtered to what **your**organization can call (China-hosted and consent-gated models appear only once an administrator has opted in), so model pickers and framework integrations that enumerate models work unchanged. Retired model IDs return

`404 ResourceNotFoundException` — enumerate `/v1/models` rather
than hardcoding lists.
**Reasoning models and small**Models that think before answering (

`max_tokens`.`gemini-3-flash-preview`, `gpt-5.x`,
`gpt-6-astra`, `grok-4-1-fast-reasoning`, …) spend part of the token budget on internal reasoning. With a tight cap
(e.g. `max_tokens: 50`) the budget can be consumed before any visible text is produced — the response then comes back
with `finish_reason / stop_reason = max_tokens` (or `length`) and empty content. That’s the model, not an error: raise
`max_tokens` (a few hundred is a safe floor for reasoning models) or pick a non-reasoning model for short outputs.
- **At least 30 days of notice.** When a model must leave the catalog, the retirement
date is announced a minimum of 30 days ahead. Until that date the model keeps
working normally.
- **How notices arrive.** Your organization’s admins receive an email naming the
model, the retirement date, and what to use instead. It goes to every organization
that used the model in the previous 60 days, and the same notice stays visible on
the admin Models tab until the date passes.
- **Upstream retirements are absorbed where possible.** When a provider retires a
model or an alias on their side, we keep the catalog ID working wherever we can by
pointing it at the successor. Precedent: DeepSeek retired its`deepseek-chat` and`deepseek-reasoner` upstream aliases in July 2026, and both IDs kept working
unchanged on this platform. Each now calls the explicit DeepSeek V4 successor with
its original thinking behavior preserved. Likewise, when Together AI withdrew
serverless serving of several open-weight deployments in September 2026, the
affected IDs were repointed at the same weights on a surviving provider:`kimi-k2.7-code` →`kimi-k2.7-code-moonshot` ,`kimi-k2.6-together` →`kimi-k2.6-moonshot` (both China-hosted and consent-gated — the re-route serves
only after your organization’s opt-ins, below),`minimax-m2.5` →`minimax-m2.5-tensormesh` (consent-gated), and`nemotron-3-ultra-together` →`nemotron-3-ultra` ; and when Baseten deprecated
its GLM 4.7 deployment in October 2026,`glm-4.7-baseten` was repointed at the
same weights on Novita AI (`glm-4.7-novita` , same price and context window). A
deprecation notice is sent only when an ID truly cannot be preserved; an ID
retired upstream with no successor returns an explicit “retired by its
provider” error rather than a generic 404 (as`mistral-small` does since
September 2026, when OpenRouter withdrew its last endpoint;`nemotron-3-nano-nvidia` since August 2026 and`nemotron-3-super-nvidia` since October 2026, when NVIDIA retired the hosted endpoints — the
DeepInfra-served`nemotron-3-nano` and`nemotron-3-super` remain available;
and`minimax-m2.7` since September 2026, when Together withdrew its only
serving).

*via AWS Bedrock serving*under

`-bedrock` IDs — same shapes, same key, one-string A/B against the direct
path — including the current Claude line (`claude-fable-5-2-bedrock`,
`claude-fable-5-1-bedrock`, `claude-fable-5-bedrock`, `claude-opus-5-5-bedrock`,
`claude-opus-5-bedrock`, `claude-sonnet-5-5-bedrock`, `claude-sonnet-5-bedrock`,
`claude-haiku-5-5-bedrock`, `claude-opus-4-8-bedrock`, `claude-opus-4-7-bedrock`) and, since November 2026, the GPT line
(`gpt-6.1-sol-bedrock`, the `gpt-6-*-bedrock` and `gpt-5.6-*-bedrock` tiers,
`gpt-5.5-bedrock`, `gpt-5.4-bedrock`), `grok-4-6-bedrock` and the Kimi, MiniMax,
Qwen, Nemotron and Llama 3.3 open-weight legs. See
Bedrock-routed models for the full mapping.
On the Bedrock wire-format routes (`/model/{id}/converse[-stream]`) the
gateway additionally accepts native AWS model IDs (`us.anthropic.…`,
inference-profile ARNs) so unmodified boto3 code works without renaming models — see
AWS SDK (boto3).
Three Claude models are additionally offered *via Google Vertex AI serving*under

`-vertex` IDs — `claude-sonnet-4-6-vertex`, `claude-sonnet-4-5-vertex`, and
`claude-haiku-4-5-vertex` — same shapes, same key, one-string A/B against the direct
path. Google operates the model inside Google Cloud infrastructure in the United States
(us-east5) as a data processor; the model’s publisher does not receive prompts or
completions. See the
provider data-handling disclosures
for the full routing notes.
More Vertex AI routes are being verified: the Gemini line, the Claude 4.6+ / 5.x line
and Model Garden partner models (xAI Grok, Z.ai GLM, DeepSeek, Meta Llama 4, OpenAI
open weights) under the same `-vertex` suffix. A route appears in this catalog the day
its gateway verification passes; until then its id is not listed by `GET /v1/models`.
Microsoft Azure AI Foundry is being onboarded as a serving provider under `-azure` IDs:
Azure OpenAI deployments of the GPT line, and Foundry Models sold directly by Azure for
Anthropic Claude, xAI Grok, DeepSeek, Moonshot Kimi, Meta Llama, Mistral and OpenAI
open weights. Microsoft operates the model inside Azure in the United States as a data
processor; the model’s publisher does not receive prompts or completions, and
Microsoft’s abuse-monitoring retention (up to 30 days on our account type) applies —
see the provider data-handling disclosures.
The first `-azure` routes are live and listed in this catalog since November 2026 (the
GPT line, the Claude Opus 4.6 – 5.5 line and Claude Sonnet 5.5, Grok, DeepSeek V4 Pro,
Kimi K2.7 Code, Llama 4 Scout, Mistral Large and gpt-oss-120b); each remaining route
appears the day its gateway verification passes.
Several open-weight models are served by more than one disclosed provider. DeepSeek
V4.1 Flash uses `deepseek-v4.1-flash` for its US-hosted DeepInfra primary,
`deepseek-v4.1-flash-novita` for the same weights served by Novita AI (US), and
`deepseek-flash` for the separately consent-gated DeepSeek official endpoint. A provider
suffix pins other requests to a provider’s deployment of the same weights — for example
`deepseek-v4-flash-deepinfra` / `deepseek-v4-flash-novita` (DeepInfra and Novita AI
serving of DeepSeek V4 Flash), `qwen-3.8-max-deepinfra` / `qwen-3.8-max-novita`,
`glm-5.3-flash-baseten`, `glm-5.2-together`, or `kimi-k3-fireworks`. Baseten (US)
serves `-baseten` variants of GLM 5.3 (`glm-5.3-baseten`), GLM 5.2 (`glm-5.2-baseten`),
DeepSeek V4 Pro (`deepseek-v4-pro-baseten`), Kimi K2.6 (`kimi-k2.6-baseten`), and
Kimi K2.7 Code (`kimi-k2.7-code-baseten`). Z.AI’s GLM 4.7 (200K context) serves
entirely through provider-suffixed variants — `glm-4.7-novita` (primary),
`glm-4.7-deepinfra`, and `glm-4.7-bedrock` (all US; the AWS route is the same
weights on Amazon Bedrock, see Bedrock-routed variants;
the `glm-4.7-baseten` variant retired with Baseten’s
deployment in October 2026 and now resolves to `glm-4.7-novita`). The `-streamlake` variants
(`deepseek-v3.2-streamlake`, `deepseek-v4-flash-streamlake`, `deepseek-v4-pro-streamlake`,
`glm-5.2-streamlake`, `kimi-k2.6-streamlake`, `kimi-k2.7-code-streamlake`,
`minimax-m2.5-streamlake`) are served by StreamLake, Kuaishou’s international AI cloud —
they are labeled China-based and additionally consent-gated (see
China-hosted models and
Organization consent gates below). The suffix names the serving provider whose
data-handling terms apply — the
model’s original publisher never receives requests on these routes. Unsuffixed
open-weight IDs default to a disclosed US serving provider — for example the Nemotron 3
family (`nemotron-3-super`, `nemotron-3-nano`, `nemotron-3-ultra`) has been served by
DeepInfra since August 2026.
Same weights, different machines: output speed, time to first token, and prompt-cache
behavior are the provider’s, so two suffixes of one model can differ by 2–3× in tokens
per second (measured across the `gpt-oss-120b` legs, October 2026). The unsuffixed ID is
the default we picked for speed and reliability; reach for a suffix when you need a
specific provider’s data-handling terms or want to compare. The portal’s Analytics
**Usage by Model**table breaks a model out by serving provider (once more than one has served it) with p50 / p95 latency and cache-hit rate on your own traffic, which is the fairest comparison. Since September 2026, GMI Cloud (US) serves seven

`-gmicloud` variants:
`deepseek-v4-flash-gmicloud`, `deepseek-v4-pro-gmicloud`, `glm-5.2-gmicloud`,
`glm-5.3-gmicloud`, `glm-5.3-flash-gmicloud`, `kimi-k2.6-gmicloud`, and
`kimi-k2.7-code-gmicloud` — same weights as each base model, often at the lowest
US-hosted rate for those weights (`deepseek-v3.2-gmicloud` retired with GMI’s V3.2
serving in September 2026 and resolves to `deepseek-v3.2`). GMI Cloud publishes no
API-content retention terms yet, so the `-gmicloud` variants require the organization
data-retention consent (see “Organization consent gates” below).
Since October 2026, Sail Research serves the `-sail` variants `kimi-k3-sail`,
`glm-5.3-sail`, `glm-5.3-flash-sail`, `deepseek-v4.1-flash-1017-sail`,
`deepseek-v4-pro-sail`, `deepseek-v4-flash-sail`, and `gemma-4-31b-sail` — same weights
as each base model — plus one Sail-only entry, `gemma-4-31b-nvfp4` (an NVIDIA NVFP4 build
of Gemma 4 31B). Sail’s published terms are zero data retention by
default (inference prompts and outputs are processed only in memory and never persistently
stored) with no training on inference data without written consent, so no organization
consent gate applies to these variants; Sail’s processing region is not fixed (multi-region
by default) — see the
provider data-handling disclosures for its
row.
Since November 2026, Inworld’s own US-hosted inference serves the `-inworld` variants
`glm-5.3-inworld`, `glm-5.3-flash-inworld`, `glm-5.2-inworld`, `deepseek-v4-pro-inworld`,
`deepseek-v4-flash-inworld`,
`deepseek-v4.1-flash-inworld`, `deepseek-v4.1-flash-1017-inworld`, `deepseek-v3.2-inworld`,
`qwen-3.8-flash-inworld`, `qwen-3.8-27b-inworld`, `muse-spark-open-inworld`, and
`gemma-4-31b-inworld` — same weights as each base model — plus four unsuffixed entries,
`glm-5.1`, `kimi-k3.1-pro`, `gemma-4-26b-a4b`, and `gpt-oss-safeguard-120b`. These
are the models Inworld hosts itself, reached on the regular `/v1` endpoints (chat
completions, responses, messages) with your AI Reserve key; Inworld’s resale of other
providers’ models is not routed, and Inworld’s voice stack keeps its own
`/inworld` passthrough. Inworld’s terms rule out training on
customer content, but it retains request content by default until its Zero Data
Retention setting is enabled for the workspace — not yet verified for AI Reserve’s — so
the Inworld variants require the organization data-retention consent (see “Organization
consent gates” below). Inworld serves some of these groups at a smaller context window
than the base model’s other hosts (for example 524K on GLM 5.3 Flash and DeepSeek V4.1
Flash); each variant’s page carries its own figure.
Novita AI retires its DeepSeek V4 Flash serving on **October 9, 2026 at 16:00 UTC**(Novita changelog).

`deepseek-v4-flash-novita`,
`deepseek-v4-flash-reasoner-novita`, `deepseek-v4-flash-vision-exp-novita`, and
`gpt-oss-20b-novita` keep working until then and are no longer fallback destinations for
any other route. Novita’s designated successor is DeepSeek V4.1 Flash, available now as
`deepseek-v4.1-flash-novita` — a different model, so it is never substituted silently;
switch to it (or to another serving variant of the same V4 Flash weights, such as
`deepseek-v4-flash-deepinfra`) before the cutoff. Organizations that used the retiring
variants recently receive the standard deprecation notice (below).
Don’t want to pick a provider suffix yourself? Append a routing variant to the model ID
and the gateway picks the serving provider per request:
- **`:auto`** — your organization’s**provider preference** from the portal’s Router
page: a balance of price, latency and reliability scored per request on live signals
(the current rate card, trailing latency, success and rate-limit rates, provider
account health), or**fixed traffic shares** per provider (for example DeepInfra 60% /
Baseten 40%), or a**pinned provider** , plus any excluded providers. The leg that serves
a conversation stays sticky so provider prompt caches stay warm; a provider that is
down or rate-limited hands its share to the others. Unset, the preference is**Balanced** .
- **`:nitro`** — the fastest provider currently serving those weights (rolling median
latency on live traffic). Alias:`:fastest` .
- **`:floor`** — the cheapest provider by blended token price. Alias:`:cheapest` .
- **`:online`** — not a routing variant but it rides the same place: the gateway runs one
live web search on your latest
message and hands the model the sources before answering. Combines with one variant
(`deepseek-v4-flash:online:nitro` ).

`x-aireserve-served-model` header name the provider-suffixed
ID that actually served (for example `deepseek-v4-flash-deepinfra`), and
`x-aireserve-served-provider` names the provider, so the serve is always disclosed. Send
`x-aireserve-route-reason: 1` to get the decision back as a compact response header
(`preset=balanced; served=deepinfra; scores=deepinfra:0.91,baseten:0.78; …`). Variants
never change what you’re allowed to use: access rules, consent gates, and model
allowlists apply exactly as they do without the suffix. Your failover protection also
survives the reroute: the model’s fallback chain still applies
to whichever provider the variant picks, minus any provider your preference excludes
(fallbacks are opt-in per organization or per request; an organization that has at least
one fallback chain configured also keeps the platform’s standard provider failover for
its other models; ask support to change either). On models with a single serving
provider, on providers we haven’t yet verified with live production traffic, and on the
China-hosted models — which always serve exactly the provider
their ID promises — the suffix is accepted and simply serves the model unchanged.
Migrating from OpenRouter? `:nitro` and `:floor` work as-is, and the request-level
`provider` object is honored on `/v1/chat/completions` — it is consumed by the gateway
and never forwarded upstream:
`only`, `allow_fallbacks: false`, `max_price`, `zdr` / `data_collection`) are
contracts: the gateway honors them or refuses the request, never widens around them —
and a provider they exclude is also removed from the request’s failover chain. A `503`
from a restriction carries the restriction’s name in the error message. Preferences
(`sort`, `ignore`, a bare `order`) never fail a request. A `provider` block that sets no
`sort` or `order` keeps the suffix’s meaning, so `deepseek-v4-flash:floor` with
`{ "provider": { "zdr": true } }` is the cheapest zero-retention provider.
Provider names are matched case-insensitively against our serving-provider labels
(`baseten`, `deepinfra`, `together`, `novita`, `fireworks`, `openai`, `anthropic`,
`bedrock`, `vertex`, `gemini`, `grok`, `mistral`, …); OpenRouter display names such as
`"Together AI"`, `"Amazon Bedrock"` or `"xAI"` are accepted too.
An enterprise admin can make `zdr: true` the organization’s standing policy instead of a
per-request flag: **Admin → your organization → Models → Zero-retention providers only**. While the switch is on, every text request from the organization — web chat and every API key — is treated as if it carried

`{ "provider": { "zdr": true } }`, and the two compose
by the strictest rule: a request can add the restriction but never relax it (`"zdr": false`
or an `order` naming a retaining provider changes nothing). It is strict by design:
- **Refused** : every first-party model — OpenAI, Anthropic, Google Gemini, xAI, Mistral,
Amazon Bedrock and Google Vertex AI — because those providers retain prompts under their
own terms. The request is not served elsewhere; it fails with`503 SERVICE_UNAVAILABLE` ,`details.reason: "provider_route_unavailable"` ,`details.cause: "retention"` , and a
message that names the qualifying providers and, when the same weights are served
zero-retention under another ID, those IDs:

- **Keeps working** : every model served by Together AI, Fireworks AI, DeepInfra, Novita AI,
Baseten or Cloudflare Workers AI. On`/v1/chat/completions` , provider-routed families
(`deepseek-v4-flash` , the`:auto` /`:nitro` /`:floor` variants) keep routing, scored
only among those providers, and their failover chains drop every retaining provider — a
rescue never lands on one. On`/v1/messages` ,`/v1/responses` and Bedrock Converse the
model you name is served exactly as requested when its provider qualifies and refused
otherwise; those routes never re-route.
- **Fails closed** : if the policy cannot be read, the request is refused rather than
served by a retaining provider. A gateway instance that has read your organization’s
setting before keeps applying its last-known value through a database blip; one that
has not retries the read once and then answers the same`503` with`details.cause: "retention_policy_unavailable"` , the message`The organization's data-handling policy could not be read; retry shortly.` and a`Retry-After: 30` header. Retry the request; nothing was dispatched.

**Models**pages carry a

**Zero data retention**badge on every model all of whose serving providers qualify (with an

*N of M providers*note where only some do), and Auto routing skips the refused models. The switch takes effect within about 30 seconds of being changed, and each change is recorded in the organization’s admin activity log. It covers the text routes today —

`/v1/chat/completions`, `/v1/messages`, `/v1/responses` and Bedrock Converse;
image, video and audio requests are governed by their models’ own
consent gates
and are not changed by this switch.
The extended-context models — `claude-sonnet-4-6`, `fable-5`, and
`opus-4-8` — serve a full 1M-token window through the gateway. Two equivalent
ways to opt in:
- Append the `[1m]` suffix to the model ID (e.g.`claude-sonnet-4-6[1m]` ) — the gateway accepts this form directly.
- Send the `anthropic-beta: context-1m-2025-08-07` header, which the gateway relays to Anthropic.

`[1m]` form through
`ANTHROPIC_MODEL` to get the full window; see
Why the `[1m]` suffix? for the details.
Some catalog models are served by providers operating under PRC jurisdiction:
`deepseek-chat` and `deepseek-reasoner` (DeepSeek’s own API), the Moonshot official-API
models (`kimi-k3`, `kimi-k2.5`, `kimi-k2-thinking`, `moonshot-v1-8k`,
`moonshot-v1-32k`, `moonshot-v1-128k`), the Alibaba Cloud Model Studio-served entries
(`qwen-3.8-max` and the other Model Studio-served Qwen IDs, plus the `-alibaba`
suffixed serving variants), the `-deepseek` / `-moonshot` suffixed serving variants,
the `-streamlake` suffixed serving variants (StreamLake is Kuaishou’s international
AI cloud — data storage is stated as Singapore, but the operator’s parent is
PRC-headquartered and its inference hosting location is unstated),
the Tencent TokenHub models (first-party Hunyuan plus explicit `-tencent` reseller
pins — served from TokenHub’s Singapore site, operator Tencent),
and the ByteDance-served image and video models. The distinguishing test is who
operates the serving endpoint, not the model’s origin — Chinese-origin open weights
served by US providers (for example `deepseek-v4-pro`, `qwen-3.7-max`,
`kimi-k2.6`, or `deepseek-reasoner-bedrock` on AWS) are not China-hosted. Note
that Alibaba’s Qwen chat endpoint runs in the United States (Virginia) for every Qwen
chat model except `qwen-3-coder-next`, which Alibaba serves only from its
international (Singapore) endpoint; all of these models are labeled China-based
because the operator is PRC-headquartered.
Organization admins control access with a per-client opt-in: when disabled, China-hosted
models are excluded everywhere — the chat picker, auto-routing, and API traffic through
the gateway. See the
provider data-handling disclosures
for what each provider receives.
Separately from the China-hosted opt-in, models served by providers whose terms permit
training on API inputs — DeepSeek’s own API, Moonshot’s own API, the NVIDIA-served
`-nvidia` Nemotron variants, and the StreamLake-served `-streamlake` variants
(StreamLake’s international Terms of Service license it to use inputs and outputs
for model training, with no published opt-out) — are locked for every organization
until an enterprise administrator explicitly opts in from the admin console. The
`-streamlake` variants stack both gates: the China-hosted opt-in and the training
consent are each required. The same applies to models
whose provider retention cannot yet be verified or disabled (the `-tensormesh` and
`-gmicloud` serving variants, and the Prodia-served image models `flux-2-dev`,
`flux-2-klein-4b`, `flux-2-klein-9b`, `flux-1.1-pro`
and `sdxl`). Locked models appear greyed-out in the model catalog with the required
consent named, are excluded from chat and automatic routing, and API requests to them
are refused. The
provider data-handling disclosures
carry the full consent-gate matrix.
Off for every organization until an enterprise administrator turns it on from the admin
console’s **Routing**tab, by choosing one of the three pair routers —

**NVIDIA NeMo Switchyard**,

**NVIDIA Switchyard Escalation**, or

**RouteLLM**— as the organization’s model router and picking the efficient model. The Routing tab is an open marketplace of routers under the AI Reserve control plane: one model router and one provider router per organization, each tunable through AI Reserve’s standard quality / cost / speed dial or its own native settings; every router listed is live. The separate

**Models**tab controls model availability and routing eligibility. When enabled, the gateway may serve a turn on the

**efficient model the administrator chose**instead of the model the request named. The three routers differ in how they decide:

- **NVIDIA NeMo Switchyard** (stage routing) serves a routine turn of a coding-agent session
(Claude Code, Codex, and other tool-calling harnesses) on the efficient model and brings
the requested model back when the conversation shows trouble — repeated or serious tool
errors, a stalled session, or a compacted context. Only requests that already carry tool
calls or tool results are candidates: a plain chat message or single-turn API call is never
switched. The decision is a deterministic read of the tool calls and results already in the
request (a TypeScript port of
NVIDIA NeMo Switchyard’s open-source stage
router), steadied on a best-effort basis within a coding session so tiers are not flipped
back and forth on borderline turns (the steadying memory lives on the gateway instance that
served the earlier turn, so it is not guaranteed on every turn); no request content leaves
the gateway to make it, and the per-session memory is a tier label — never conversation
content.
- **NVIDIA Switchyard Escalation** starts every coding-agent session on the efficient model,
from its first turn. On each later turn a trajectory judge — AI Reserve’s internal routing
model, reading a condensed view of the task framing and the most recent turns, never the
full conversation — rules whether the run is in sustained trouble (loops, false progress,
drift, giving up); the number of consecutive trouble verdicts the administrator sets
(upstream’s benchmarked default is two) moves the rest of the session to the requested
model, and a session that has escalated stays there with no further judge calls. It is a
TypeScript port of Switchyard’s open-source escalation router, with one stated deviation:
the judge reads the trajectory as it stands when a request arrives rather than after a
buffered reply from the efficient model, so nothing is served twice. The judge call is
routing overhead AI Reserve pays for; it adds a short delay to each unlatched turn.
- **RouteLLM** scores every chat request — pinned models included — with the open-source
RouteLLM matrix-factorization router from LMSYS, run
in-process with its published checkpoint: an embedding of the latest prompt predicts how
likely the requested model is to answer better than the efficient one, and prompts below the
cost threshold go to the efficient model. The embedding (OpenAI`text-embedding-3-small` ,
the model the checkpoint was trained against) is routing overhead AI Reserve pays for, and
it means each routed prompt reaches OpenAI: an organization that blocks OpenAI as a maker,
leaves it out of its allowed makers, or whose processing-region policy excludes it is not
routed by RouteLLM at all — every request gets the model it asked for, and the Routing tab
says so. The standard interface calibrates the threshold on the organization’s own traffic
to the strong-model share its dial asks for, falling back to RouteLLM’s published Chatbot
Arena calibration until enough requests have been seen; the native knob pins the threshold
itself.

`x-aireserve-served-model` header names the model
that served, and the usage record carries the requested model, the served model, and which
router decided. A switch never crosses your governance: the efficient model must itself be
routable for your organization (not blocked, consent-gated, or restricted) and permitted by
the requesting key’s quota allow- and blocklists; turns carrying images or files anywhere —
including screenshots returned by tools — are never switched; and a turn is left alone when
the efficient model could not take it (a thinking setting it does not support, a context
larger than its window) or would not be at least 2× cheaper on both input and output.
Requests from organizations that have not opted in are unaffected; requests with
`model: "auto"` first receive their frontier model from the organization’s pool, which then
plays the requested model’s part.
An enterprise administrator can route every request that asks for `model: "auto"` to the
enrolled model with the top published benchmark score, by choosing **Benchmark Router**as the organization’s model router on the Routing tab. The ranking is deterministic arithmetic over independent benchmark results published by Artificial Analysis and refreshed daily — no model is called to decide. In standard tuning the pool is ranked on the Artificial Analysis Intelligence Index, discounted by the control plane’s cost weight (a price term across the pool) and speed weight (a latency term); in native tuning the administrator weights up to three benchmarks (Intelligence, Coding, Math and Agentic indices, Humanity’s Last Exam, MMLU-Pro, GPQA Diamond, GDPval, LiveCodeBench, IFBench, τ²-bench, SWE-bench) summing to 100 and nothing else applies. A model with no published score for a weighted benchmark is left out of the ranking; the Routing tab shows, per enrolled model, which scores exist, and when nothing can be ranked AI Reserve picks from the routed pool instead. The block is listed only in environments where benchmark data has been loaded. Requests that name a model are served as asked. An enterprise administrator can route every request that asks for

`model: "auto"` through
Not Diamond’s commercial model router under the organization’s own
Not Diamond account, by choosing **Not Diamond**as the organization’s model router on the Routing tab and pressing

**Test & save**with the organization’s Not Diamond API key. The key is kept only once Not Diamond answers a real test call with a model the gateway serves; it is stored encrypted, never shown again, and Not Diamond usage is billed to the organization’s own Not Diamond account. Each routing call sends Not Diamond the organization’s enrolled models (as Not Diamond names them, or described by price and context length for models it does not list) and the latest user message — hashed by default, so Not Diamond routes on a hash and does not persist the text; never the system prompt, earlier turns, tool results, or user identities. Not Diamond’s answer is checked against the organization’s model policy before it serves, and when Not Diamond does not answer in time AI Reserve picks from the routed pool instead. Requests that name a model are never sent to it. Tuning follows either the control plane’s dial (cost ↔ quality becomes Not Diamond’s

`cost_quality_tradeoff`, a speed-leading dial its
latency tradeoff) or Not Diamond’s own knobs, including a custom router trained on the
organization’s evaluations (`preference_id`). This is a bring-your-own-account integration,
not a Not Diamond service operated by AI Reserve or a Not Diamond endorsement.
An enterprise administrator can make a service you run pick the model for every request that
asks for `model: "auto"`, by choosing **Your endpoint**as the organization’s model router on the Routing tab, entering the endpoint’s URL and HTTP method, and pressing

**Test & save**. The endpoint is saved only once it answers the test with a model the gateway serves; until then (and whenever it does not answer in time) AI Reserve picks from the organization’s routed pool. Requests that name a model are never sent to it.

**Request.**Every routing call sends the organization’s routed models as gateway model IDs, the administrator’s own routing preferences (omitted when empty), and the conversation id when the request carries one:

**Tune with.**The block offers the two tuning modes every router on the Routing tab has:

- **Native** (the default, and how every endpoint saved before this option existed behaves): the
request above, with only the preferences JSON the administrator entered.
- **AI Reserve standard** : the organization’s control plane — the quality / cost / speed dial
and the plain-language routing instructions set under the**AI Reserve Prompt Router** block — is added to`preferences` as`aiReserve` on every routing call, beside the
administrator’s own keys:

`dial` carries the three stored percentages (`smarter` is the quality share, `cheaper` the
cost share, `faster` the speed share; they sum to 100) and is `null` until the organization
has saved a dial; `instructions` is the instructions text or `null` when none are set. Both
keys are always present in standard mode, and `aiReserve` is reserved — a same-named key in the
administrator’s own preferences is replaced. On a `GET` endpoint the same object arrives
URL-encoded in the `preferences` query parameter. The Routing tab shows the exact request
either mode sends, and **Test & save**sends that same request, so an endpoint is never saved against a shape it has not answered.

**Response.**Answer within the deadline (2.5 seconds per routing call; the administrator’s test waits up to 5 seconds) with a model identifier as JSON —

`{"model": "gpt-5.5"}`, with
`modelId` or `id` also accepted — a bare JSON string, or a single-line plain-text body. Loosely
formatted names are tolerated (`Opus-4-8` resolves), but the model must be served on the
gateway and permitted by the organization’s model policy and the requesting key’s quota; any
other answer, or no answer in time, falls back to the organization’s routed pool for that
request. The `x-aireserve-served-model` header on the response names the model that served.
Every image model — generation via `POST /v1/images/generations` and editing via
`POST /v1/images/edits` — is listed with pricing on Image models and in the
browser above under **Image**. The image generation guide covers parameters, response shapes and the async edit job flow. The Qwen Image 3 models and

`seedream-5` (ByteDance’s official Ark API) are China-hosted (see above) and follow the same
per-client opt-in as the chat models. The retired `dall-e-3`, `imagen-3`, `imagen-4`,
`imagen-4-fast`, and `imagen-4-ultra` IDs return errors — migrate to
`gemini-3.1-flash-image` or `gpt-image-1`.
Every video model on the async `POST /v1/videos/generations` job API — text-to-video,
image-to-video and reference-to-video — is listed with per-second pricing on
Video models and in the browser above under **Video**. The video generation guide covers inputs, durations, resolutions and polling.

`wan3.0-video` (Alibaba Model Studio) and the ByteDance official-API
models (`seedance-2.5`, `seedance-2.0-bytedance`, `seedance-2.0-mini`, `seedance-2.0-fast`)
are China-hosted (see above); `wan-2.6`, `seedance-2.0`, and every fal image-to-video /
reference-to-video model are served by fal in the United States.
Every decision model on `POST /v1/decisions` — Cloudflare Clef, TypeSafe Jev, Perplexity
pplx-decider and OpenAI `gpt-6-luna`, each called on its maker’s own API — is listed with its
per-input-token price on Decision models and in the browser above under
**Decisions**. The decisions guide covers the request shape, the

`noul` / `choice` / `score` question types and billing. All four makers are US vendors with no
consent gate. `gpt-6-luna` is also a chat model: it keeps one page, with the decisions route
listed alongside chat.
The typed extraction model on `POST /v1/extractions` — Cloudflare `clairic`, the sibling of
Clef, called on Cloudflare Workers AI — is listed with its per-input-token price on
Extraction models and in the browser above under **Extractions**. The extractions guide covers the request shape, the field types and billing. Cloudflare is a US vendor with no consent gate.

`POST /v1/search` has no `model` field: every search bills under `tako-search` (the default
`fast` effort, or `instant`) or `tako-search-deep` (`effort: "deep"`), the two ids listed with
their per-search price on Live search and in the browser above under
**Search**. The live search guide covers the request shape, using search as a tool and grounded search on chat completions. Tako retains queries, so both ids carry the unverified-retention opt-in.
