---
id: collect-261121-veille-ia/kimi/kimi-k3-1-pro-takes-on-claude-opus-5-5-and-gpt-6-astra-at-much-lower-cost
title: "Kimi K3.1 Pro Takes on Claude Opus 5.5 and GPT-6 Astra at Much Lower Cost"
domain: kimi
role: reference
task: article
actors: ["Moonshot AI"]
dates: ["2026-10-24"]
keywords: ["claude", "opus", "kimi", "gpt-6", "astra", "model", "moonshot", "behind"]
source: https://propakistani.pk/2026/10/24/kimi-k3-1-pro-takes-on-claude-opus-5-5-and-gpt-6-astra-at-much-lower-cost/
source_anchor: ""
source_lines: [1, 81]
section: "Kimi"
---
Moonshot AI has launched Kimi K3.1 Pro, a new open-weight model that it says matches or comes close to the world’s strongest proprietary AI models in coding, agentic work, and knowledge tasks. In the company’s own benchmarks, K3.1 Pro scores 65.7% on Terminal-Bench 4.0, ahead of OpenAI’s GPT-6 Astra (58.2%) and just behind Anthropic’s Claude Opus 5.5 (66.4%).

The Chinese AI company announced that K3.1 Pro is the first model in its new K3.1 series and its most capable 3T-class open-weight model so far. It has a 1-million-token context window and brings major upgrades in coding, 3D creation, financial research, and other agent tasks. It is also cheaper to use than its predecessor through the API.

Moonshot also released Kimi K3.1 Pro Highspeed, a faster version that runs two to three times quicker than the standard model. Both are available now on Kimi.ai, Kimi Work, Kimi Code, and the Kimi API. The full model weights will be released by November 4.

## Same Giant Brain, Far More Training

K3.1 Pro is not a brand-new architecture. It is a post-trained version of Kimi K3, the 2.8-trillion-parameter model Moonshot launched in July, but with much more post-training compute behind it.

Moonshot says it ran large-scale reinforcement learning across hundreds of domains at once. To make this work, it optimized its RL framework, long-context inference, training efficiency, sandbox infrastructure, and rollout scheduling. According to the company, this produced consistent gains in every domain it tested.

## Where It Beats Claude and GPT

Moonshot compared K3.1 Pro with Claude Opus 5.5, Claude Fable 5.1, GPT-6 Astra, DeepSeek-V4.1-Flash-1017, and GLM-5.3. All results are self-reported, with K3.1 Pro running at maximum reasoning effort.

K3.1 Pro came out on top in several tests. It leads SWE-Marathon with 51.7, ahead of Claude Opus 5.5 at 46.3, and Humanity’s Last Exam Full with tools at 70.0, compared with 67.7 for Opus 5.5. It also tops OfficeQA Pro v2 (64.4), Agents’ Last Exam (43.2), GDP.pdf (34.6), and ZeroBench with tools (63.0). Moonshot also reports leads on BrowseComp (92.1), AutomationBench-AA (73.3), and DeepSWE v1.1 among the proprietary models.

It also edged past GPT-6 Astra on the GDPval-AA v2.1 work-task ranking with an Elo score of 1,846, while staying just behind Claude Opus 5.5 at 1,866.

## Where It Still Falls Behind

The model is not ahead everywhere. Its weakest result is on Terminal-Bench Science 0.1, where it scores 38.1, far behind GPT-6 Astra (68.1) and Claude Opus 5.5 (63.3).

It also trails the top proprietary models on ProgramBench (31.5 vs 65.0 for Opus 5.5), HLE-Diamond (39.4 vs 65.8 for GPT-6 Astra), and GPQA-Diamond (94.2 vs 96.1 for GPT-6 Astra). On cybersecurity tests such as ExploitBench and ExploitGym, it remains well behind Claude and GPT, which Moonshot says is intentional.

Against other open models, however, K3.1 Pro leads clearly. On Terminal-Bench 4.0, DeepSeek-V4.1-Flash-1017 scores 42.3 and GLM-5.3 scores 41.8, more than 20 points behind Kimi.

## Builds Games and Fixes Own Mistakes

Moonshot says K3.1 Pro can handle everything from quick fixes in real codebases to full project builds. It works with game engines like Unity, Unreal Engine, and Godot, and combines them with web search and 3D generation to create complete experiences.

In one internal test, the model built a human walking simulator on top of an open-source anatomical model with 296 skeletal and 402 muscle components. Across 167 missions, it took the simulated human from collapsing within seconds to walking continuously for 11.2 seconds. When its approach stopped working, it found and fixed 36 errors in the simulator’s shoulder-muscle calculations before retraining.

## It Now Builds 3D Models Too

K3.1 Pro scores 92.7% on BenchCAD with tools, a test of rebuilding 3D objects from images using CAD code. That is close to Claude Opus 5.5 (96.2%) and GPT-6 Astra (95.9%). Without tools, it scores 72.8, nearly level with Opus 5.5 at 73.0.


The model can turn a text brief, sketch, or reference image into a usable 3D model. It can also write and run scripts for tools like Blender and CAD software without needing MCP connectors. Moonshot has launched a new 3D Make feature on Kimi.ai and the Kimi app, letting users preview, edit, and download 3D models for printing.

## Big Jump in Finance and Accuracy

Moonshot says K3.1 Pro can now deliver complete financial workflows from start to finish. It scores 58.6 on Frontier Finance, ahead of Opus 5.5 (56.7) and GPT-6 Astra (45.0).

The company also focused on factual accuracy. On its internal Kimi Hallu Bench Hard test, the claim-level hallucination rate fell from 23.4% for K3 to 8.3% for K3.1 Pro. That is better than Claude Opus 5.5 (15.0%) but still behind GPT-6 Astra (5.5%). Moonshot notes that this test is deliberately designed to trip models up and does not reflect everyday error rates.

## How It Compares With Kimi K3

Since K3.1 Pro is built on K3, Moonshot shared several direct comparisons with its previous flagship. On long-horizon coding, it scores 61.2% on Kimi Code Bench 3.0, up from K3’s 55.95%. On Moonshot’s internal finance tests, it rises from 50.4 to 57.6 in financial modeling and from 58.8 to 70.4 in financial research. The biggest jump is in a simulated business operations test, where it scores 37.0 against K3’s 8.9, about four times higher.


Both models share the same 1-million-token context window. The biggest practical difference for developers is price.

## A Third Cheaper Than Kimi K3

According to Moonshot’s API pricing, K3.1 Pro costs $2 per million input tokens and $10 per million output tokens. Cached input costs $0.20 per million tokens.

That is a third cheaper than Kimi K3, which costs $3 per million input tokens, $15 per million output tokens, and $0.30 for cached input. The faster K3.1 Pro Highspeed version costs $4 per million input tokens and $20 per million output tokens.

Moonshot says pricing is flat across the full 1-million-token context window, with no extra charge for longer prompts. API access unlocks after a minimum $1 top-up, and Zero Data Retention is available for API customers.

## Deliberately Limited Hacking Skills

Moonshot says it found that cyber-related data in its training mix was steadily boosting the model’s ability to find and exploit software vulnerabilities. For the open-weight release, it removed that data from both mid-training and reinforcement learning.

On its internal xdev-bench-hard test, a version of the model trained without these limits solved 90.5% of tasks. The final K3.1 Pro solves 81.0%. Moonshot says its open weights are designed not to push the frontier of offensive cyber capability, and that K3.1 Pro remains behind frontier proprietary models in this area.

## How to Use It

K3.1 Pro is rolling out on Kimi.ai, the Kimi mobile apps for iOS, Android, and HarmonyOS, the Kimi Work desktop app (version 3.1.0 or later), and Kimi Code. Developers can access it through the Kimi API using the model IDs kimi-k3.1-pro and kimi-k3.1-pro-highspeed.

Kimi Enterprise administrators can enable the model for their organizations, though it is turned off by default at launch. The full open weights are due by November 4, 2026.

#### Stay Connected with ProPakistani

Get the latest tech news, telecom insights, and product launches wherever you prefer.

See more ProPakistani stories in Google Search and Top Stories.

excellent detail, Aasil. Thank you for the effort.
