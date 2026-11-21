---
id: collect-261121-veille-ia/ia-agents/gdp-pdf-benchmark-artificial-analysis
title: "GDP.pdf Benchmark | Artificial Analysis"
domain: ia-agents
role: reference
task: article
actors: []
dates: []
keywords: ["tnxb", "cleds", "joint", "task", "mutations", "gene", "benchmark", "model"]
source: https://artificialanalysis.ai/evaluations/gdp-pdf
source_anchor: ""
source_lines: [1, 186]
section: "IA_Agents"
---
# GDP.pdf Benchmark

GDP.pdf: A Benchmark for Professional Document Reasoning

#### GDP.pdf All-pass

#### Cost per Task

#### Output Tokens Used

## Score

### GDP.pdf: All-pass Rate

### GDP.pdf: All-pass Rate vs. Cost per Task

## Domain Results

### GDP.pdf: Mean Pass by Domain

## Cost

Costs include contestant model calls only. Judge calls and local PDF preparation and OCR are excluded.

### GDP.pdf: Cost per Task

## Token Usage

### GDP.pdf: Output Tokens per Task

## Speed

Time per task estimates contestant response-generation time from output-token usage and model output speed. It excludes judge calls and local PDF preparation and OCR.

### GDP.pdf: Time per Task

Explore five representative GDP.pdf tasks, their source documents, reviewed model submissions, and criterion-level judge rationales.

Instructions

Submission result

One reviewed model submission

Model submission

Gemini 3.5 Flash

Based on the provided research review, mutations in the ***TNXB* gene** (which encodes the tenascin-X protein) are associated with the following Ehlers-Danlos syndrome (EDS) subtypes and clinical presentations:

### **1. Associated EDS Subtypes**

- **Classical-like EDS (clEDS):** Caused by autosomal recessive, biallelic mutations (homozygous or compound heterozygous) in*TNXB* , leading to a complete deficiency of tenascin-X (TNX).
- **Contiguous Gene Syndrome (CAH-X):** Caused by a contiguous gene deletion or mutation overlapping both the*TNXB* gene and the adjacent*CYP21A2* (previously*CYP21B* ) gene.
- ***TNXB* Haploinsufficiency:** Associated with heterozygous state mutations in*TNXB* that result in reduced (but not completely absent) serum TNX levels, which clinically overlaps with hypermobile EDS (hEDS).

### **2. Differentiating Characteristics**

#### **Classical-like EDS (clEDS)**

- **Primary Features:** Marked by generalized joint hypermobility (GJH), soft/velvety and hyperextensible skin, and easy bruising.
- **Key Differentiating Characteristics:**  - Unlike classical EDS, individuals with clEDS **lack the typical atrophic (papyraceous) scarring** .
  - It follows an **autosomal recessive** pattern of inheritance, whereas classical EDS is typically autosomal dominant.
- Unlike classical EDS, individuals with clEDS 

#### **CAH-X (Contiguous Gene Syndrome)**

- **Primary Features:** Combines the connective tissue features of EDS with the endocrine features of**congenital adrenal hyperplasia (CAH)** due to 21-hydroxylase deficiency.
- **Key Differentiating Characteristics:**  - Patients with CAH-X exhibit significantly higher frequencies of joint hypermobility, chronic joint pain, joint dislocations, and piezogenic papules on the feet compared to patients with CAH alone.
  - They are also predisposed to structural cardiac valve abnormalities.

#### ***TNXB* Haploinsufficiency**

*TNXB*Haploinsufficiency

- **Primary Features:** Present in heterozygous carriers (such as obligate family members of clEDS patients) who have roughly half of the normal level of serum TNX.
- **Key Differentiating Characteristics:**  - Individuals present with joint hypermobility, chronic joint pain, and joint subluxations.
  - Unlike those with complete tenascin-X deficiency (clEDS), **skin hyperextensibility and easy bruising are generally absent** in these heterozygous individuals.

Criterion verdicts

Independent judge results and concise rationales

The answer correctly identifies classical-like EDS (clEDS) as caused by biallelic TNXB mutations leading to complete tenascin-X deficiency.

The answer correctly states that classical-like EDS results from biallelic TNXB mutations leading to a complete deficiency of tenascin-X (TNX) protein.

The answer correctly identifies TNX-deficient clEDS as caused by autosomal-recessive biallelic TNXB variants, explicitly including homozygous and compound heterozygous mutations. This satisfies the stated genetic-basis criterion.

The answer identifies generalized joint hypermobility as a primary feature of classical-like EDS (clEDS), which satisfies the criterion's required common feature.

The response explicitly lists soft/velvety skin as a primary feature of classical-like EDS (clEDS), satisfying the criterion.

The answer lists hyperextensible skin as a primary feature of classical-like EDS (clEDS), which satisfies the criterion’s required common feature.

The response explicitly lists easy bruising as a primary feature of classical-like EDS (clEDS), satisfying the criterion for a common cEDS feature.

The answer explicitly identifies TNXB haploinsufficiency as associated with heterozygous TNXB variants and states that it clinically overlaps with hypermobile EDS (hEDS), including hEDS-like features such as joint hypermobility, chronic pain, and subluxations.

The answer explicitly states that TNX-deficient classical-like EDS lacks the typical atrophic (papyraceous) scarring seen in classical EDS, directly satisfying the criterion.

The answer explicitly states that TNX-deficient classical-like EDS (clEDS) is caused by biallelic mutations and follows an autosomal recessive inheritance pattern.

The answer explicitly lists “Contiguous Gene Syndrome (CAH-X)” as a separate associated EDS subtype, contrary to the criterion requiring that CAH-X not be treated as a distinct EDS subtype.

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
