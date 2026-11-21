---
id: collect-261121-veille-ia/gemini-google/google-s-first-gemini-4-model-argon-avoids-hallucinations-and-shines-at-knowledge-work
title: "Google's First Gemini 4 Model, Argon, Avoids Hallucinations and Shines at Knowledge Work"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["gemini", "google", "model", "reasoning", "argon", "models", "cybersecurity", "million"]
source: https://www.deeplearning.ai/the-batch/argon-a-low-hallucination-model-build-for-knowledge-work
source_anchor: ""
source_lines: [1, 37]
section: "Gemini_Google"
---
Google took over half a year before it replaced its flagship AI model, Gemini 3.1 Pro Preview. Now as then, the company’s latest release is among the leaders, but only cybersecurity defenders can use it for now.

**What’s new:** Google announced Gemini 4 Argon, a vision-language model trained to solve long, complex problems in software engineering, law, finance, and cybersecurity.

- **Input/output:** Text, images, and video in (up to 1 million tokens), text out (up to 1 million tokens)
- **Features:** Adjustable reasoning
- **Performance:** Leads Vals AI’s Vals Index v2.1 (68.9 percent); ties for third on Artificial Analysis’ Intelligence Index v4.3.2 (53)
- **Availability:** Currently only available to organizations in Google’s Fairwind cybersecurity program
- **Price** : Via API at an introductory price of $2/$0.10/$10 per million input/cached/output tokens, then $4/$20 per million input/output tokens
- **Weights/license:** Proprietary
- **Undisclosed:** Parameter count, architecture, latency and throughput, training data and methods, knowledge cutoff, end date of introductory pricing, date for broader availability

**How it works:** Google described how the model generates very long outputs and how it built safeguards around the model’s scientific and cybersecurity outputs.

- Gemini 4 Argon’s generations can extend up to 1 million tokens (up from 64,000 for Gemini 3.1 Pro Preview). This relies on a Gemini API feature called Long Decode Continuation, which pauses long responses and resumes them in later calls so requests don’t time out.
- Selected defenders in the company’s Fairwind Program and Google’s internal teams receive a version of the model without cybersecurity guardrails. Google says the version for wider release refuses harmful requests related to cyber, chemical, biological, radiological, or nuclear attacks.
- Google says it isolates and seals the environments it uses for high-risk training and evaluation. Last month, Google and security firm Irregular confirmed that during autonomous testing, an unnamed Gemini model was able to hack into three other companies’ systems due to poor sandboxing.
- Google uses inexpensive classifiers called probes to monitor the model’s internal activations, rather than its text output, for signs of disallowed uses, a technique it used in earlier Gemini models.
- To defend against indirect prompt injections (instructions hidden in documents or web pages the model reads), Google synthetically generated many such attacks and trained the model to resist them.
- Monitors read the model’s reasoning and actions and halt operations when they stray beyond Google-defined parameters. Similar systems monitored training. Google kept these systems’ findings out of the training set so the model wouldn’t learn to hide its reasoning from them.

**Performance:** Independent benchmarks rank Gemini 4 Argon well above Google’s other models and near the top of the field. It leads most tests of finance, legal, and business performance and seldom guesses wrong when it doesn’t know an answer. It still trails Anthropic’s newest models on agentic coding and Artificial Analysis’ broad index of overall intelligence.

- On Vals AI’s Vals Index, eight finance, coding, legal, and tax benchmarks weighted by each sector’s share of U.S. gross domestic product, Gemini 4 Argon set to high reasoning ranks first of 43 models (68.9 percent accuracy, $15.68 and 46.55 minutes per test at standard prices).
- On Artificial Analysis’ Intelligence Index, a composite of 10 evaluations across math, science, coding, and reasoning, Gemini 4 Argon set to high reasoning (53, $1.99 per task at discounted price, $3.98 per task at standard price) achieves the same overall score as GPT-6 Astra set to max reasoning and Claude Fable 5.1 set to max reasoning with fallback at lower cost per task.
- On AA-Omniscience, a test of factual knowledge that penalizes wrong answers but not refusals, Gemini 4 Argon set to high reasoning has the lowest hallucination rate (15 percent) of any model scoring 45 or higher on the Intelligence Index, significantly lower than other flagship models, such as GPT-6 Astra set to max reasoning (51 percent).
- On Arena.ai’s Text Arena leaderboard, which ranks models via blind head-to-head comparisons, Gemini 4 Argon set to high reasoning ranks first (1,525 Elo), ahead of Claude Opus 4.6 set to high reasoning (1,505 Elo) and Claude Opus 5.5 set to high reasoning (1,504 Elo).

**Behind the news:** Anthropic and OpenAI frequently release new models, particularly those they believe could pose a cybersecurity risk, only through gated programs (and for internal use). With Gemini 4 Argon, Google follows their example. In each case, organizations selected by the AI company get a less-restricted version first, and everyone else gets a version with more safeguards later.

- Anthropic initially limited all its Mythos models to members of Project Glasswing, its organization of U.S. cybersecurity and life sciences organizations that do defensive work. Only selected companies can use Claude Mythos 5.1, while anyone can use Claude Fable 5.1, which includes safeguards and retains input data for one month.
- OpenAI’s Daybreak program sorts selected organizations that do defensive cybersecurity work into tiers. Besides OpenAI itself, only one tier, Daybreak Red, can use GPT-6 Astra with reduced safeguards. Other customers get a model with limits.
- Google used this approach with its Fairwind Program and Gemini 3.8 Flash Cyber, a variant of Gemini 3.8 Flash with looser cybersecurity safeguards that only selected cybersecurity organizations can use.

**Why it matters:** One of Gemini 4 Argon’s strengths is long-context and high-output code migrations. Google said its agents translated C and C++ code into Rust across the company, including more than 800,000 lines from the kernel of Fuchsia, an open-source operating system that Google developed. Rust is designed to prevent many of the memory bugs that C and C++ allow. A patch fixes one vulnerability, but a rewrite in Rust addresses an entire vulnerability category. If models like Gemini 4 Argon can enable such rewrites quickly and cheaply, defenders may be able to remove specific types of vulnerabilities before attackers find them, rather than patching them piecemeal.

**We’re thinking:** Many models are competing to be the best software engineering assistant in the world. Gemini 4 Argon may not be the most powerful coder, but Vals AI’s index shows its strengths in fields like finance, taxes, and law. Gemini 4 Argon is also the frontier model most likely to admit it doesn’t know the answer to a factual question rather than guess wrong. In a financial analysis or legal brief, a confident incorrect answer may slide past a reviewer, while a refusal is obvious and can be passed to a person or another model — assuming applications using it have a plan for questions the model won’t answer.
