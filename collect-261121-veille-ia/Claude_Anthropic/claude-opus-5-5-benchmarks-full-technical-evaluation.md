---
id: collect-261121-veille-ia/claude-anthropic/claude-opus-5-5-benchmarks-full-technical-evaluation
title: "Claude Opus 5.5 Benchmarks: Full Technical Evaluation"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["claude", "opus", "fable", "benchmarks", "evaluation", "astra", "openai", "gpt-6"]
source: https://www.layer3labs.io/guides/claude-opus-5-5-benchmarks
source_anchor: ""
source_lines: [1, 114]
section: "Claude_Anthropic"
---
# Claude Opus 5.5 Benchmarks: Technical Evaluation and Scores

Published evaluation scores for Claude Opus 5.5 across coding, reasoning, and workflow benchmarks compared to Claude Fable 5.1, Claude Opus 5, and rival models.

Claude Opus 5.5 leads published benchmarks in agentic coding and enterprise reasoning over its predecessor Claude Opus 5 and rival systems, while trailing OpenAI's Generative Pre-trained Transformer 6 Astra (GPT-6 Astra) in multi-step business workflow execution. Anthropic released the model on September 22, 2026, publishing benchmark results across software engineering, document analysis, and autonomous computer interaction. The published claude opus 5.5 benchmarks place the model ahead of Claude Fable 5.1 and Claude Opus 5 on Terminal-Bench 4.0 and GDPval-AA v2.1.

The release marks the first entry in Anthropic's Claude 5.5 artificial intelligence (AI) family. Operating costs drop by 40% compared to Claude Opus 5 on typical production workloads. Text generation speeds increase by more than 30% relative to the prior generation.

Evaluation audits from external research groups Frontier Design and METR (Model Evaluation and Threat Research) accompanied the launch announcement. Those audits measured behavioral boundaries and decision reversibility during autonomous execution tasks. For technical teams evaluating model migrations, these benchmark scores provide concrete metrics across coding environments, enterprise knowledge work, and workflow automation.

## Claude Opus 5.5 Benchmark Scores Across Core Test Suites

Claude Opus 5.5 scored 66.4% on Terminal-Bench 4.0 and achieved an Elo rating of 1846 on GDPval-AA v2.1 in evaluation data published by Anthropic on September 22, 2026. On Terminal-Bench 4.0, Claude Opus 5.5 surpassed Claude Fable 5.1 at 55.8%, Claude Opus 5 at 52.3%, and OpenAI GPT-6 Astra at 57.9% under high-effort settings. On the GDPval-AA v2.1 enterprise reasoning benchmark, Claude Opus 5.5's 1846 Elo rating outpaced Claude Fable 5.1 at 1735, Claude Opus 5 at 1708, and OpenAI GPT-6 Astra at 1542.

Software engineering benchmarks show similar margins for Claude Opus 5.5 over earlier Claude models. On FrontierCode v1.1 Main, Claude Opus 5.5 recorded 54.4%, leading OpenAI GPT-6 Astra at 53.3%, Claude Fable 5.1 at 50.3%, and Claude Opus 5 at 48.0%. On CursorBench 4.0, Claude Opus 5.5 achieved 57.8%, which improved on Claude Opus 5 at 46.6% by 11.2 percentage points.

Autonomous execution and multimodal benchmarks demonstrate broader operational coverage across system interfaces. Claude Opus 5.5 achieved 81.8% partial-task completion on OSWorld 2.0 operating system (OS) navigation tests. In Chartography visual chart recognition with tools, Claude Opus 5.5 scored 89.0% against 88.4% for Claude Fable 5.1, while reaching 58.7% on Terminal-Bench-Science 0.1.

- Terminal-Bench 4.0 (agentic terminal coding): Claude Opus 5.5 66.4%, OpenAI GPT-6 Astra 57.9%, Claude Fable 5.1 55.8%, Claude Opus 5 52.3%
- GDPval-AA v2.1 (enterprise knowledge work Elo rating): Claude Opus 5.5 1846, Claude Fable 5.1 1735, Claude Opus 5 1708, OpenAI GPT-6 Astra 1542
- FrontierCode v1.1 Main (software engineering synthesis): Claude Opus 5.5 54.4%, OpenAI GPT-6 Astra 53.3%, Claude Fable 5.1 50.3%, Claude Opus 5 48.0%
- CursorBench 4.0 (in-editor developer code completion): Claude Opus 5.5 57.8%, Claude Opus 5 46.6%
- AutomationBench (business workflow execution): OpenAI GPT-6 Astra 41.4%, Claude Opus 5.5 40.0%, Claude Fable 5.1 31.4%, Claude Opus 5 26.9%
- OSWorld 2.0 (operating system navigation, partial-task completion): Claude Opus 5.5 81.8%
- Chartography (multimodal visual chart recognition with tools): Claude Opus 5.5 89.0%, Claude Fable 5.1 88.4%
- Terminal-Bench-Science 0.1 (autonomous scientific investigation with tools): Claude Opus 5.5 58.7%

## Evaluation Criteria Behind Claude Opus 5.5 Benchmarks

Each technical benchmark in the Claude Opus 5.5 evaluation suite tests distinct operational abilities ranging from command-line debugging to multi-step workflow automation. Terminal-Bench 4.0 assesses agentic execution inside terminal interfaces, requiring models to write bash scripts, install software dependencies, and resolve compilation errors without human guidance. The claude opus 5.5 terminal-bench score of 66.4% reflects its capacity to finish command-line programming sequences without stalling.

Software development benchmarks evaluate distinct stages of the engineering lifecycle. FrontierCode v1.1 Main tests repository-level modifications across multi-file codebases, while CursorBench 4.0 evaluates in-editor code completions during live development. As an applied claude opus 5.5 coding benchmark, CursorBench measures how cleanly suggested edits merge into active codebases without syntax errors.

Enterprise and system benchmarks address broader operational demands across business tools. GDPval-AA v2.1 measures enterprise knowledge work through pairwise comparative evaluations of legal synthesis, financial spreadsheets, and policy documents. AutomationBench evaluates multi-step business workflow execution across software systems, while OSWorld 2.0 grades how accurately the model controls native mouse, keyboard, and application inputs.

- Terminal-Bench 4.0: Agentic terminal execution, command-line problem-solving, and automated environment configuration.
- FrontierCode v1.1 Main: Full repository navigation, cross-file bug fixing, and software architecture updates.
- CursorBench 4.0: In-editor developer suggestion accuracy, inline completion quality, and edit acceptance rates.
- GDPval-AA v2.1: Complex enterprise knowledge work, policy synthesis, and quantitative analytical documentation.
- AutomationBench: End-to-end multi-step workflow automation across business databases and digital interfaces.
- OSWorld 2.0: Direct operating system navigation, graphic user interface (GUI) control, and desktop task completion.
- Chartography: Tool-assisted interpretation of complex visual graphics, charts, and technical diagrams.
- Terminal-Bench-Science 0.1: Autonomous scientific experimentation, computational hypothesis testing, and research analysis.

## Performance Margins and Where Claude Opus 5.5 Trails Competitors

Claude Opus 5.5 delivers clear leads in coding accuracy and enterprise reasoning, yet it trails OpenAI GPT-6 Astra in business workflow execution. On AutomationBench, Claude Opus 5.5 achieved 40.0%, falling behind OpenAI GPT-6 Astra's 41.4% score. While Opus 5.5 improves over Claude Fable 5.1 at 31.4% and Claude Opus 5 at 26.9%, OpenAI maintains a 1.4 percentage point lead in multi-step task execution.

In software engineering and terminal execution, Claude Opus 5.5 establishes its widest performance gap over rivals. The claude opus 5.5 vs gpt-6 astra benchmark comparison on Terminal-Bench 4.0 shows an 8.5 percentage point advantage for Anthropic at 66.4% versus 57.9%. Against its direct predecessor, the claude opus 5.5 vs opus 5 benchmark comparison on Terminal-Bench reveals a 14.1 percentage point leap from 52.3% to 66.4%.

Enterprise knowledge work reveals an even sharper divergence. In the claude opus 5.5 vs claude fable 5.1 comparison on GDPval-AA v2.1, Opus 5.5 scored 1846 Elo points against Fable 5.1's 1735 and Opus 5's 1708. OpenAI GPT-6 Astra trailed significantly on this test, registering 1542 Elo points.

- Terminal-Bench 4.0: Claude Opus 5.5 leads all tested models at 66.4%, outperforming OpenAI GPT-6 Astra by 8.5 points and Claude Opus 5 by 14.1 points.
- FrontierCode v1.1 Main: Claude Opus 5.5 leads at 54.4%, edging out OpenAI GPT-6 Astra at 53.3% and Claude Fable 5.1 at 50.3%.
- GDPval-AA v2.1 Elo: Claude Opus 5.5 leads at 1846, holding a 111-point advantage over Claude Fable 5.1 and a 304-point margin over OpenAI GPT-6 Astra.
- CursorBench 4.0: Claude Opus 5.5 leads Claude Opus 5 by 11.2 percentage points (57.8% vs 46.6%).
- AutomationBench: OpenAI GPT-6 Astra leads at 41.4%, with Claude Opus 5.5 trailing at 40.0% despite strong gains over Claude Opus 5 (26.9%).

## Third-Party Safety Audits and Autonomous Boundary Testing

Pre-release safety evaluations conducted by Frontier Design and METR found that Claude Opus 5.5 produced the lowest rate of unauthorized boundary actions among tested models. METR tested the system for hard-to-reverse operational decisions during persistent agentic execution. In these evaluations, Claude Opus 5.5 adhered to administrative constraints more reliably than earlier generations, reducing unintended file system modifications and unprompted network calls.

Boundary discipline protects operational stability. High capability scores become operational liabilities if an autonomous agent executes destructive terminal commands or alters database configurations without authorization. Because Claude Opus 5.5 balances high Terminal-Bench performance with low boundary-violation rates, engineering teams can grant it command-line access with lower monitoring overhead.

Anthropic paired these behavioral findings with strict access verification programs for sensitive domains. Workloads involving biological research require approval through the Life Sciences Verification Program, while network defense tasks require enrollment in the Cyber Verification Program. When automated safety interventions trigger, cyber tasks fall back to Claude Opus 4.8, while biology and frontier AI training tasks fall back to Claude Opus 5.

- Unauthorized action reduction: Pre-release METR audits confirmed fewer unauthorized boundary breaches and unprompted administrative commands.
- Hard-to-reverse decisions: Lower rates of unconfirmed file deletions, schema alterations, or unapproved script executions during agent loops.
- Verification tiers: Restricted access programs govern deployment in biological research and defensive cybersecurity contexts.
- Automated safety fallbacks: System triggers route cybersecurity workflows to Claude Opus 4.8 and biological research tasks to Claude Opus 5.

## Workload Suitability Based on Claude Opus 5.5 Benchmarks

Benchmark distributions across coding, reasoning, and automation dictate distinct implementation paths for different operational buyers. Software engineering teams handling persistent debugging and repository maintenance benefit directly from the model's 66.4% Terminal-Bench 4.0 result and 57.8% CursorBench score. Coupled with a 40% reduction in operating costs and 30% faster token generation compared to Claude Opus 5, software organizations gain higher accuracy at reduced infrastructure expenditure.

Knowledge-work organizations managing regulatory filings, complex contracts, and policy synthesis find the strongest justification in the 1846 GDPval-AA v2.1 score. This rating demonstrates a substantial margin over OpenAI GPT-6 Astra's 1542, making Claude Opus 5.5 the preferred engine for analytical document drafting. For these workloads, input pricing of $4 per million tokens and cached input pricing of $0.20 per million tokens provide sustainable economics for large context processing.

Workflow automation buyers face a nuanced tradeoff between capability and execution completeness. While Claude Opus 5.5 marks a notable jump over Claude Opus 5 on AutomationBench (40.0% versus 26.9%), it does not surpass OpenAI GPT-6 Astra at 41.4%. Organizations whose primary workload consists of headless browser automation or cross-application form completion must evaluate whether Claude's lower token costs compensate for this slight completion deficit.

Claude Opus 5.5 is not suitable for basic single-step automations that require minimal reasoning or simple text categorization. Organizations operating high-volume transactional pipelines will overpay using Opus 5.5 and should wait for lighter options such as Claude Haiku 5.5 or use existing utility endpoints. Furthermore, engineering teams without verified institutional status will find biological and cybersecurity features blocked by Anthropic's mandatory verification programs.

- Recommended for engineering teams: High-leverage repository refactoring, terminal agent scripting, and active in-editor developer completion.
- Recommended for legal and finance analysts: Complex document verification, regulatory synthesis, and policy drafting scoring at 1846 Elo.
- Evaluate with testing for automation teams: Multi-step digital workflows where Claude's 40.0% AutomationBench score must be weighed against GPT-6 Astra's 41.4%.
- Not recommended for low-complexity utilities: Simple classification, standard sentiment tagging, and high-frequency basic chatbot endpoints.

## Conditions That Alter Published Benchmark Evaluations

Independent evaluation data and upcoming model releases will determine whether Claude Opus 5.5 maintains its competitive standing across enterprise workloads. All current evaluation figures originate from Anthropic's September 22, 2026 announcement documentation. If third-party testing laboratories publish divergent results from real-world production traffic, or if API reliability benchmarks show elevated latency under peak concurrency, current architectural recommendations will require adjustment.

The impending release of Claude Sonnet 5.5 and Claude Haiku 5.5 will directly affect the value calculus for technical teams. Anthropic has announced that both models will ship in the coming weeks following the Opus 5.5 release. If Claude Sonnet 5.5 replicates 90% or more of Opus 5.5's coding and reasoning benchmark scores at its traditional mid-tier pricing, standard production routing should move to Sonnet 5.5.

Pricing changes among frontier competitors and context window performance will also influence deployment viability. While the predecessor Claude Opus 5 offered a 1 million token context window, Anthropic has left specific token limits, request caps, and file upload limits unpublished for Opus 5.5. Teams planning a model transition should audit their internal prompts against published claude opus 5.5 benchmarks before updating production endpoints.

- Independent benchmark divergence: Third-party evaluation results that differ from Anthropic's published numbers would require updating model selections.
- Mid-tier model releases: The launch of Claude Sonnet 5.5 could offer comparable coding scores at lower per-token pricing.
- Context window documentation: Official publication of Opus 5.5's token limits and concurrency caps will clarify large-document economics.
- Competitor price moves: Rate card revisions from OpenAI or cloud hosting partners could alter the total cost of ownership.

## Frequently Asked Questions

- Claude Opus 5.5 scored 66.4% on Terminal-Bench 4.0, 54.4% on FrontierCode v1.1 Main, 57.8% on CursorBench 4.0, and 1846 Elo on GDPval-AA v2.1 in Anthropic's published evaluation data. It also achieved 40.0% on AutomationBench, 81.8% on OSWorld 2.0 partial-task completion, 89.0% on Chartography with tools, and 58.7% on Terminal-Bench-Science 0.1 with tools.
- Yes, Claude Opus 5.5 outperforms Claude Fable 5.1 across all direct benchmark comparisons published by Anthropic. It leads Fable 5.1 on Terminal-Bench 4.0 (66.4% vs 55.8%), FrontierCode v1.1 Main (54.4% vs 50.3%), GDPval-AA v2.1 (1846 vs 1735 Elo), AutomationBench (40.0% vs 31.4%), and Chartography (89.0% vs 88.4%).
- Claude Opus 5.5 leads OpenAI Generative Pre-trained Transformer 6 Astra (GPT-6 Astra) in coding and enterprise reasoning, but trails it in business workflow automation. Opus 5.5 scored higher on Terminal-Bench 4.0 (66.4% vs 57.9%), FrontierCode v1.1 Main (54.4% vs 53.3%), and GDPval-AA v2.1 (1846 vs 1542 Elo), while GPT-6 Astra scored higher on AutomationBench (41.4% vs 40.0%).
- Yes, Claude Opus 5.5 records substantial improvements over Claude Opus 5 across all published programming evaluations. It leads Opus 5 on Terminal-Bench 4.0 (66.4% vs 52.3%), FrontierCode v1.1 Main (54.4% vs 48.0%), and CursorBench 4.0 (57.8% vs 46.6%), while generating code more than 30% faster.
- External evaluators Frontier Design and METR (Model Evaluation and Threat Research) conducted pre-release behavioral safety audits for Claude Opus 5.5. Anthropic reported that Opus 5.5 achieved the lowest rate of unauthorized boundary actions and hard-to-reverse decisions among all models tested prior to launch.
- No, Claude Opus 5.5 does not lead every benchmark. On AutomationBench, which measures multi-step business workflow execution, Claude Opus 5.5 scored 40.0%, trailing OpenAI GPT-6 Astra's 41.4% result. However, Opus 5.5 does lead all tested predecessor models from Anthropic on that suite.
- Technical teams can verify official benchmark results and methodology on Anthropic's Claude Opus 5.5 announcement page. Current per-token pricing and account tier rate limits are documented on Anthropic's pricing page.

### Evaluate Claude Opus 5.5 for Production Workflows

Book an AI workflow review with Layer3 Labs to benchmark model accuracy, estimate API token costs, and evaluate safe integration paths.

Book an AI Workflow Review
