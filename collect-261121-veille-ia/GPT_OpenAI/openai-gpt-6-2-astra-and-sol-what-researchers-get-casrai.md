---
id: collect-261121-veille-ia/gpt-openai/openai-gpt-6-2-astra-and-sol-what-researchers-get-casrai
title: "OpenAI GPT-6.2 Astra and Sol: what researchers get — CASRAI"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["gpt-6", "astra", "openai", "models", "work", "cybersecurity", "access", "both"]
source: https://casrai.org/news/openai-gpt-6-2-astra-sol-release-2026
source_anchor: ""
source_lines: [1, 85]
section: "GPT_OpenAI"
---
OpenAI released GPT-6.2 Astra and GPT-6.2 Sol on 17 November 2026, making both models available in ChatGPT Work and Codex for Plus, Pro, Enterprise and Edu users and through the API as gpt-6.2-astra and gpt-6.2-sol. For universities and research offices the release brings an Edu rollout, a 20% cut in Astra’s API prices, and tighter default safeguards for higher-risk cybersecurity requests.

What happened

OpenAI announced the two models on 17 November 2026 in a post in its Developer Community announcements forum. It describes GPT-6.2 Astra as its frontier model for the hardest projects and GPT-6.2 Sol as an efficient, versatile model for everyday work and multistep agentic workflows. The Decoder reports that OpenAI calls Astra “the world’s best model for scientists.”

OpenAI says Astra sets new highs on evaluations across professional work and scientific research at less than a third of the cost per task of GPT-6 Astra. On Agents’ Last Exam, Astra at its xhigh setting scores 5.2 points above GPT-6 Astra at max, at less than half the cost. On Cognition’s FrontierCode 2.0, which measures whether changes to real codebases are good enough for maintainers to merge, Sol comes within half a point of Astra at about one-fifth of the cost, according to 9to5Mac.

Key details

Both models launched with tighter safeguards for higher-risk cybersecurity assistance, which OpenAI attributes to their increased capabilities. The table summarises the access and price terms reported by OpenAI and the outlets covering the launch.

Item

GPT-6.2 Astra

GPT-6.2 Sol

Positioning

Hardest projects, professional and scientific work

Everyday work and multistep agentic workflows

ChatGPT Work and Codex

Plus, Pro, Enterprise, Edu

Plus, Pro, Enterprise, Edu

API model name

gpt-6.2-astra

gpt-6.2-sol

API input / cached input / output, per million tokens

$8 / $0.40 / $40

$2 / $0.10 / $10

Price versus predecessor

20% below GPT-6 Astra

Same as GPT-6.1 Sol

Pro subscribers at $500 a month get Ultrafast mode with both models; Pro subscribers at $200 get it starting with Sol. 9to5Mac reports that eligible Enterprise and Edu workspaces get Ultrafast once an administrator enables it. Ultrafast generates tokens at up to 4.5 times the standard rate but uses more of a plan’s allowance. OpenAI also says both models finish demanding tasks with fewer output tokens, which stretches usage limits.

Background and context

GPT-6 Astra launched in September 2026 with a system card in which OpenAI rated it Critical for cybersecurity under its Preparedness Framework; CASRAI covered that rating in OpenAI rates GPT-6 Astra Critical for cybersecurity. The 17 November release is an incremental upgrade of the same two-model line, and Sol keeps its GPT-6.1 pricing. CASRAI also covered OpenAI’s earlier expansion of free access for academic researchers in OpenAI expands free AI access to 200,000 researchers.

Evidence from the system card and independent evaluators

OpenAI’s system card states that both models meet the Critical threshold for cybersecurity capabilities and the High threshold, but not Critical, for biological and chemical capabilities. Astra is the first externally released model to meet the High threshold for AI self-improvement; Sol does not. On OpenAI’s internal ExploitBench Internal Port, Astra reaches a 53.5% arbitrary code execution rate and Sol 50.5%, against 31.5% for GPT-6 Astra. The card says default restrictions on higher-risk cyber assistance were tightened and that more permissive access requires additional vetting. It also lists two Trusted Access programmes, one for biology (GPT-Rosalind) and one for cybersecurity (Trusted Access for Cyber).

The Decoder’s coverage adds third-party measurements. Artificial Analysis scores Astra 62 on its Intelligence Index, up 5 points from GPT-6 Astra and second to Claude Fable 5.2 at 65, with Sol at 57. Artificial Analysis finds coding scores flat for Astra and 3 points lower for Sol than GPT-6.1 Sol, and, at max effort, hallucination rates of 59% for Astra and 55% for Sol on its AA-Omniscience test, whose questions are unusually hard. Epoch AI reports that on its real-world research tasks Astra trails Claude Fable 5.2 and barely improves on GPT-6 Astra, and that Astra solved 7 of 68 FrontierMath Erdős tasks, each verified in Lean, against a previous record of 3. Vals AI ranks Astra first on its Science Index.

Reaction

Coverage has been mostly descriptive. 9to5Mac quotes early testers as reporting “cleaner code style and more focused tests, with fewer rounds of revision” in Codex. The Decoder cautions that most headline gains are OpenAI-reported, that the cyber results come from internal tests with safeguards and classifiers disabled, and that the stricter cyber safeguards may also limit legitimate security research. On the OpenAI community thread, users reported not seeing the models after updating; an OpenAI community staff member said availability depends on plan, workspace permissions and rollout progress. ANI, in a report carried by The Tribune, notes that the performance and cost comparisons are OpenAI’s own claims.

What it means for researchers, universities and research offices

Edu access and administration. Edu workspaces are among the plans that receive both models, and Ultrafast is switched on by an administrator. Research computing and IT teams should check which workspace permissions govern model selection before staff ask why a model is missing from the picker.

API access for research groups. Groups that call the API can use gpt-6.2-astra and gpt-6.2-sol. Astra’s 20% lower token prices and Sol’s unchanged prices matter for grant budgets that fix a model per project: re-estimating per-task cost is more reliable than assuming a price change carries through, because OpenAI says both models use fewer output tokens on demanding tasks.

Security research. Because both models are rated Critical for cybersecurity, default behaviour on higher-risk cyber requests is stricter, and more permissive access requires additional vetting through OpenAI’s programmes. Vulnerability-research groups and teams working on research security should expect refusals on requests that earlier models answered, and should look at the vetting route rather than rely on rephrasing prompts.

Evidence for model choice. The independent figures point in different directions: strong results on science benchmarks from Vals AI and Artificial Analysis, but weaker real-world research performance in Epoch AI’s tasks and high hallucination rates on very hard factual questions. Research offices drafting guidance on large language model use should treat vendor benchmarks as a starting point and ask groups to validate outputs on their own tasks, and keep AI tool disclosure requirements for publications tied to the specific model name and version used.

Procurement and data governance. A model upgrade changes the product behind an existing contract line. Offices that approved a specific model for restricted data should confirm whether the approval covers the new model names before groups switch.

What happens next

OpenAI says it is working to bring the models to Daybreak, where approved defenders could access them with reduced refusals. Rollout in ChatGPT continues by plan and workspace, according to OpenAI’s community staff.

Full access: large monthly usage, up to 10 source passages behind each answer, and MCP and API access for Claude Code, Codex and other CLIs. Cancel any time.

An AI assistant for research: funder rules, compliance, methods and publishing. It links the sources behind its answers and labels answers that come from a web search.

Answers draw on CASRAI's guides and dictionary plus the federal and funder documents we index: Federal Register, Grants.gov, Regulations.gov and UKRI.
