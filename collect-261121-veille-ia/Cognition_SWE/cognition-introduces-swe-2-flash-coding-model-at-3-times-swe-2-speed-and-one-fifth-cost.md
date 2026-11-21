---
id: collect-261121-veille-ia/cognition-swe/cognition-introduces-swe-2-flash-coding-model-at-3-times-swe-2-speed-and-one-fifth-cost
title: "Cognition introduces SWE-2 Flash coding model at 3 times SWE-2 speed and one-fifth cost"
domain: cognition-swe
role: reference
task: article
actors: ["Cognition"]
dates: ["2026-11-04"]
keywords: ["swe-2", "flash", "cognition", "devin", "times", "coding", "inkling-small", "terminal-bench"]
source: https://ashutoshai.com/ai-compass/2026/11/04/cognition-swe-2-flash/
source_anchor: ""
source_lines: [1, 46]
section: "Cognition_SWE"
---
Cognition introduced SWE-2 Flash on 4 November 2026, a fast software-engineering model built on Thinking Machinesâ sparse mixture-of-experts base model Inkling-Small (276 billion total parameters, 12 billion active per token). Designed for interactive repository navigation, rapid edits and parallel background subagents, SWE-2 Flash streams roughly 200 tokens per second in Cognitionâs Terminal-Bench replay, running 3 times faster than flagship SWE-2 at one-fifth its price.

To lift a 12B-active checkpoint to agentic coding reliability, Cognition added a three-stage mid-training curriculum spanning a few trillion tokens before running audited multi-domain reinforcement learning across shell, code-editing and verification environments. Cognition selected mid-training mixtures by tracking token-efficiency loss ratios rather than raw validation perplexity alone, steering capacity toward multi-step tool trajectories.

Cognitionâs interactive score charts are rendered in HTML on its blog rather than as downloadable images: mid-training and RL raise Terminal-Bench 4.0 from 0.9% on Inkling-Small to 20.0% on SWE-2 Flash (ahead of 4.0% for Beam 1.0 and 8.0% for Muse Spark Open, versus 27.3% for full SWE-2), FrontierCode 2.0 from 3.2% to 24.0% and ExploitBench from 11.3% to 34.8%. SWE-2 Flash is free for all users in Devin Desktop and Devin CLI through 30 November 2026; citing its 34.8% ExploitBench score, Cognition withheld open weights and is establishing a vetted access programme for external security researchers and partners without publishing standalone token rates.

## Technical details

- Base architecture
- Sparse MoE post-trained from Thinking Machines Inkling-Small (276B total, 12B active per token)
- Speed and relative cost
- Roughly 200 tok/s (versus 60 tok/s on 2.8T-A104B SWE-2) at 3 times speed and one-fifth cost
- Coding benchmarks
- 89.9 SWE-Bench Pro v2 Hard, 89.2 Terminal-Bench 2.1, 60.1 DeepSWE 1.1 and 24.0 FrontierCode 2.0
- Security evaluation
- 34.8% on ExploitBench (up from 11.3% on Inkling-Small; versus 36.1 on SWE-2)
- Access and promo window
- Free inside Devin Desktop and Devin CLI through 30 November 2026; weights gated for vetted partners

## Release details & context

### Full benchmark breakdown and Devin Desktop workflow

Beyond the headline Terminal-Bench 4.0 jump in Cognitionâs SWE-2 Flash technical post, Cognitionâs full evaluation table discloses flagship SWE-2 at 2.8T total and 104B active parameters (10.1 times larger than 276B-A12B SWE-2 Flash) streaming at 60 tokens per second on Terminal-Bench 4.0âs gsea-proteomics task versus 200 tokens per second on Flash. Across additional coding suites, SWE-2 Flash reaches 89.9 on SWE-Bench Pro v2 Hard (versus 56.5 on Inkling-Small, 76.5 on Reflection Beam 1.0 and 82.4 on Meta Muse Spark Open), 89.2 on Terminal-Bench 2.1, 60.1 on DeepSWE 1.1, 50.0 on SWE-Atlas QA and 45.5 on FrontierCode 1.1 Main.

Inside Devin Desktop (download portal), engineers manage local and cloud agents over the Agent Client Protocol (ACP) and pair frontier planners with fast execution models through Devin Fusion while Devin Review 2.0 handles pull-request diffs.

## Common questions

### Why did Cognition hold back public open weights for SWE-2 Flash?

Targeted cybersecurity training lifted SWE-2 Flash from 11.3% to 34.8% on ExploitBench, prompting Cognition to build a vetted weight-sharing programme for trusted security researchers and enterprise on-premises deployments.

### How does SWE-2 Flash compare in parameter count to Cognitionâs flagship SWE-2?

SWE-2 Flash activates 12B of 276B total parameters per token, roughly one-tenth the size of flagship SWE-2 at 2.8T total and 104B active parameters.

## Editorâs weekly perspective

**Coding agents and developer tools**: Post-trained on Thinking Machinesâ 12B-active Inkling-Small base, running 3 times faster than SWE-2 at one-fifth the cost.

## Cognition release timeline (10 launches tracked)

1. Cognition upgrades Devin Security with Claude Mythos deep scansCognition upgraded Devin Security with Claude Mythos deep vulnerability scans and hundreds of millions of dollars in Devin remediation credits.
2. Cognition releases Devin Review 2.0 with fivefold cost reduction and faster pull request reviewsCognition released Devin Review 2.0 for automated pull request reviews, running up to 5 times cheaper and 1.5 times faster than its predecessor.
3. Cognition upgrades Devin Teams with free SWE-2 sessions and SSOCognition upgrades Devin Teams with included Devin Desktop seats, two concurrent free SWE-2 sessions per user, SAML/OIDC SSO and a standard HIPAA BAA.
