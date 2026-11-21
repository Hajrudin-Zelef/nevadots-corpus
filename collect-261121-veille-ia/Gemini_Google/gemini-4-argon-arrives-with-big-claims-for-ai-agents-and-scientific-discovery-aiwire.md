---
id: collect-261121-veille-ia/gemini-google/gemini-4-argon-arrives-with-big-claims-for-ai-agents-and-scientific-discovery-aiwire
title: "Gemini 4 Argon Arrives With Big Claims for AI Agents and Scientific Discovery - AIwire"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-10-21"]
keywords: ["argon", "google", "also", "gemini", "through", "model", "agents", "tasks"]
source: https://www.hpcwire.com/aiwire/2026/10/21/gemini-4-argon-arrives-with-big-claims-for-ai-agents-and-scientific-discovery/
source_anchor: ""
source_lines: [1, 39]
section: "Gemini_Google"
---
Most of the attention around AI infrastructure has gone to GPUs, and there are good...

## Off the Wire Press Releases

Google just released the new Gemini 4 Argon, which it claims can generate up to a million tokens in a single run, a big jump for long code migrations and multi-step agent work. Competitors top out around 128,000 output tokens per request, so based on that claim, Argon’s 1 million is roughly 8x higher.

Argon is now available globally, with developers accessing it through the paid Gemini API and businesses through Google Cloud. Individual users need a Google AI Pro or Ultra subscription, while access to the version without cybersecurity restrictions requires separate approval. Argon costs $2 per million input tokens and $10 per million output tokens through November 30. Both rates double in December.

Google says it is also its most resilient model yet. It is built to withstand indirect prompt injection. That is the kind of attack that could hijack an AI agent working inside a company. Independent testing of that claim has not been shared by Google.

Argon is built on a sparse mixture-of-experts design, which routes each input to a small set of specialized sub-networks. This is the opposite approach to running the whole model at once. It’s worth keeping in mind that this is a new model, not a fine-tune of an earlier one.

Unlike rivals such as GPT-6 Astra and Claude Opus 5.5, which list text and image input, Argon can also take audio and video, though it only produces text. Paired with code execution, search grounding and function calling, that makes it a candidate for AI agents that work through multi-step tasks.

Google also shared some interesting benchmark results. Argon scored 68.9% on Vals Index. This was a lead against rivals GPT-6 Astra (63.1%) and Claude Opus 5.5 (67%). The Vals Index is designed to test models on the kind of work people do in finance, law, coding and tax services.

Argon also scored 51.3% on AutomationBench, beating both rivals again. The AutomationBench tests whether AI agents can actually complete tasks across different business applications. However, 51.3% by itself isn’t exactly a high success rate. What this means is that nearly half the tasks still went unfinished or were done incorrectly. Compared to rivals this might be good, but it raises concerns about the overall ability of the tested models. All these figures are based on Google’s published comparisons.

Google has also been using Argon internally. The Chrome team put it to work on hundreds of files in Chromium, the codebase behind Chrome. Google says the changes were done in weeks, rather than the months they would normally have taken. Its Agent Development Kit team found improvements too, with session preparation time dropping 40% and cold imports getting 21% faster.

Gopal Srinivasan, Global AI and Data Lead, Deloitte, has also been impressed with the model. He said, “Early impressions from Gemini 4 are very strong: for long-running tasks across these workflows, it completed successfully 100% of the time in our evals, maintaining context and running through multiple tools across impressive spans.

“Its depth of reasoning delivers significantly higher, clearer output. Performing in-depth reasoning for domain-specific tasks without detailed prior logic or instructions is something we’ve never seen with any model in its class before.”

With Gemini 4 Argon, Google has also made a big push toward unlocking new scientific breakthroughs. The company shared that “Argon is already having real-world impact across the scientific community.”

In quantum computing, Argon helped nearly double the inference throughput of Google’s AlphaQubit error-correction system. Another implementation it produced cut the spacetime cost of a quantum operation by more than 100 times compared with an earlier method.

Google researchers also worked with University of Maryland professor Matt Landreman on nuclear fusion. They found new mathematical solutions that could help researchers test simulations of stellarator reactors.

At Gamow Labs, Argon spotted a chromosomal rearrangement in a newborn’s genetic data that conventional software had missed for years. In another experiment, Google used Argon to design five new enzymes in hours. All five showed catalytic activity when tested in the lab.

“Gemini 4 represents a step change in what AI can accomplish in real-world science,” said Alex Cabrera, Founding Engineer, Axiom Bio.

“We’ve seen the reason through complex experimental data, from cellular imaging to mass spectrometry, and make subtle findings that traditionally require expert scientists to identify. On our Scientific Agent Benchmark, Gemini 4 was at the frontier across most tasks, including our hardest: ranking the clinical risk between two drugs.”

Google is offering 10,000 eligible scientists six months of free access to its highest Antigravity usage tier.

**Learn more:** Google is hosting a session on Gemini 4 Argon on Tuesday, October 27, where attendees can learn more about the model and ask questions. Find out more.
