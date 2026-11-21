---
id: collect-261121-veille-ia/ia-agents/healthbench-professional-benchmark-artificial-analysis
title: "HealthBench Professional Benchmark | Artificial Analysis"
domain: ia-agents
role: reference
task: article
actors: []
dates: []
keywords: ["healthbench", "professional", "benchmark", "task", "analysis", "score", "artificial", "work"]
source: https://artificialanalysis.ai/evaluations/healthbench
source_anchor: ""
source_lines: [1, 125]
section: "IA_Agents"
---
# HealthBench Professional Benchmark

#### HealthBench Professional

#### Cost per Task

#### Output Tokens Used

## Score

### HealthBench Professional: Length-Adjusted Score

### HealthBench Professional: Length-Adjusted Score vs. Cost per Task

### HealthBench Professional: Score vs. Answer Length

### HealthBench Professional: Score by Use Case, Type and Difficulty

## Token Usage

### HealthBench Professional: Output Tokens per Task

## Cost

### HealthBench Professional: Cost per Task

## Speed

### HealthBench Professional: Time per Task

## Score vs. Release Date

### HealthBench Professional: Score vs. Release Date

## Frequently Asked Questions

HealthBench Professional is OpenAI's benchmark of 525 conversations with clinicians, each graded against a rubric written by physicians. It covers consult, writing and research use cases.

HealthBench Professional has 525 conversations with clinicians rather than 5,000 general health conversations. They split into consult, writing and research use cases, and include red-teaming and difficult conversations. We grade it with GPT-5.4 (low) and run 4 repeats per task, reporting the mean across them; we run HealthBench once.

The length-adjusted score subtracts 1.47 percentage points for every 500 characters above 2,000 and adds the same amount for every 500 characters below, before averaging. This is the value OpenAI reports. Reasoning does not count toward length.

A task that still fails after retries scores 0 in the raw score, the length-adjusted score and every breakdown. It does not count toward the mean answer length.

## Explore Evaluations

A composite benchmark aggregating nine challenging evaluations to provide a holistic measure of AI capabilities across knowledge work, coding, science, and general knowledge.

A private evaluation developed by Artificial Analysis for frontier agentic capability in long-horizon knowledge work, testing agents on realistic business workflows that require deliverables such as spreadsheets, presentations, and memos.

A benchmark developed by Artificial Analysis for how well AI models build interactive 3D experiences. Models work as agents in an offline sandbox via Stirrup to turn a written brief into a three.js scene, with Elo ratings derived from blind pairwise votes.

Artificial Analysis' implementation of the APEX-Agents benchmark, testing AI agents on long-horizon, cross-application tasks in professional-services environments with realistic application tooling.

Artificial Analysis' data analysis benchmark, testing AI agents on their ability to work with spreadsheets and documents to answer quantitative questions a Business Analyst or Data Analyst would face day-to-day.

A benchmark measuring agentic task completion across simulated SaaS application environments, scoring the share of each task's objectives completed without guardrail violations.

Artificial Analysis' implementation of Harvey's Legal Agent Benchmark (LAB), testing AI agents on real-world legal work from Harvey's dataset of 120 private tasks spanning 24 legal practice areas. v1.1, built in collaboration with Harvey, checks every deliverable against the task's source documents for hallucinations, so work that passes every rubric criterion but contains a material hallucination receives no credit. A panel of three judges grades each deliverable criterion-by-criterion.

Artificial Analysis' implementation of Surge AI's GDP.pdf benchmark, testing whether language models can reason over long, real-world professional documents and satisfy detailed task-specific criteria.

Artificial Analysis' independent implementation of ServiceNow's EnterpriseOps-Gym, an agentic benchmark testing whether LLM agents can complete stateful, multi-step enterprise workflows across eight business domains via live tool use, graded on the final state of the underlying databases.

A harder 66-task benchmark of complex terminal work across software, machine learning, science, operations, security, hardware, and media, with recalibrated compute and time allowances and improved instructions, environments, and verifiers.

A 70-task benchmark of research workflows authored and reviewed by domain experts across the life, physical, earth, mathematical, and engineering sciences, each completed in a terminal and checked by its own set of tests.

Artificial Analysis' implementation of Cognition's FrontierCode 2.0, a private agentic coding benchmark of 132 tasks in real open-source repositories, graded on whether a maintainer would merge the agent's change.

A refined and updated 1,000-question set of Humanity's Last Exam, split evenly between reasoning and knowledge questions, with 272 questions that include an image.

Measures how models communicate rather than whether they are correct: 30 judged personality traits rolled up into 8 indexes, plus 21 measured communication characteristics.

A benchmark measuring factual recall and hallucination across various economically relevant domains.

The most challenging 198 questions from GPQA, where PhD experts achieve 65% accuracy but skilled non-experts only reach 34% despite web access.

Artificial Analysis' implementation of IBM's ITBench benchmark, testing AI agents on Kubernetes incident root-cause analysis from offline incident snapshots. The agent inspects alerts, events, traces, and topology and identifies the contributing-factor entities (deployments, pods, namespaces, network policies, etc.) responsible for the failure.

An enhanced MMMU benchmark that eliminates shortcuts and guessing strategies to more rigorously test multimodal models across 30 academic disciplines.

A benchmark evaluating precise instruction-following generalization on 58 diverse, verifiable out-of-domain constraints that test models' ability to follow specific output requirements.

An open benchmark from Wisedocs measuring how well models reason over long, fragmented medical records, performing the multi-document synthesis claims professionals rely on when reviewing insurance and healthcare cases.

Artificial Analysis' implementation of HeyGen's Code2Video Bench, evaluating how well models create short product and brand videos from 114 design briefs using HyperFrames HTML.

OpenAI's HealthBench: 5,000 health conversations graded against physician-written rubrics, from emergency referrals to global health.

CritPt v2 is the revised version of CritPt, a benchmark designed to test LLMs on research-level physics reasoning tasks, featuring 70 composite research challenges. Models solve problems with a code-execution tool in an offline Linux sandbox.

VIALS-AA is Artificial Analysis' implementation of Handshake AI's VIALS benchmark. It tests whether language models can interpret scientific images and answer questions about the evidence they contain.

Artificial Analysis' implementation of Google DeepMind's Multimodal Knowledge Agent (MKA) benchmark. It tests whether agents can read visually dense project files, such as architectural drawings, engineering schematics and clinical scans, and produce the deliverables a professional would.

GDPval-AA v2.1 is Artificial Analysis' evaluation framework for OpenAI's GDPval dataset. It tests AI models on real-world tasks across 44 occupations and 9 major industries. Models are given shell access and web browsing capabilities in an agentic loop via Stirrup to solve tasks, with Elo ratings derived from blind pairwise comparisons.

A fintech customer-support benchmark from the 𝜏-Knowledge framework that tests whether agents can navigate a large unstructured knowledge base and execute multi-step tool calls to resolve realistic banking workflows.

A verified refresh of Terminal-Bench 2.0: 89 curated tasks across software engineering, system administration, data processing, model training, and security, with environment and instruction fixes so scores reflect agent capability rather than environment gaps.

Artificial Analysis' implementation of Cognition's FrontierCode 1.1, a private agentic coding benchmark that tests models on completing coding tasks in a way that would actually get merged by maintainers of real-world repositories.

A challenging benchmark measuring language models' ability to extract, reason about, and synthesize information from long-form documents ranging from 10k to 100k tokens (measured using the cl100k_base tokenizer).

A scientist-curated coding benchmark featuring 288 test set subproblems from 80 laboratory problems across 16 scientific disciplines.

A frontier-level benchmark with 2,500 expert-vetted questions across mathematics, sciences, and humanities, designed to be the final closed-ended academic evaluation.

A benchmark designed to test LLMs on research-level physics reasoning tasks, featuring 71 composite research challenges.

An agentic benchmark evaluating AI capabilities in terminal environments through software engineering, system administration, and data processing tasks.

A dual-control conversational AI benchmark simulating technical support scenarios where both agent and user must coordinate actions to resolve telecom service issues.

An enhanced version of MMLU with 12,000 graduate-level questions across 14 subject areas, featuring ten answer options and deeper reasoning requirements.

A contamination-free coding benchmark that continuously harvests fresh competitive programming problems from LeetCode, AtCoder, and CodeForces, evaluating code generation, self-repair, and execution.

A 500-problem subset from the MATH dataset, featuring competition-level mathematics across six domains including algebra, geometry, and number theory.

All 30 problems from the 2025 American Invitational Mathematics Examination, testing olympiad-level mathematical reasoning with integer answers from 000-999.

A lightweight, multilingual version of MMLU, designed to evaluate knowledge and reasoning skills across a diverse range of languages and cultural contexts.
