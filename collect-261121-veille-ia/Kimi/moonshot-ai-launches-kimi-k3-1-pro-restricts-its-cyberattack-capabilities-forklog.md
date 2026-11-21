---
id: collect-261121-veille-ia/kimi/moonshot-ai-launches-kimi-k3-1-pro-restricts-its-cyberattack-capabilities-forklog
title: "Moonshot AI launches Kimi K3.1 Pro, restricts its cyberattack capabilities | ForkLog"
domain: kimi
role: reference
task: article
actors: ["Moonshot AI"]
dates: []
keywords: ["kimi", "moonshot", "capabilities", "model", "scored", "financial", "developers", "launches"]
source: https://forklog.com/en/moonshot-ai-launches-kimi-k3-1-pro-restricts-its-cyberattack-capabilities/
source_anchor: ""
source_lines: [1, 61]
section: "Kimi"
---
# Moonshot AI launches Kimi K3.1 Pro, restricts its cyberattack capabilities

Moonshot AI launches Kimi K3.1 Pro and restricts its cyberattack capabilities.

Chinese company Moonshot AI introduced the Kimi K3.1 Pro language model with improved programming, 3D modeling, and financial analysis capabilities. The developers intentionally limited its ability to find and exploit vulnerabilities.

The new model is a fine-tuned version of Kimi K3. It is built on a Mixture-of-Experts (MoE) architecture with 2.8 trillion parameters, of which 104 billion are activated per query. The context window is 1 million tokens, and the model works with text and images.

At the same time, the company released Kimi K3.1 Pro Highspeed. According to the developers, it generates responses two to three times faster than the standard version.

Both models are available through the Kimi website and apps, the Work and Code services, and the API. The developers said they would publish the full weights by November 4.

## Programming and agent tasks

According to Moonshot AI, K3.1 Pro scored 65.7% on Terminal-Bench 4.0 — a test of AI agents performing engineering tasks via a terminal. For comparison, Claude Opus 5.5 scored 66.4%, and GPT-6 Astra scored 58.2%.

However, the results were obtained using different methodologies: the company tested its own model through Claude Code, and took competitors’ scores from published reports and the official leaderboard.

In the internal Kimi Code Bench 3.0 Long-horizon, which evaluates long-running programming tasks, the new model scored 61.2% versus 55.95% for its predecessor.

Independent testing by Artificial Analysis confirmed progress over K3. The new model received 55 points in the Intelligence Index versus 44 for the previous version and ranked fourth.

At the same time, K3.1 Pro generated on average 14% more output tokens when solving tasks. Despite a one-third cut in API rates, the cost of completing a single task remained roughly in line with K3.

## 3D modeling and financial analysis

Moonshot AI also expanded the model’s 3D graphics capabilities. According to the developers, K3.1 Pro can create ready-to-use 3D models from text descriptions, sketches, and images, determining shape, proportions, and the relative arrangement of elements.

The system can independently write and run scripts for Blender and CAD software, and modify geometry, materials, and object layouts. Claimed use cases include designing mechanical parts, architectural structures, and game scenes.

Kimi apps now include a 3D Make feature that lets users view and edit generated models and export them for 3D printing.

In the BenchCAD test for reconstructing 3D objects from images, K3.1 Pro scored 92.7 points on the Voxel IoU metric when using tools. Claude Opus 5.5 scored 96.2, and GPT-6 Astra scored 95.9.

Another focus is financial analysis. Moonshot AI said the model can process corporate reports, cross-reference information from multiple documents, build financial models with linked formulas, and prepare investment research.

In OfficeQA Pro v2, which evaluates analysis of financial documents and multi-step calculations, K3.1 Pro scored 64.4 points. In internal tests, financial modeling results improved by 7.2 points versus K3, and investment research by 11.6 points.

The developers also reported a decrease in the share of incorrect statements from 23.4% to 8.3% in the internal Kimi Hallu Bench Hard. However, the test is intentionally composed of difficult questions and does not reflect the error rate in typical use.

## Developers limited capabilities for cyberattacks

Moonshot AI paid particular attention to model safety ahead of the planned publication of its weights.

During training, engineers observed that vulnerability discovery and exploitation skills increased as fine-tuning scaled. The source was cybersecurity materials in the general training corpus and corresponding tasks in the reinforcement learning environment.

To reduce potential risks, such data were excluded at both stages of training, while standard programming tasks were retained.

On the internal xdev-bench-hard, an experimental version without these restrictions successfully completed 90.5% of tasks, whereas the final K3.1 Pro completed 81%.

In the test, the model receives source code with a recently disclosed vulnerability and a short prompt, then must write an exploit and achieve arbitrary code execution in an isolated environment. The results have not undergone independent verification.

The previous Kimi K3 scored 33.3% on the same test. The company acknowledged that even after the restrictions, the new system’s capabilities increased significantly: improved programming skills transfer to such tasks.

However, in Moonshot AI’s assessment, K3.1 Pro still lags behind leading proprietary models that have been available for at least three months.

Moonshot AI said it was unwilling to publish open weights that would set a new high-water mark for cyberattack capabilities. At the same time, the company emphasized the usefulness of such tools for defensive software audits.

API pricing for the standard K3.1 Pro is $2 per 1 million input tokens and $10 per 1 million output tokens.

In July, Moonshot AI introduced Kimi K3 — which the project described as the world’s first open 3T-class model with native image support. The model is designed for long-running autonomous programming tasks.
