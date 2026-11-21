---
id: collect-261121-veille-ia/ia-agents/apex-35b-benchmarks-coding-biology-and-data-cleaning-tested
title: "Apex-35B Benchmarks: Coding, Biology, and Data Cleaning Tested"
domain: ia-agents
role: reference
task: article
actors: []
dates: []
keywords: ["apex-35b", "model", "coding", "data", "biology", "tested", "billion", "only"]
source: https://www.mindstudio.ai/blog/apex-35b-benchmark-test
source_anchor: ""
source_lines: [1, 108]
section: "IA_Agents"
---
## What is Apex-35B?

Apex-35B is a sparse mixture-of-experts language model built by Apex Intelligence on top of Qwen 3.6. It carries 35 billion total parameters but activates only about 3 billion per token, which keeps inference costs close to a much smaller dense model while retaining more knowledge in reserve. It supports a context window of up to 262,000 tokens, decides on its own how long to “think” before answering, and ships under the Apache 2.0 license, meaning it’s free to use, modify, and deploy commercially.

The more unusual part is how it was trained. Apex Intelligence built an internal system called Apex Research that automated large chunks of the model development pipeline: generating training data, running experiments, tuning the training recipe, and optimizing GPU kernels. In other words, AI tooling did meaningful work in building the model that then gets compared against much bigger systems like GLM and DeepSeek.

## TL;DR

- **Sparse MoE efficiency** means Apex-35B has 35 billion parameters total but only around 3 billion active per token, so it runs like a small model while drawing on a larger knowledge base.
- **Local deployment is realistic** : a Q8 quantized version ran on a single H100 using just over 38GB of VRAM, with full BF16 precision also available for those with more headroom.
- **Real-world coding tasks landed well** , including finding and fixing a billing bug duplicated across two screens of a live solar-operations dashboard, complete with a regression test, in about five and a half minutes.
- **Visual/creative coding was weaker** : a single-file HTML rain simulation hit most of the functional requirements but looked rougher than expected, with thick trails and visible noise artifacts.
- **Data cleaning and reporting held up** , turning a messy year of e-commerce order data (duplicate rows, inconsistent dates, inconsistent country names) into a cleaned dataset, a problem log, a chart, and a findings report grounded only in the actual numbers.
- **Genetics reasoning was a standout** , correctly solving a multi-step inheritance probability problem (66.7% carrier probability) after roughly five minutes of extended reasoning, including resisting several deliberate trap conditions in the prompt.
- **Benchmark tables show Apex-35B beating its own base model and similarly sized competitors** on research-style tests, and even edging out GLM 5.3 on two tests and DeepSeek on one hard biology question, despite being a fraction of their size.

*regulatory compliance and security.*

- Name
- Maria Alvarez
- Date of birth
- SSN
- Diagnosis
- Balance due
- $412.80

## How was Apex-35B actually tested?

The testing described here wasn’t synthetic benchmark-running alone. It paired standard benchmark comparisons with hands-on tasks against a real, already-running full-stack application: a solar panel operations dashboard used by a company operating across several Asian countries. That app has a live fleet dashboard, a cost-savings calculator (“code studio”), and a tariff explorer screen.

A real bug existed in the app: a payback calculation showing an unrealistically fast 1.8-year return on investment, caused by the same underlying error surfacing on two separate screens (the savings calculator and the electricity tariff chart, which should render as a smooth curve but instead showed sudden jumps). Apex-35B was given no hint about where the bug lived or what it was. It was asked only to find and fix it.

Beyond the coding fix, the model was tested on a creative canvas animation task (simulating rain on a foggy window using nothing but a single HTML file with no external libraries), a data-cleaning task using a deliberately messy spreadsheet of a year’s worth of online orders, and a logic-heavy biology word problem involving genetic inheritance probabilities.

## How did Apex-35B perform on the coding tasks?

On the bug-fix task, Apex-35B did well. It located the shared root cause behind two different symptoms, rewrote the relevant function, rebuilt only the affected API service rather than the whole app, and added a regression test to guard against the bug reappearing. The whole process took about five and a half minutes. After the fix, the payback period in the dashboard changed from the suspicious 1.8 years to a more realistic 2.7 years, and the 25-year savings projection dropped from roughly $44,000 to about $30,000. The tariff chart also rendered correctly as a smooth curve instead of a stepped one.

The creative coding test told a different story. Asked to build a single HTML file simulating rain on a foggy city window at night, complete with drops that slide, merge, and act like lenses, wipeable fog, and lightning flashes, Apex-35B took about 24 minutes and reported that it had verified every requested feature. The output did include most of the described elements: a city backdrop, a foggy window, wipeable fog, and moving rain. But the visual execution fell short of the prompt’s intent. Rain trails looked like thick, dark ropes rather than thin, lens-like streaks, and there were visible noisy square patches on the canvas. The functionality was mostly there; the polish wasn’t.

## How well does Apex-35B handle data cleaning and analysis?

The spreadsheet test is one of the more practically relevant evaluations for anyone considering this model for business workflows. The source file was intentionally messy: duplicate order rows, dates formatted four different ways, country names spelled inconsistently, and prices or quantities with invalid values. The instructions asked the model to clean the data, produce one chart, and generate three findings a store owner could act on, without inventing any numbers that weren’t actually in the data.

**1,284**

**37**

**6**

| Vendor | Tier | Risk | Approval <sup>NEW</sup> | 
|---|---|---|---|
| **Northwind Supply** | Tier 1 | Low | Approved | 
| **Acme Logistics** | Tier 2 | Medium | Approve | 
| **Helios Data** | Tier 1 | High | Pending | 
| **Granite Freight** | Tier 3 | Low | Approved | 
| **Pinecrest Labs** | Tier 2 | Low | Approve | 

Apex-35B produced four outputs: a cleaned file with around 1,400 orders, a log documenting every data problem it found and corrected, a chart, and a markdown findings report. The chart showed that electronics accounted for roughly half of total revenue but also had a disproportionately high return rate, about one in five orders. Some chart labels overlapped, a minor cosmetic issue, but the underlying analysis and the written findings report were coherent, specific, and tied directly back to the cleaned data rather than fabricated.

## Does Apex-35B beat GLM 5.3 and DeepSeek?

Not across the board, but it does in specific cases. According to benchmark comparisons covered in testing, Apex-35B consistently outperforms the Qwen 3.6 base model it was built on, and it also beats Flint, a separate model of comparable parameter size. Against much larger systems like GLM 5.3 and DeepSeek, the bigger models generally lead, which is expected given the parameter gap.

The notable exceptions: Apex-35B surpassed GLM 5.3 on two research-oriented benchmark tests and beat DeepSeek on one of the harder biology reasoning questions. For a model with only about 3 billion active parameters per token, beating systems many times its size on even a subset of tasks is a meaningful result. It doesn’t win everywhere, and nobody testing it should expect it to replace a frontier-scale model across the board. But it closes the gap on specific reasoning-heavy tasks more than its size would suggest.

## How did Apex-35B do on genetics and reasoning problems?

The genetics test was arguably the strongest showing. The prompt described a family history involving a genetic condition, a carrier test with a known chance of producing a false negative, and two healthy children, then asked the model to calculate exact probabilities across several related questions, using reasoning alone with no code execution or external tools.

Apex-35B worked through the problem for about five minutes, visibly longer than its base model would, before arriving at correct answers across the board, including a 66.7% probability figure for one of the questions. It correctly identified that a sibling’s affected status implied both parents were carriers, which was a necessary deductive step buried in the problem. It also avoided several deliberately placed reasoning traps in the prompt. The one soft spot was a secondary figure (1.98%) where it skipped a small statistical correction, a minor miss rather than a fundamental reasoning failure.

## Is Apex-35B worth running locally?

For teams that want a capable model they can self-host without committing enormous VRAM, Apex-35B is a reasonable option. The quantized Q8 version ran comfortably within 38GB of VRAM on a single H100, which puts it within reach of much smaller setups than the 80GB card used for testing, especially for those renting GPU capacity rather than owning it outright. Its Apache 2.0 license also removes licensing friction for commercial use.

Where it clearly delivers: real bug fixes in existing codebases, structured data cleaning with business-relevant reporting, and multi-step logical or scientific reasoning. Where it’s shakier: highly visual, aesthetically demanding generative tasks, where functional correctness didn’t translate into polished output. For reasoning and coding-adjacent work, it earns its place. For creative visual coding, treat its output as a first draft rather than a finished product.

## Frequently Asked Questions

### What base model is Apex-35B built on?

Apex-35B is built on Qwen 3.6, using a sparse mixture-of-experts architecture with 35 billion total parameters and roughly 3 billion active parameters per token.

### How much VRAM does Apex-35B need to run?

**Live with Dmitry Shapiro**Ex-Google product lead · Ex-MySpace CTO

What you'll learn · Dec 2

1. 01How enterprises really build with AI
2. 02What full-stack means and why it's critical for business software
3. 03Prompting that holds up in production
4. 04Build and deploy your own app, live

The Q8 quantized version used just over 38GB of VRAM when served locally with llama.cpp. Full BF16 precision will require more, and lower-precision quantizations can reduce the requirement further for smaller GPUs.

### What license is Apex-35B released under?

Apex-35B is released under the Apache 2.0 license, allowing free commercial use, modification, and redistribution.

### Does Apex-35B beat larger models like GLM 5.3 or DeepSeek?

Not consistently. Larger models generally lead overall, but Apex-35B outperformed GLM 5.3 on two research benchmark tests and beat DeepSeek on one difficult biology reasoning question, which is notable given its much smaller active parameter count.

### What does “self-improving” mean for Apex-35B’s training?

Apex Intelligence used an internal automated system called Apex Research to help generate training data, run experiments, tune the training recipe, and optimize GPU kernels, meaning AI tooling handled significant parts of the model’s own development process.
