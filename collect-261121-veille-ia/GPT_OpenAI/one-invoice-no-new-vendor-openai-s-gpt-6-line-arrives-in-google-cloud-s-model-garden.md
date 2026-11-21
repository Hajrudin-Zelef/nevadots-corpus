---
id: collect-261121-veille-ia/gpt-openai/one-invoice-no-new-vendor-openai-s-gpt-6-line-arrives-in-google-cloud-s-model-garden
title: "One Invoice, No New Vendor: OpenAI's GPT-6 Line Arrives in Google Cloud's Model Garden"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["cloud", "google", "openai", "models", "model", "line", "gpt-6", "coming"]
source: https://www.gurujicorp.org/blog/openai-google-cloud-model-garden-2026.html
source_anchor: ""
source_lines: [1, 43]
section: "GPT_OpenAI"
---
Google Cloud added GPT-Astra, GPT-6.1 Sol and GPT-6 Luna to a model garden that already lists more than 200 models, sold through Google Cloud Marketplace so existing commitments and Flexible Savings Plans can pay for them — in private preview, with general availability still 'in the coming weeks'.

The most consequential line in OpenAI's 21 October announcement is not the model list. It is the billing line.

Google Cloud will sell OpenAI's frontier models through Google Cloud Marketplace, on a single Google Cloud invoice, and enterprises with existing Google Cloud spend commitments can draw those commitments down to pay for them, according to Google Cloud's own post. For a buyer in India who has already signed a multi-year cloud commitment, that is the difference between "we should evaluate GPT-6" and "we can turn it on this quarter without a new purchase order".

On 21 October, OpenAI and Google Cloud announced that OpenAI's frontier models and Codex agentic workflows are coming into Google Cloud's model garden. Google's post, written by Michael Gerstenhaber, VP of Product Management for Gemini, names three models rolling out now in **private preview to select enterprise customers**:

Two claims in Google's post are worth checking rather than repeating. The first is that Google Cloud is now "the only hyperscale cloud provider offering access to all three leading closed source LLMs, alongside more than 200 first-party, third-party, and open models on a unified enterprise AI platform." The 200-plus figure is a catalogue count, not a capability; the "all three" claim is a positioning statement Google has every reason to make. The second is timing: OpenAI says general availability "is planned in the coming weeks", and neither post lists a price.

What is live today is narrower and more useful. Google says customers can build with these models through its APIs, and can connect their local **Codex CLI and Desktop workflows directly to Google's Gemini platform for inference**. Both companies say OpenAI models will reach Gemini for business and Google Antigravity later, and OpenAI adds one sentence that reads like a contract clause rather than marketing: "Microsoft remains OpenAI's primary cloud partner."

Three details decide whether this changes a procurement conversation or just a press release.

**Commitment drawdown.** Spend on OpenAI models transacted through Google Cloud Marketplace counts against existing Google Cloud commitments and toward Flexible Savings Plans across model providers. No new vendor master, no fresh security review for a separate API contract, no separate dollar float. It is the same construction OpenAI used with Oracle in June, when it opened its models and Codex to Oracle cloud commitments — cloud spend, not a new AI line item.

**Isolation, stated in engineering language.** OpenAI models run on dedicated Google Cloud infrastructure, with Confidential Compute coming soon, and all queries route through hardened, isolated gateways so that, in Google's words, "third-party model providers have no unauthorized access to your cloud environment."

**Audit surface.** Every agent and model interaction runs under an authenticated identity, with VPC Service Controls protection and unified audit logging across the footprint. Prompts, context and outputs stay inside the designated data perimeter and are not used to train foundation models, with Zero Data Retention "coming soon."

Note the pattern in those last two paragraphs: dedicated infrastructure, Confidential Compute, ZDR — each announced as *coming*, not shipped. The governance story is real; parts of it are still on the roadmap.

**Your cloud commitment just became a model budget.** Most Indian mid-market firms on Google Cloud already carry an annual commitment they under-use. Until now, an OpenAI API bill was a second, separately negotiated line item in dollars, often on a corporate card, outside the IT budget and outside the cloud discount. Marketplace transacting moves it inside the commitment, onto one invoice, and into the approval chain the finance team already understands. That is unglamorous and it is the whole point.

**Model routing stops being an architecture rewrite.** The pitch from both vendors is swapping models by workload — Gemini for high-volume tasks, GPT-Astra where answer quality actually matters — without a separate contract per vendor. For a team running classification, extraction and summarisation alongside a handful of hard reasoning calls, that is a cost-control lever: route the 90% cheap, pay frontier prices for the 10% that needs it. The caveat is that routing is a measurement discipline, not a switch. Without a harness that compares cost per completed task on your own documents, "model choice" quietly becomes "whatever the last demo used".

**Governance evidence for data-protection reviews.** Indian teams handling personal data need to show where prompts and outputs live, who can access them and whether they were used for training. Data perimeters, authenticated identity per agent interaction and unified audit logs are exactly the artefacts an internal review or a client's vendor questionnaire asks for. Zero Data Retention being "coming soon" is the line to track if your sign-off depends on it.

**Codex on Gemini inference is the quiet item.** Connecting local Codex CLI and Desktop workflows to Google's inference platform means a developer can keep the coding agent they already use while the tokens bill against the cloud commitment. For a 20-person engineering team, that is usually the first place agentic AI shows a measurable return — and the first place an unmanaged bill appears.

**Preview is not production.** This is private preview for select enterprise customers, with general availability "in the coming weeks", no published pricing, and no announced India region or data-residency commitment in either post. Anyone writing "we run GPT-6 on Google Cloud" into a proposal this week is writing about a form submission.

A day later, on 22 October, OpenAI took the **Decisions API** out of beta. It is built on GPT-6 Luna and answers bounded questions — is this document damaged, which queue should this ticket go to, how severe is this — returning a probability, one of your own choices, or a rubric score, at up to 10x the speed of calling GPT-6 Luna through the Responses API.

The pricing is the interesting part: **$0.10 per million input tokens, $0.01 cached, and no output-token charges at all**, with up to 400 questions per request, twice the old limit, plus inference residency in the US and EU and added capacity in several Asian regions. OpenAI's launch quotes put Datadog at 40x faster p95 for scoring shell commands before execution, Ramp at 4x faster and 30% cheaper for AI cost attribution, and Sierra at under 280 ms p95 with 90% lower cost per call. Those are vendor-published customer figures, not independent benchmarks — but the shape of the offer is clear: not a smarter model, a cheaper decision, priced so that putting it in the hot path stops being absurd.

For Indian teams, the honest use case is the boring one. Every workflow has a dozen small branching decisions that currently wait for a human or a rules engine. A per-million-token price with no output charge makes the arithmetic visible for the first time.

We do not resell cloud commitments and we are not going to pretend a preview programme is a strategy. What we do is the layer that decides whether any of this saves money: a routing and measurement harness that runs your real documents and tickets through each candidate model and reports **cost per completed task**, not tokens per second; procurement-ready comparisons that put the commitment drawdown next to the API bill so the saving is a number rather than a claim; and the audit scaffolding — logged identities, retained prompts, reproducible outputs — that makes an AI-assisted decision defensible a year later.

If you are weighing a Google Cloud commitment against a pile of separate AI subscriptions, the useful first deliverable is your own number. Talk to us and we will measure it on your workload.

**Sources**
