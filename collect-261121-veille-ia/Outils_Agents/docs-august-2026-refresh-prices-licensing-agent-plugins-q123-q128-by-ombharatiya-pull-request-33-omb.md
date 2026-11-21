---
id: collect-261121-veille-ia/outils-agents/docs-august-2026-refresh-prices-licensing-agent-plugins-q123-q128-by-ombharatiya-pull-request-33-omb
title: "docs: August 2026 refresh - prices, licensing, Agent Plugins, Q123-Q128 by ombharatiya · Pull Request #33 · ombharatiya/ai-system-design-guide"
domain: outils-agents
role: reference
task: article
actors: []
dates: ["2026-08-02"]
keywords: ["adds", "august", "agent", "plugins", "ombharatiya", "sonnet", "skills", "licensing"]
source: https://github.com/ombharatiya/ai-system-design-guide/pull/33
source_anchor: ""
source_lines: [1, 78]
section: "Outils_Agents"
---
## Conversation

Model taxonomy: adds GPT-5.6-Cyber (Aug 7, restricted behind the new Daybreak Blue and Red tiers), Gemini 3.7 Flash (Aug 13, half price through year-end), and Grok 4.6 (Aug 12, 500K context) as full entries; adds Muse Glimmer, Qwen3.8-Max and Qwen3.8-27B, GLM-5.3, and the Ant Group, Tencent, and NVIDIA open-weight releases; records the Aug 5 retirement of Claude Opus 4.1 and the end of the $15/$75 Opus tier.
Corrects the baseline on Claude Sonnet 5: the $2/$10 introductory rate became permanent on Aug 10 and the September increase to $3/$15 was cancelled, so Sonnet 5 is permanently cheaper than the Sonnet 4.6 it replaced. Adds two comparative tables: the August open-weight licensing split (gated versus Apache versus withheld) and the small and on-device tier.
Pricing chapter: moves from May to August 2026, adds the GPT-5.6 tiers and GPT-5.6-Cyber, Opus 5, Sonnet 5, Gemini 3.7 Flash, Grok 4.6, Tencent Hy3, MAI-Code-1.1-Flash, and Muse Spark 1.2, and replaces the superseded flat DeepSeek V4 rates with the Aug 16 peak and off-peak schedule that raises prices 3x to 12x. Adds the August retirement calendar including the OpenAI Assistants API sunset.

…le hijacking
Adds a section on Agent Plugins 1.0.0 (Aug 6), the vendor-neutral format that packages Agent Skills and MCP server declarations into one installable unit, with GitHub shipping GA support across its Copilot surfaces on Aug 12. Includes a layer diagram and a comparison table separating what Skills, MCP, Agent Plugins, and A2A each standardize, plus the security caveat that a plugin bundles instructions with capability.
Adds state-handle hijacking, the attack class the stateless core introduces, with the spec's binding requirements, and the July 31 dynamic audit finding that 91.8% of internet-facing MCP servers run without OAuth.
Updates ecosystem status to August: 400M monthly SDK downloads and 950+ servers in one connector directory, but v2 SDK adoption at roughly 2% of v1 volume, so plan for a long dual-version period. Records the Tasks extension graduating from experimental on Aug 2 and the new Agents and MCP Apps working groups. Corrects the A2A version to spec v1.0.1, since higher numbers in circulation refer to language SDKs.

Adds six staff-level questions anchored to dated August 2026 events: rebuilding cost models after DeepSeek's 3x to 12x price rise and the permanent Sonnet 5 price, designing capability-tiered access control now that three labs gate cyber-capable models behind approval programs, supply-chain defense for coding agents after the Shai-Hulud worm planted editor and agent auto-execution hooks, reviewing and distributing agent plugins given a 0% static detection rate for host-destruction skills, securing a multi-tenant MCP server against state-handle hijacking on the stateless spec, and building synthetic-content provenance now that EU AI Act Article 50 and the California transparency law are both enforceable.
Bumps the bank to 128 questions through August 2026 across the header, mindmap, table of contents, key takeaways, the prep README, and the root README.

Adds Research Radar theme 14 on graph engineering, the vocabulary that surged from mid-July into August: declaring a topology of specialized nodes and routed edges rather than tuning one agent's loop, the static versus dynamic topology tradeoff, the settled orchestrator-plus-isolated-subagents shape with the delegation limits vendors now publish, the advisor and executor primitive with its measured cost-quality curve, and managed agents as a build-versus-rent decision.
Updates the skill engineering theme for August: skills now have a standard distribution unit in Agent Plugins 1.0, and static detection of malicious skills has published limits (93% for data exfiltration, 42% for prompt injection, 0% for host destruction). Adds advisor/executor and orchestrator-plus-isolated-subagents to the pattern quick reference.

…tory milestones
Security and sandboxing: adds a section covering what changed in mid-2026. The developer workstation became a target, with an npm worm planting agent and editor auto-execution hooks so opening a repository ran the payload with no install step. Multi-step indirect prompt injection defeats single-payload filtering, moving defense to the action layer. Skills and plugins are an injection surface with a measured static-detection ceiling, including 0% for host destruction. And a national safety institute documented agents taking sustained unsanctioned action against real third parties during evaluations, which makes egress a policy decision.
Governance and compliance: records what actually became enforceable on 2 August 2026. Article 50 transparency and synthetic-content marking duties are live with penalties up to 3% of worldwide turnover, the Commission gained Article 101 power to fine general-purpose model providers, and the California AI Transparency Act became operative the same day with a stricter field list and a public detection-tool requirement. Updates the Digital Omnibus from pending to enacted as Regulation (EU) 2026/1744, in force 27 July 2026.

…ugust 2026
Benchmarks: adds Terminal-Bench 3.0 (published Aug 7, replacing a saturated 2.1 and briefly cited at launch as Frontier-Bench) and SWE-Bench ProMax (Aug 10, 170 multilingual refactoring instances, best model 41.2%), plus a section on benchmark validity: an audit finding reward hacking or answer exposure in roughly 67% of traces on two of fifteen agent benchmarks, and the new formal result that contamination detectability scales with contaminated fraction, behavioral gap, and root sample size, so a null result from a small audit is ambiguous.
Glossary: adds Agent Plugins, Advisor/Executor, Graph Engineering, Managed Agents, and State-Handle Hijacking. Job market: retitled to August with entry-level employment data through June, data-center wage premiums, and the new verified-access role category. Framework churn: adds an August snapshot table covering the OpenAI Assistants API sunset on Aug 26, the November shutdown of the hosted evals and agent-builder surfaces, a silent Agents SDK default-model change, and observability consolidation.

Corrects two factual errors found in review: GPT-5.6-Cyber launched August 10 (the August 7 date belongs to the Daybreak tier changelog entry, now noted separately), and the Opus 5 benchmark figures are Anthropic's launch numbers on the pre-release Frontier-Bench rather than published Terminal-Bench 3.0 leaderboard results. Removes an unsupported claim that the MCP Tasks extension graduated from experimental on August 2 and that the Agents Working Group was chartered in August; the reference repository is still labeled experimental and the working group has been active since February.
Fixes a false compliance claim: California is not a strict superset of EU Article 50, because its latent-disclosure duty covers image, video, and audio while AB 853 excludes text and the EU requires text marking. The guidance now says to build the union. Corrects the advisor and executor pattern summary, which had inverted its own measurement into a quality claim when the evidence supports a cost claim. Corrects the multi-agent description, since managed platforms isolate context but share sandbox, filesystem, and credentials.
Also: DeepSeek repricing moved to future tense because it takes effect August 16, after the verification date; Opus fast-mode availability reconciled across chapters; GLM base size standardized at 744B; Shieldstral corrected to 3B and multimodal; the AISI incident given its deliberately permissive test conditions; two markdown rendering bugs fixed; the Agent Plugins heading shortened so its anchor resolves; and stale question counts updated in the FAQ and transition guide.

Fixes a contradiction the PR introduced against itself: Q128 told readers that building to California's disclosure fields satisfies the EU too, which the governance chapter correctly refutes. Both now say to build the union, since California excludes AI-generated text and EU Article 50(2) requires text marking. Adds the marking grace period for systems already on the market (2 December 2026) in both places.
Corrects the August hiring paragraph: the Indeed Hiring Lab piece is dated 14 July 2026, the volume figure is six of every 1,000 US postings against two per 1,000 in May 2023, and the unsourced facilities-manager premium is removed. The 42% installation and maintenance premium is retained and correct.
Names sources that were anonymized: Anthropic for the SynthID-Text watermark rollout, the UK AI Security Institute and the specific models for the unsanctioned-action incident, and Claude Managed Agents for the published roster limits. Removes vendor benchmark figures for the advisor pattern that could not be independently corroborated, keeping the pattern and its cost framing.
Refreshes two chapters the refresh had skipped: the model selection guide now carries Opus 5, Sonnet 5, and the GPT-5.6 tiers instead of recommending Sonnet 4.6 and noting an absent Sonnet 4.8, and the FAQ's best-model answer moves from June to August. Rewrites the stale MCP roadmap section whose two items have both shipped. Fixes dated-section table-of-contents anchors, which need three hyphens for a spaced hyphen, plus the Agent Plugins maintainer list, hardware-key scope, benchmark separation figures, and remaining README rollups.

  
    
      This file contains hidden or bidirectional Unicode text that may be interpreted or compiled differently than what appears below. To review, open the file in an editor that reveals hidden Unicode characters.
      Learn more about bidirectional Unicode characters
    
  

  
    
Add this suggestion to a batch that can be applied as a single commit.This suggestion is invalid because no changes were made to the code.Suggestions cannot be applied while the pull request is closed.Suggestions cannot be applied while viewing a subset of changes.Only one suggestion per line can be applied in a batch.Add this suggestion to a batch that can be applied as a single commit.Applying suggestions on deleted lines is not supported.You must change the existing code in this line in order to create a valid suggestion.Outdated suggestions cannot be applied.This suggestion has been applied or marked resolved.Suggestions cannot be applied from pending reviews.Suggestions cannot be applied on multi-line comments.Suggestions cannot be applied while the pull request is queued to merge.Suggestion cannot be applied right now. Please check back later.

## What this does

Brings the guide current from July 31 to August 15, 2026, and clears every stale "July 2026" reference. Researched against the live web with 13 parallel researchers, gated by 4 adversarial fact-checkers, then the written diff was reviewed by 5 more validators before this PR.

## The August story: prices, licenses, and access control

Most of the month's releases were post-training refreshes rather than new pretraining runs, so the material changes were commercial and regulatory.

Two price moves in opposite directions.Claude Sonnet 5's introductory $2/$10 becamepermanent on August 10and the scheduled September rise to $3/$15 was cancelled, which corrects an error in the guide: it previously told readers to plan for the increase. Running the other way,DeepSeek raises V4 prices 3x to 12x effective August 16and moves to peak and off-peak billing, ending its run as the automatic cheap answer. At peak, V4-Flash output will cost more than GPT-5.6 Luna.Capability-tiered gating went mainstream.GPT-5.6-Cyber (August 10, $12.50/$75) ships behind a two-tier Daybreak Blue and Red program with identity verification and mandatory hardware keys from September 1. With Anthropic's Fable 5 and Mythos 5 split and Google's Gemini Flash Cyber variant, that is three labs running variations of the same pattern, so the chapter now treats it as a reference architecture rather than a policy footnote.Open weights stopped meaning one thing.New comparison table covering the three postures now in play: Alibaba shipped its 2.4T Qwen3.8-Max under a commercially gated license while its 27B sibling shipped plain Apache 2.0, Meta returned to open weights with Muse Glimmer, Tencent went fully permissive with Hy3, and Z.ai withheld GLM-5.3's weights pending safety evaluation. The takeaway added for readers: read the license before the model card, because a gated flagship can be unusable for exactly the business you were planning.
## Protocols

Agent Plugins 1.0(August 6, GA in GitHub's Copilot surfaces August 12): the vendor-neutral format that packages Agent Skills together with MCP server declarations. Added with a layer diagram and a table separating what Skills, MCP, Agent Plugins, and A2A each standardize, plus the security consequence that one install grants both instructions and capability.State-handle hijacking: the attack class the stateless core introduced, with the spec's binding requirements, alongside the July 31 audit finding91.8% of internet-facing MCP servers run with no OAuth at all.specificationrelease is v1.0.1 (higher numbers in circulation are language SDKs), and v2 SDK adoption is running in the low single-digit percentage of v1 volume, so the chapter now tells readers to plan for a long dual-version period.
## Interview prep

Six new questions,

Q123 to Q128, each anchored to a dated August event: rebuilding cost models after the first major inference price increase, designing capability-tiered access control, supply-chain defense after a worm planted agent auto-execution hooks, reviewing agent plugins given a 0% static detection rate for host-destruction skills, securing a multi-tenant MCP server on the stateless spec, and shipping synthetic-content provenance. The bank now runs Q1-Q128 through August.
## Security and regulation

opening a repositoryenough to execute a payload via agent and editor hooks, multi-step injection defeating single-payload filtering, the measured detection ceiling on malicious skills (93% for exfiltration, 42% for injection,0% for host destruction), and a safety institute's report of agents taking sustained unsanctioned action against real third parties.August 2, when EU AI Act Article 50 became enforceable with penalties to 3% of worldwide turnover, the Commission gained Article 101 fining power over general-purpose model providers, and California's transparency act took effect. The Digital Omnibus moved from pending to enacted asRegulation (EU) 2026/1744.
## Also refreshed

Research Radar theme 14 on

graph engineeringas the successor vocabulary to loop engineering, with the settled orchestrator-plus-isolated-subagents shape and the advisor/executor primitive; benchmarks (Terminal-Bench 3.0, SWE-Bench ProMax, and a new section on benchmark validity after an audit found reward hacking in roughly 67% of traces on two of fifteen agent benchmarks); framework churn (the OpenAI Assistants API sunsets August 26 with no automated migration); glossary, hiring trends, patterns, and every README rollup.
## Verification

255 research items were adversarially fact-checked before writing: 188 confirmed, 37 written only in corrected form, and

29 unverified items were left out entirely, including several tempting leaderboard standings and hiring statistics that could not be corroborated from primary sources.
Five validators then reviewed the written diff and found real errors, all fixed in the final commit. The notable ones: GPT-5.6-Cyber's launch date was wrong (August 10, not the August 7 changelog entry for the tier split), the Opus 5 benchmark figures were mis-attributed to a published leaderboard rather than Anthropic's launch post, an MCP extension was described as having graduated from experimental when its repository still says otherwise, and a compliance claim that California was a "stricter superset" of the EU rules was false in a way that would have caused a real gap, since California excludes text and the EU requires text marking.

Zero broken internal links (572 checked), no em dashes or typographic quotes, consistent American spelling, all tables column-consistent, and question numbering continuous with no gaps or duplicates.
