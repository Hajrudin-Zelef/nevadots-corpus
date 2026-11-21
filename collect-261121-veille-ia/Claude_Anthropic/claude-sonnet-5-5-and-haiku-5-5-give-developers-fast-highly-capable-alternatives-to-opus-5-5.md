---
id: collect-261121-veille-ia/claude-anthropic/claude-sonnet-5-5-and-haiku-5-5-give-developers-fast-highly-capable-alternatives-to-opus-5-5
title: "Claude Sonnet 5.5 and Haiku 5.5 Give Developers Fast, Highly Capable Alternatives to Opus 5.5"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["claude", "sonnet", "haiku", "tokens", "models", "model", "reasoning", "index"]
source: https://www.deeplearning.ai/the-batch/claude-fills-out-its-model-family
source_anchor: ""
source_lines: [1, 31]
section: "Claude_Anthropic"
---
Claude Haiku, Anthropic’s fastest and least expensive model gets a long-awaited update. Meanwhile, the new version of Claude Sonnet nearly matches Claude Opus 5.5 on an index of independent benchmarks, but only at a reasoning level that uses more tokens per task than any other model tested.

**What’s new:** __Claude Haiku 5.5__ replaces the year-old Claude Haiku 4.5. Independent tests show marked improvement, even with a steep price. Two weeks earlier, Anthropic launched __Claude Sonnet 5.5__, which the company says is best suited to routine coding and office work, including fixing bugs and producing documents, slides, and spreadsheets. It replaces Claude Sonnet 5 at the same per-token prices.

- **Input/output:** Text and images in (up to 1 million tokens), text out (up to 128,000 tokens; 237 tokens per second for Claude Haiku 5.5, 127.5 tokens per second for Claude Sonnet 5.5)
- **Features:** Adaptive thinking with five reasoning levels (low, medium, high, xhigh, max; defaults to high via API and medium via Claude Code and Anthropic’s apps), tool use, computer use
- **Knowledge cutoff:** June 2026
- **Performance:** At launch, Claude Haiku 5.5 scored a 43 on Artificial Analysis’ Intelligence Index, placing it 13th among available models. Likewise, Claude Sonnet 5.5 was first on Artificial Analysis’ Coding Agent Index (68) and second on Artificial Analysis’ Intelligence Index (56) and Vals AI’s Vals Index (67.04 percent) at launch
- **Availability/price:** Free and paid plans on Claude.ai (web, iOS, Android) and Claude Code; Claude Haiku 5.5 $.0.10/$0.01/$0.50 via API per million input/cached/output tokens (all prices are 5x if they exceed 100k tokens); Claude Sonnet 5.5 via API $2/0.10/$10 per million input/cached/output tokens; 50 percent off for batched requests
- **Weights/license:** Proprietary
- **Undisclosed:** Parameter count, architecture, training data and methods

**How it works:** Anthropic disclosed few details about how it built Claude Haiku 5.5 or Sonnet 5.5 beyond their reasoning controls and safeguards.

- Anthropic trained the models on a mix of public web data, public and private datasets, synthetic data, and user data from feedback, bug reports, and users who permitted training. After training, the company fine-tuned the model to align with values it defined using a constitution.
- Adaptive thinking, in which the models decide whether and how long to reason before it responds, is on by default. Developers using the API can set their reasoning level with a parameter called effort, which governs all output tokens, including reasoning, response text, and tool calls. At lower levels, the models reason less or not at all on simple problems and make fewer tool calls.
- Classifiers screen requests for risks involving cybersecurity, biology, weapons, and the development of frontier AI models. For cybersecurity, three components decide whether to block a conversation: a probe of the model’s internal activations, a lightweight classifier that runs on Claude Sonnet 5.5 itself, and a separate language model trained to classify requests. Anthropic warns that users should expect more refusals than with Claude Sonnet 5. Blocks related to biology and weapons have no fallback.
- Anthropic says Claude Sonnet 5.5 is the first Sonnet model to launch with classifiers that block attempts to extract its reasoning without falling back to another model, an attempt to make distillation (training a model to imitate another model’s outputs) harder. The reasoning blocks it returns work only in the account that generated them, and the API can reject them if earlier parts of the conversation change.

**Performance:** Claude Haiku 5.5 at max effort outperforms DeepSeek V4.1 Flash and all other “fast” models, but behind the top models. The latest Haiku version is also the fastest model Artificial Analysis has tested by tokens per second. Claude Sonnet 5.5 set to max effort ranks first or second on three leaderboards from two independent testers. But at every effort setting, another model matches or outperforms Sonnet’s Artificial Analysis score at a lower cost per task.

- Claude Haiku 5.5 (43) ranks 13th on Artificial Analysis’ __Intelligence Index__ , a composite of 10 tests across mathematics, science, coding, and reasoning, ahead of all the “fast” models. Claude Sonnet 5.5 set to max reasoning (56) ranks second. Sonnet 5.5 trails Claude Opus 5.5 set to max reasoning and ties Claude Opus 5.5 set to xhigh reasoning. But to get there, Claude Sonnet 5.5 generates about 193,000 output tokens per task, roughly 60 percent more than Opus, and the most that Artificial Analysis has measured. All three Claude 5.5 models (Opus, Sonnet, and Haiku) are the top three on this metric, with Opus and Sonnet first and second in cost per task.
- On Artificial Analysis’ __Coding Agent Index__ , which tests combinations of models and harnesses on three coding benchmarks, Claude Sonnet 5.5 set to max reasoning running in Claude Code ranks first (68), ahead of Claude Opus 5.5 in Claude Code and Gemini 4 Argon in Antigravity.
- On Vals AI’s __Vals Index__ , eight finance, coding, legal, and tax benchmarks weighted by each sector’s share of U.S. gross domestic product, Claude Sonnet 5.5 set to max reasoning ranks second (67.04 percent accuracy), behind Gemini 4 Argon set to high reasoning (68.9 percent) and just ahead of Claude Opus 5.5 set to max reasoning (66.97 percent).
- Both models are high-speed but also high-latency. Claude Haiku 5.5 generates 237 tokens per second, but takes 337 seconds on average to produce its first answer token. Claude Sonnet 5.5 set to max reasoning generates 127 tokens per second, versus 97 for Claude Opus 5.5 set to max reasoning and 58.3 for GPT-6.1 Sol set to max reasoning, but Sonnet’s average time-to-first token is 430 seconds.

**Behind the news:** Anthropic launched Claude Haiku 5.5 on October 7, Claude Sonnet 5.5 on September 28, and Claude Opus 5.5 on September 22, At launch, Opus 5.5 __led__ Artificial Analysis and Vals AI’s overall indices. Even with the later releases of Gemini 4 Argon and Muse Wonder and Muse Wonder Full, Claude Opus 5.5 and Sonnet 5.5 remain the top models on independent benchmark indices measuring overall intelligence. 

**Why it matters:** Per-token prices alone don’t tell developers what a task costs. On Artificial Analysis’ Intelligence Index tests, Claude Sonnet 5.5’s  cost per task varies more than eighteenfold across reasoning levels, and at max, it costs more per task than Claude Opus 5.5 set to max reasoning. On Vals AI’s tests, the cost is about two-thirds as much. Anthropic concedes Sonnet’s cost advantage over Opus is strongest at its lower reasoning levels. Claude Haiku 5.5, too, can be used as a reasoning model, but its cost advantages stand out for simpler tasks like summaries and classifications. Developers should measure each model’s cost at each relevant reasoning level on their own tasks, alongside accuracy and completion time, since each varies significantly.

**We’re thinking:** A higher reasoning level can change what an agent does, not just how long it reasons, and the change doesn’t always help. On FrontierCode, a test of whether an agent’s code changes are ready to merge without human edits, Anthropic says that Claude Sonnet 5.5 scores lower at max reasoning than at xhigh (46.2 percent versus 52.1 percent). Test your tasks on different effort levels to see what works best.
