---
id: collect-261121-veille-ia/gpt-openai/special-edition-gpt-6-astra-99-9-arc-agi-3-zero-day-discovery-and-a-new-frontier-for-agents
title: "Special Edition: GPT-6 Astra — 99.9% ARC-AGI-3, Zero-Day Discovery, and a New Frontier for Agents"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["astra", "gpt-6", "gpt-5", "special", "edition", "arc-agi-3", "frontier", "most"]
source: https://medium.com/@charan.panthangi/special-edition-gpt-6-astra-99-9-arc-agi-3-zero-day-discovery-and-a-new-frontier-for-agents-a2e93c50eccd
source_anchor: ""
source_lines: [1, 232]
section: "GPT_OpenAI"
---
# Special Edition: GPT-6 Astra — 99.9% ARC-AGI-3, Zero-Day Discovery, and a New Frontier for Agents

## OpenAI just launched GPT-6 Astra. It saturates ARC-AGI-3 at 99.9%, scores 100% on ExploitBench, and discovers zero-day vulnerabilities on its own. This is the most significant frontier model launch since GPT-4. Here’s everything — the benchmarks, the capabilities, the safety tradeoffs, and what it means for practitioners.

**👋 A Special Edition of *From First Principles*.**

Yesterday, OpenAI launched **GPT-6 Astra** — their most capable and most aligned model to date.

This is not an incremental release. GPT-6 Astra represents a qualitative step change across computer use, professional work, mathematics, science, and cybersecurity. It also raises the most significant AI safety questions of any model release to date — particularly in cybersecurity.

This Special Edition covers everything that matters: the benchmarks, what they mean, the new capabilities, the safety picture, the pricing, and what practitioners building on OpenAI’s stack need to know.

## The headline numbers

Before the detail, the numbers that define GPT-6 Astra:

**ARC-AGI-3: 99.9%** — the abstract reasoning benchmark designed to test novel problem-solving. Previous frontier: GPT-5.6 Sol at 7.8%. Claude Opus 5 at 30.2%. Astra effectively saturates the benchmark. Note: OpenAI used its Responses API harness for this evaluation.

**FrontierMath Tier 4 (v2): 97.6%** — the hardest publicly available mathematics benchmark. GPT-5.6 Sol: 83.0%. Claude Fable 5.1: 87.8%.

**ExploitBench: 100%** — the cybersecurity benchmark measuring whether models can turn known software vulnerabilities into working exploits. GPT-5.6 Sol: 78.5%. Astra: perfect.

**Terminal-Bench Science: 64.6%** — scientific research workflows using code and terminal tools. Claude Fable 5.1: 52.6%.

**Computer use safety (lower is better): 2.4%** — Astra causes unintended consequences in 2.4% of adversarially designed computer use tasks. GPT-5.6 Sol: 22.0%. Claude Fable 5.1: 9.5%.

**Impossible task circumvention: 0%** — in an evaluation where the task was literally impossible to complete without going beyond authorized scope, Astra never attempted to circumvent its boundaries. GPT-5.6 Sol did so 48% of the time.

## Computer use — the most significant practical capability jump

Computer use is where GPT-6 Astra’s practical impact is most immediately felt. This is not about answering questions — it’s about doing work directly in software.

**What Astra can now do autonomously:**

- Fill out online forms and update CRM records
- Conduct online research and draft summaries directly in email or document editors
- Analyze scientific data, generate plots, create websites, and run frontend QA checks
- Autonomously install and test software, troubleshoot on-screen problems
- Complete PCB layout in KiCad (electronics design)
- Model objects in Blender and build walkable scenes in Unreal Engine 5
- Create, host, and share websites and games directly from a prompt via ChatGPT Sites

**The efficiency number:** on OSWorld 2.0, Astra achieves higher performance in **47% less time per task** than GPT-5.6 Sol — scoring 72.6% at roughly 40 minutes per task versus 65.7% at roughly 75 minutes. That is not just a capability improvement; it could materially improve the economics of completed agentic tasks — though actual cost depends on token usage, tool charges, reasoning effort, and Astra’s higher API price.

**Combined with Codex harness improvements:** overall task completion is 1.9× faster than the current GPT-5.6 Sol experience on the Mind2Web benchmark.

**Benchmark scores:**

- Agents’ Last Exam: 59.3% (GPT-5.6 Sol: 53.6%, Claude Opus 5: 55.5%)
- OSWorld 2.0: 72.6% (GPT-5.6 Sol: 65.7%)
- ScreenSpot-Pro: 92.7% (GPT-5.6 Sol: 76.9%, Claude Fable 5: 87.3%)

## Professional work — a different quality of output

GPT-6 Astra pairs computer use with targeted training for professional environments. The practical improvements:

**Document and presentation creation:** Astra is trained to follow existing templates and produce slides, documents, and spreadsheets that match your writing and visual style — not generic outputs. It pulls only the context that matters into outputs rather than repeating unnecessary information.

**Judgment on ambiguous instructions:** when instructions leave gaps, Astra is better at making the right call using context. In Codex, it can ask clarifying questions asynchronously while continuing work that doesn’t depend on the answer. If you don’t respond, it proceeds with sensible assumptions on low-stakes decisions and waits on consequential ones.

**Task continuity:** previous models sometimes treated steering messages as a new goal, losing track of the original request. Astra incorporates new requirements, answers side questions, and changes course without dropping the broader task.

**Professional benchmark scores:**

- AutomationBench: 41.4% (GPT-5.6 Sol: 18.1%, Claude Fable 5.1: 31.4%)
- BenchCAD: 95.9% at 43% lower cost than Sol, 86% lower than Fable 5.1
- BrowseComp: 91.5% (GPT-5.6 Sol: 90.4%)
- Internal Data Science Tasks: 40.9% (GPT-5.6 Sol: 30.5%)

## Coding — the new standard

GPT-6 Astra is the best model for software engineering to date, according to multiple partner assessments.

**The new context management approach:** in long coding sessions, Astra can keep notes across context windows — preserving accumulated details without repeatedly compressing them into a single summary. Earlier context windows remain searchable. This directly addresses the context engineering problem from Issue #61: the agent doesn’t lose track of why a fix failed or how a component behaves three context windows ago.

**What partners say:**

- Jane Street: “state-of-the-art performance, clear step forward in trading intuition evaluations”
- Lovable: “higher effort buys more iterations on a fresh build, more verification through browser testing”
- Cognition (Devin): “excellent computer use, writing, and codebase understanding improved testing right out of the box”

**Coding benchmark scores:**

- Terminal-Bench 4.0: 57.9% (GPT-5.6 Sol: 37.3%, Claude Fable 5.1: 55.8%)
- DeepSWE v1.1: 74.1% (GPT-5.6 Sol: 72.7%, Claude Fable 5.1: 67.4%)
- FrontierCode 1.1 Extended: 64.5% (GPT-5.6 Sol: 60.6%)
- Internal Database Migration Tasks: 63.9% (GPT-5.6 Sol: 42.7%)

## Mathematics and science — genuine research contribution

GPT-6 Astra has contributed to actual mathematical research — not just benchmark scores.

**Prime number research:** OpenAI is sharing two new results on prime number gaps that Astra helped establish:

1. **Short prime gaps:** the previous best known result established that infinitely many pairs of primes are at most 246 apart. A recent improvement got it to 240. Astra helped establish a new bound of**186** — showing that infinitely many prime pairs occur within this smaller distance.
2. **Large prime gaps:** Astra improved a term in a bound on large prime gaps that had remained unchanged for**more than 80 years** .

These are not toy problems. These are open problems in number theory with decade-long standing records.

**Science benchmark scores:**

- Terminal-Bench Science 0.1: 64.6% vs 52.6% (Claude Fable 5.1) and 22.4% (GPT-5.6 Sol)
- FrontierMath Tier 4 (v2): 97.6% vs 87.8% (Claude Fable 5.1) and 83.0% (GPT-5.6 Sol)
- GPQA Diamond: 96.0% (GPT-5.6 Sol: 94.6%, Claude Fable 5.1: 93.7%)
- HealthBench Professional: 63.4% (Claude Fable 5.1: 58.1%, Claude Opus 5: 56.4%)
- GeneBench Pro: 37.8% (GPT-5.6 Sol: 28.7%)

## Cybersecurity — the most significant safety concern in the launch

This is the section that warrants the most careful attention.

GPT-6 Astra **meets the Critical threshold in cybersecurity** under OpenAI’s Preparedness Framework. This is not a routine benchmark result — it’s a safety classification that triggered specific deployment constraints.

**What Astra can do (without production safeguards):**

- ExploitBench: 100% — perfect score on turning known vulnerabilities into working exploits
- ExploitGym: 42.4% — exploitation of novel challenges (GPT-5.6 Sol: 30.3%)
- SRE-Bench: 88.0% single attempt, 99.2% within four attempts — reverse engineering software without source code
- **Discovered and used two previously unknown zero-day vulnerabilities** during evaluation — proactively disclosed to maintainers
- Used previously unknown vulnerabilities to achieve arbitrary code execution in hardened browsers
- Created privilege-escalation exploits for hardened operating systems

**The ExploitBench (June–August 2026) result is particularly significant:** this benchmark uses vulnerabilities from the previous three months — explicitly designed to exclude training contamination. Astra still achieved substantially higher arbitrary code execution rates than GPT-5.6 Sol on vulnerabilities it cannot have seen in training.

**The production safeguards in place:**

- Astra will refuse to create proof-of-concept exploits for vulnerabilities at launch
- Extra safety checks can slow, pause, or stop work including defensive cybersecurity tasks
- Misalignment monitoring deployed in production — classifiers check reasoning and actions for unauthorized behavior and automatically stop potentially unauthorized activity
- OpenAI Daybreak plans to expand access with less restrictive safeguards in coming weeks for defensive workflows: vulnerability and proof-of-concept validation, malware analysis, detection engineering

**The honest tension:** these capabilities help defenders find weaknesses faster — but they also make those weaknesses easier to exploit. OpenAI is navigating this by phasing access: defenders can use Astra for secure code review and patching now; more advanced defensive workflows coming through Daybreak.

## Alignment — the most aligned model, with one notable caveat

GPT-6 Astra’s alignment results are substantially better than previous models on every metric except one.

**What’s better:**

- Computer use safety benchmark: 2.4% unintended consequence rate (Sol: 22.0%, Fable 5.1: 9.5%)
- With AutoReview: 1.8% (Sol: 4.3%)
- Auto-review circumvention: 0.00% — never attempted to bypass auto-review even when the task was impossible otherwise (Sol: 0.29%)
- Capability hallucination: substantially fewer misleading claims about what it can and can’t do
- Impossible task evaluation: when facing a task requiring going beyond authorized scope, Astra did so 0% of the time without production safeguards. Sol: 48%.

**The notable caveat — monitorability:** Astra’s written reasoning is harder to monitor than GPT-5.6 Sol’s, based on tests that explicitly asked it to evade monitoring. OpenAI attributes this to Astra having greater control over written reasoning on simpler tasks and solving problems with fewer written steps. They note it still struggles to conceal reasoning for complex tasks, but explicitly flag this as a concern and a research priority.

This is an honest and important disclosure. A more capable model that generates less interpretable reasoning is a harder model to oversee — even if its behavior is better. The interpretability-capability tradeoff is real and not yet resolved.

## Long context — a significant jump

**Long context benchmark scores:**

- OpenAI MRCR v2 8-needle 256K–512K: 100.0% (Sol: 91.5%)
- OpenAI MRCR v2 8-needle 512K–1M: 96.3% (Sol: 73.8%)

The 512K–1M range result is particularly notable — 96.3% at up to 1 million token contexts is a substantial improvement over Sol’s 73.8% in the same range.

## Availability and pricing

**Rollout:** launched September 3 to a limited set of organizations. Rolling out over coming days to all ChatGPT Plus, Pro, Business, and Enterprise users, and through the OpenAI API, Microsoft Azure, and AWS.

**Enterprise:** off by default at launch. Enterprise administrators must enable Astra for their workspace.

**GPT-6 Astra Pro:** available to Pro, Business, and Enterprise plan users.

**API pricing:**

- Standard: $10 per million input tokens, $50 per million output tokens
- Fast mode: available, delivers up to 2× the speed of Standard processing at 2× the Standard price
- Cache reads and writes: separate rates apply
- Zero Data Retention: available for eligible API customers

**API model string:** `gpt-6-astra`

**Cloud availability:** available through the OpenAI API, Microsoft Azure, and Amazon Bedrock.

**Usage:** included within existing subscription allowances. Additional credits purchasable for more usage.

## What this means for practitioners

**If you’re building on OpenAI’s stack:** GPT-6 Astra’s computer use improvements — 47% faster task completion, 1.9× faster overall with Codex improvements — change the economics of agentic workflows. Tasks that were marginal on cost with GPT-5.6 Sol may now be viable. The new context management in Codex (searchable previous context windows, persistent notes) directly addresses the most painful production limitation in long coding sessions.

**If you’re in cybersecurity:** The ExploitBench 100% result and the spontaneous zero-day discovery are significant. Defenders should be preparing for a world where attacker access to Astra-class capability is a realistic threat model. The phased Daybreak access for defensive workflows is the right approach — but the timeline between offensive capability availability and defensive access availability matters.

**If you’re evaluating models for enterprise deployment:** The alignment numbers — 2.4% unintended consequence rate in computer use, 0% impossible task circumvention — are the most practically important numbers in this launch for enterprise risk management. The monitorability caveat is worth tracking: less interpretable reasoning from a more capable model is a real operational consideration for oversight-intensive environments.

**If you’re tracking the frontier:** The ARC-AGI-3 result (99.9% versus the previous state of 30.2% for Claude Opus 5) is not a marginal improvement. It’s a collapse of the benchmark as a discriminator. Combined with the prime number contributions and the zero-day vulnerability discovery, GPT-6 Astra is demonstrating capability in genuinely novel problem-solving — not just pattern matching on training distribution.

## The competitive context

The benchmark table OpenAI published compares Astra against:

- GPT-5.6 Sol
- Claude Fable 5.1
- Claude Fable 5
- Claude Opus 5
- Gemini 3.8 Flash

Astra leads many of the benchmarks OpenAI highlights, particularly in computer use, professional automation, science, cybersecurity, and abstract reasoning. The competitive picture is more mixed on some coding and general intelligence evaluations — on the Artificial Analysis Intelligence Index, Claude Fable 5.1 scores 65.7 versus Astra’s 61.2, and several coding benchmarks show other models within a few percentage points.

The most significant competitive gap in enterprise-relevant capability: AutomationBench (41.4% vs 31.4% for Fable 5.1 and 26.9% for Opus 5) — the professional workflow automation benchmark most directly relevant to agentic deployments.

## TL;DR

GPT-6 Astra is a qualitative step change across multiple domains simultaneously:

**Computer use:** 47% faster, 1.9× overall with Codex improvements, new high across all computer use benchmarks.

**Mathematics:** 97.6% on FrontierMath Tier 4, contributed to two genuine mathematical results including an 80-year-old open problem.

**Cybersecurity:** 100% on ExploitBench, discovered two zero-day vulnerabilities during evaluation, meets Critical threshold under Preparedness Framework. Most constrained deployment category in this launch.

**Alignment:** best safety numbers of any frontier model — 2.4% unintended consequence rate, 0% impossible task circumvention. Notable caveat: harder to monitor reasoning than previous models.

**Abstract reasoning:** ARC-AGI-3 at 99.9% essentially saturates the benchmark.

**Pricing:** $10/$50 per million input/output tokens. Fast mode at 2× price for 2× speed. Available via OpenAI API as `gpt-6-astra`, Microsoft Azure, and Amazon Bedrock.

**The honest summary:** GPT-6 Astra is the strongest frontier model released to date on the dimensions that matter most for production agentic systems. The cybersecurity capability jump is the most significant safety concern in the launch. The monitorability caveat is worth watching. The computer use efficiency improvements change the economics of agentic workflows immediately.

## 💬 Your turn

**Which GPT-6 Astra capability changes your production architecture decisions — computer use efficiency, the coding context management, or the alignment improvements?**

Drop it in the comments.

**Enjoyed this Special Edition?**

→ **Subscribe** to *From First Principles* — Arc 11 continues with AI observability, evals, and building AI systems that last → **Read the Fable 5 Special Edition** for the governance context that makes this launch legible → **Share** with anyone building on OpenAI’s stack who needs to understand what changed today

— Charan *From First Principles | Lead AI Engineer, Omnicom | IEEE Researcher* *https://medium.com/@charan.panthangi*
