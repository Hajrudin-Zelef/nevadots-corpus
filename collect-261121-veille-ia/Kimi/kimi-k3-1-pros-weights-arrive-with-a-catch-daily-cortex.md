---
id: collect-261121-veille-ia/kimi/kimi-k3-1-pros-weights-arrive-with-a-catch-daily-cortex
title: "Kimi K3.1 Pro’s Weights Arrive, With a Catch — Daily Cortex"
domain: kimi
role: reference
task: article
actors: ["Moonshot AI"]
dates: []
keywords: ["kimi", "model", "weights", "moonshot", "license", "models", "revenue", "hugging"]
source: https://www.dailycortex.com/kimi-k3-1-pro-weights-arrive
source_anchor: ""
source_lines: [1, 26]
section: "Kimi"
---
BEIJING, Nov. 4 — Moonshot AI, the Chinese start-up behind the Kimi models, has put the full weights of Kimi K3.1 Pro on Hugging Face for anyone to download: 2.8 trillion parameters, 104 billion of them at work on any one token, in 96 files totaling 1.56 terabytes.Mixture of ExpertsThe model holds 896 “expert” networks and routes each token through 16 of them, plus 2 shared ones, so only a small share of its parameters is active at a time. Its context window is 1,048,576 tokens.

The license is generous, with one large exception. If a company or any of its affiliates runs a “Model as a Service” business, and their combined revenue tops $20 million over any 12 consecutive months, the company must sign a separate agreement with Moonshot before using the model for any commercial purpose. The license defines such a business as giving others access to a model’s inference or fine-tuning, as through an API, with meaningful control over its inputs; it excludes features embedded in end-user products and the “mere relaying of requests to models hosted by others.”

Moonshot had said it would release the weights by Nov. 4. It announced the release on X at 11 a.m. Eastern on Wednesday, the first minute of Nov. 5 in Beijing; when the repository itself went public, the desk could not establish. Its only two commits are an initial one, dated 12:13 p.m. on Nov. 2, Beijing time, and a change to one script, encoding_k3.py, at 2:11 a.m. Eastern on Thursday. At 7:45 a.m. Eastern on Thursday, Hugging Face counted 11,658 downloads.

The release puts within reach of anyone with the hardware what Artificial Analysis, an independent testing firm, said on Oct. 23 would be the highest-scoring open-weights model on its Intelligence Index once the weights were out. The hardware is considerable. vLLM, an open-source serving engine, says its simplest working setup takes eight Nvidia GPUs, in one B300 server or two GB300 trays, or one server of AMD’s MI355X chips. The routed experts’ weights ship in MXFP4, a compact number format the model was trained to use, and the rest in BF16.Same Size as K3K3.1 Pro is post-trained from Kimi K3, whose weights Moonshot posted on Hugging Face in July, and keeps its layout but for sparse attention in 24 of its layers. Hugging Face counts about 2.78 trillion parameters in each.

### The Terms

The grant itself borrows the wording of the permissive MIT license: free of charge, to use, copy, modify, sell, fine-tune and build on. The revenue test counts all of a host’s revenue and its affiliates’, not only what it earns from Kimi, so a large cloud that sells the model by the token needs a deal however little it makes from it.

A second condition requires any product with more than 100 million monthly active users, or more than $20 million in monthly revenue, to display “Kimi K3.1-Pro” prominently. Internal use is exempt from both conditions, as is any use through Moonshot’s own products or its “certified inference partners,” whom the license does not name.Not NewThe terms match, word for word but for the model’s name, the license now posted with Kimi K3.

Below the revenue line, a host needs no deal at all. Above it, the license does not say what a deal costs.

K3.1 Pro’s offensive cyber capability is, by design, below what our pipeline can produce, and remains behind that of frontier proprietary models.


On Moonshot’s own tests, the model splits the row wins with Anthropic’s Claude Opus 5.5: by the desk’s count of the model card’s table, it beats Opus 5.5 on 21 of the 42 tests where the table scores both, and trails on the other 21. The card warns that some of its scores, such as Elo ratings, are not directly comparable with others. Moonshot ran many of its scores with Anthropic’s Claude Code as the harness, the software that drives a model through a task, and took many rivals’ scores from their makers’ reports or public leaderboards. Several tests are Moonshot’s own.

It trails on attack work, and Moonshot says that is deliberate: it removed cybersecurity material from training for the open release. On ExploitBench, scored as the best of five tries, it reports 74.7 for K3.1 Pro, against 91.0 for Opus 5.5 and 100 for OpenAI’s GPT-6 Astra, a result OpenAI says it measured without production safeguards; on xdev-bench-hard, its internal test of finding and exploiting flaws in source code, scored as the best of three tries, 81.0. “K3.1 Pro’s offensive cyber capability is, by design, below what our pipeline can produce, and remains behind that of frontier proprietary models,” Moonshot wrote in its launch post.

The next test is whether outside testers, running the downloaded weights on their own machines, match Moonshot’s figures.

*Daily Cortex is written with AI models, including Anthropic’s Claude, and fact-checked with OpenAI’s GPT models.*
