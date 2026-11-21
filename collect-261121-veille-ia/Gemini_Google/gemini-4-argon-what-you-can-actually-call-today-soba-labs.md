---
id: collect-261121-veille-ia/unifi-ubiquiti/gemini-4-argon-what-you-can-actually-call-today-soba-labs
title: "Gemini 4 Argon: what you can actually call today | Soba Labs"
domain: unifi-ubiquiti
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["input", "tokens", "output", "model", "gemini", "argon", "introductory", "price"]
source: https://sobalabs.ai/blog/gemini-4-argon-what-you-can-call-today/
source_anchor: ""
source_lines: [1, 63]
section: "Gemini_Google"
---
Google’s September 30 announcement put Gemini 4 Argon behind a cybersecurity allowlist and left builders with a benchmark table, an introductory price and no way to call it. On October 13 that changed. **You can call `gemini-4-argon` on the paid Gemini API and on Vertex AI today, at $2 per million input tokens until November 30, with no published rate limits and no knowledge cutoff in the API docs.** This post walks what a production builder actually touches: the thirteen-day rollout, what the endpoint serves, the pricing and its expiry date, the numbers Google has not published or has filed where nobody looks, how Vertex differs, and why your model router probably cannot reach it yet.

## Thirteen days separated the benchmark table from a callable endpoint

The September 30 announcement, credited to Koray Kavukcuoglu, described a frontier model for coding, enterprise knowledge work and cyber defense, *“rolling out to a set of trusted cyber defenders through our Fairwind Program”*, with an introductory price of **$2 per million input tokens and $10 per million output** and a footnote promising $4 and $20 after the introductory period. It also raised the output limit to **1 million tokens**, up from 64K. What it did not contain: a model ID, an input context figure, or a date for general access.

For the next thirteen days the gap was the story. The Batch covered Argon on October 9 with availability listed as Fairwind organizations only and a long “undisclosed” list: parameter count, knowledge cutoff, the end date of the introductory pricing, the date for broader availability. Then a second Google post on October 13 opened the model to developers on the paid tier of the Gemini API, and the docs filled in most of the blanks the same day. It is the same ship-today test we ran on world models last week: the announcement is marketing until there is an endpoint with a price beside it.

## The endpoint serves one unified window, and long output is a mode, not a default

The model page lists `gemini-4-argon` as Stable with a unified context window of **1,048,576 tokens shared across input, thinking and output**. That unification is the spec worth reading twice. A synchronous request returns at most **65,536 output tokens**; the 1M output the marketing leads with is a separate mode called long decode, which runs the interaction in the background and streams the response in hops of up to 64K tokens that your code joins back together, documented on the What’s new page. Every example there goes through the Interactions API, with `generateContent` kept for cases that need client-side control over retries and storage:

```
from google import genai
client = genai.Client()
interaction = client.interactions.create(
    model="gemini-4-argon",
    input="...",
    background=True,
    generation_config={
        "max_output_tokens": 262144,
        "thinking_level": "high",
    },
)
```
The rest of the spec sheet: input is text, image, video, audio and PDF with text out, which makes it a multimodal LLM on the input side only. Thinking comes in three levels (low, medium, high), the standard shape for reasoning models now. We set those levels side by side with OpenAI’s and Claude’s in reasoning effort across the three APIs. Caching is **implicit only**, with explicit caching unsupported. Computer use is listed as supported in preview, and the Live API is not supported at all. If your product depends on explicit cache control or realtime voice, Argon is not your model this quarter.

## The price is real, and it doubles on December 1

The pricing page turned the announcement’s open-ended “introductory price” into dated rows: input is **$2.00 through November 30, 2026 and $4.00 starting December 1**, output including thinking tokens is **$10.00 through November 30 and $20.00 after**. Cached input runs $0.10 now and $0.20 later. Batch halves the standard rate ($1 in, $5 out through November), and priority inference costs 1.8x standard ($3.60 in, $18 out). For scale, Gemini 3.8 Flash sits at $0.75 in and $3.75 out on the same page, so Argon costs about 2.7x Google’s workhorse model today. After December 1 that becomes 5.3x, and it returns to about 2.7x on January 1, when Flash takes its own step to $1.50 and $7.50.

Access is paid only, and the same pricing page says it plainly: *“Gemini 4 Argon is not available on the Free tier or on the Google AI Pro plan.”* There is no free-tier evaluation path; the cheapest way to try it against your own workload is a paid key and a small batch.

**The trap:** any unit economics you model this month are built on a price that doubles on December 1. Put the date beside the number in your model-selection doc, or the doc misleads whoever reads it in Q1.


## The rate limits are missing, the cutoff is misplaced, and the front door shows neither

The rate-limits page names no requests-per-minute or tokens-per-minute figure for Argon, or for any model: *“Rate limits depend on a variety of factors (such as your usage tier) and can be viewed in Google AI Studio”*, not on any public page. The numeric limits the page does publish for Argon govern batch work (enqueued token caps of **5M, 500M and 1B** across tiers 1 to 3) and spend velocity ($10, $50 and $200 per 10 minutes by tier). You cannot size a synchronous production workload from the public docs alone.

The knowledge cutoff is the second gap. It appears nowhere on the API model page or the models index. The only first-party statement sits in the model card on DeepMind’s site: *“The knowledge cutoff for Gemini 4 is March 2026, but in some domains the model’s knowledge may be less up to date.”* The same card is where you learn the model is a sparse mixture-of-experts transformer and not a fine-tune of a prior model.

The third gap is the front door itself. DeepMind’s Gemini page, the page a search for the model lands on, contains no price, no rate limit and no input-context figure (only the 1M output window). It also says Argon is *“available today for Google AI Pro and Ultra subscribers”*, while the API pricing page excludes the AI Pro plan outright; whichever tier Google means, a builder reading both pages gets two different answers. GA now means the model ID ships before the operating envelope does: the price carries a date, the rate limits live in a console, and the cutoff hides in the model card.

## Vertex serves the same model with different defaults, and they bite

On Google Vertex AI the Argon spec page shows the same model ID at GA with the same October 13 release date, and then diverges in the details that break ports. The API maturity is inverted: on the Gemini API the Interactions API is the recommended path, while on Vertex it is marked preview and `GenerateContent` is the supported path. Thinking defaults to **medium** on Vertex where the Gemini API examples pass the level explicitly, and setting `thinking_level="MINIMAL"` or a `thinking_budget` returns an HTTP 400. Custom values for `temperature`, `top_p` and `top_k` are ignored outright, so a config block copied from a Gemini 3 deployment silently stops meaning anything. Implicit caching carries a 6,144-token minimum. Availability spans a `global` endpoint plus `us` and `eu` multi-regions, which matters if your data residency story is specific. And the cybersecurity variant the September announcement teased is a real second SKU there, `gemini-4-argon-cyber-permissive`, behind a Fairwind Program allowlist. Model swaps inside one vendor carry the same trap, which is why we ran this exercise on Claude Haiku 5.5, where six request shapes now error after the swap from the old Haiku.

Also worth noting for your bookmarks: `cloud.google.com/vertex-ai` model docs now redirect to a “Gemini Enterprise Agent Platform” section on `docs.cloud.google.com`, so the spec above lives at an address your saved links do not predict.

## Your router does not have it, so verify availability by invoking

If your stack reaches models through model routing or a third-party inference provider, day one looks different. OrcaRouter’s October 8 write-up documented the pre-GA gap precisely (no model ID in the API list, a 404 on the model-card path), and five days later most of its claims were obsolete, which is its own lesson about dated availability posts. But the routing half still holds: on October 14, one day after GA, the router’s live model list carries 207 models and no `argon` or `gemini-4` route, and requesting `google/gemini-4-argon` returns `model_not_found`. Day-one adoption means calling Google directly.

We hold this rule from our own delivery work. On a client build running on Amazon Bedrock, “the model is available” decomposed into four separate facts: the runtime IAM permission, the region the model actually lives in, whether the account had it enabled, and the detail that newer models there are invocable only through a regional inference profile ID rather than the bare model ID. Console visibility proved none of them. The test we now treat as definitive is a one-cent invoke returning tokens on the exact route we plan to ship, and that test applies unchanged to Argon: a GA banner, a router’s catalog page and a marketing site can each disagree with the endpoint.

## Read the benchmark table as what it is: the vendor’s own

DeepMind’s page carries a 19-row comparison against GPT-6 Astra and two Claude models, and the methodology note that travels with it says the quiet part: Gemini scores are pass@1 at the highest thinking settings, and the *“results for non-Gemini models are sourced from providers’ self reported numbers”* unless noted, as of September 2026. Read with that lens, the table is more honest than the headline. Argon posts **77.9% on DeepSWE v1.1**, the state-of-the-art claim from the announcement, and in the same table posts **55.0% on FrontierSWE v2** against GPT-6 Astra’s 65.5%. Terminal-bench 4.0 has it at 57.4% behind Claude Opus 5.5 at 66.4%. The strongest early third-party signal points elsewhere entirely: The Batch highlights the **15% hallucination rate on AA-Omniscience**, the lowest of any model scoring 45 or higher on its Intelligence Index, and a first-place 68.9% on the Vals Index, which is a knowledge-work profile rather than a coding one. Whether any of that moves the models builders actually run is a question the usage data answers in a quarter, not the launch table.

## The takeaway

Gemini 4 Argon is callable today, and the gap between callable and operable is the work. The model ID, the unified 1M window, the dated pricing and the Vertex quirks are all published; the rate limits, the cutoff and the third-party routes are not, and the marketing page will not tell you any of it. Before routing production traffic at it, run the invoke test on your exact route, price against the December 1 step rather than the introductory number, and re-check the table’s rows that your workload resembles rather than the rows the announcement quotes. The procurement half of this question, what it takes to trust a model vendor’s claims at all, is the same one we worked through for Chinese open-weight models.
