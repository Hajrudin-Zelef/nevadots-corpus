---
id: collect-261121-veille-ia/kimi/moonshot-ai-releases-kimi-k3-1-pro-what-makes-the-new-model-unique-htx-insights
title: "Moonshot AI Releases Kimi K3.1 Pro: What Makes the New Model Unique? | HTX Insights"
domain: kimi
role: reference
task: article
actors: ["Moonshot AI"]
dates: []
keywords: ["model", "kimi", "moonshot", "programming", "models", "capabilities", "complex", "tasks"]
source: https://www.htx.com/news/moonshot-ai-releases-kimi-k31-pro-what-makes-the-new-model-u-cReDd7hY/
source_anchor: ""
source_lines: [1, 105]
section: "Kimi"
---
**Written by: Li Dan**

Chinese large model company Moonshot AI has launched its next-generation flagship model Kimi K3.1 Pro, targeting complex professional tasks such as programming, 3D creation, and financial research, and plans to open the full model weights before November 4. The model has a total parameter scale of 2.8 trillion, adopts a Mixture of Experts (MoE) architecture, activates 104 billion parameters each time, and supports a 1 million Token context window.

Moonshot AI states that Kimi K3.1 Pro is the first model in the K3 series and also the company's most capable 3-trillion-level parameter model to date. Compared with previous models that focused on general capabilities, the new model places emphasis on strengthening long-horizon programming, 3D modeling, financial analysis, and the ability of Agents to execute complex tasks, and has reached or approached the level of top closed-source models in multiple tests.

Moonshot AI also simultaneously launched the high-speed version Kimi K3.1 Pro Highspeed, saying its intelligence level is comparable to the standard version, while output speed can be increased to about 2 to 3 times. At present, both models are already available through the Kimi official website, App, Kimi Work, Kimi Code, and API, and the full model weights are expected to be released before November 4.

For Moonshot AI, the core of this Kimi K3.1 Pro upgrade is not just pushing the parameter scale to the 3-trillion level, but further integrating programming, spatial reasoning, financial analysis, and long-horizon agent execution capabilities into the same model.

With the full weights about to be opened, what the market needs to observe next is whether the advantages shown in these benchmark tests can be consistently delivered in real projects, and whether developers can, at an acceptable deployment cost, turn the model's capabilities into stable and reliable productivity.

# **Backed by Large-Scale Reinforcement Learning, Complex Task Execution Capabilities Improved**

Kimi K3.1 Pro is not a new model trained from scratch, but a post-trained version based on Kimi K3. Moonshot AI says the new model greatly increased post-training compute and optimized areas including the reinforcement learning framework, long-context reasoning, training efficiency, sandbox infrastructure, and task scheduling systems to support large-scale joint training across hundreds of downstream domains.

Moonshot AI believes that the key to this training route is converting larger-scale reinforcement learning investment into sustained execution capability in complex tasks, rather than merely improving single-turn question-answering performance.

Moonshot AI says that on code, Agent, and knowledge benchmarks, K3.1 Pro has reached or approached the level of the strongest closed-source models. It disclosed that in the programming agent test Terminal-Bench 4.0, Kimi K3.1 Pro achieved an evaluation score of 65.7%, higher than OpenAI's GPT-6 Astra at 58.2%, and only slightly behind Anthropic's Claude Opus 5.5 at 66.4%.

Terminal-Bench 4.0 tests, through terminal tasks in real environments, the model's ability to use tools, execute commands, and solve programming problems under resource-constrained conditions.

In Moonshot AI's internal Kimi Code Bench 3.0 Long-horizon test, the new model scored 61.2%, higher than the previous generation K3's 55.95%. This test focuses on multi-step execution, tool invocation, and end-to-end engineering problem-solving capabilities.

It should be noted that the above results come from different testing systems, some of which are Moonshot AI's internal benchmarks. The testing environments, tool configurations, and evaluation methods of different models may differ, and scores cannot be directly equated with capability rankings in all actual work scenarios.

# **From Writing Code to Developing Games, Attempting to Complete Entire Projects End-to-End**

In programming, Moonshot AI hopes that Kimi K3.1 Pro can handle tasks ranging from fixing problems in existing codebases to developing complete software projects from scratch.

Cases demonstrated by Moonshot AI include using game engines such as Unreal Engine, Unity, and Godot to develop games, as well as generating web pages, Three.js interactive scenes, and 3D assets.

Among them, in Unreal Engine 5.8, the model built an Eastern mythological heavenly palace above a sea of clouds. Moonshot AI says that the colonnade in the project was procedurally modeled through Blender, the source model contains 36 million triangles, and it was rendered using Nanite and Lumen.

Another case is a 3D action-adventure game developed based on Godot, including five explorable regions, procedurally generated terrain and villages, third-person combat, a two-phase Boss fight, a day-night cycle, and fast travel functions.

The model also completed a browser-based 3D robotic arm display page, generating 2,317 parts and 268 dynamic cable runs, and implementing exploded views, transparent shells, and cross-section displays.

These cases attempt to show that the model's goal is no longer limited to generating code snippets, but further undertaking project planning, invoking professional tools, generating assets, debugging, and iteration.

Moonshot AI also demonstrated a more complex simulation task: the model built a human walking dynamics simulator based on an open-source human anatomy model and continuously adjusted the walking controller through 167 tasks and 420 independent review processes, finally enabling the simulated human body to walk continuously for 11.2 seconds.

More notably, when adjusting the reward mechanism could not improve arm movement, the model instead inspected the simulator dynamics, located and corrected 36 errors in the calculation of the shoulder muscle moment arm, and then retrained the controller.

Such cases reflect an important direction for long-horizon agent capabilities: the model not only needs to generate solutions, but also needs to locate problems based on experimental feedback and continuously improve results over multiple rounds of execution. However, the demonstration cases themselves cannot represent that the model can stably reproduce the same performance in all similar tasks.

# **Enhanced 3D Modeling Capabilities, Supporting Blender and CAD Professional Tools**

In addition to programming, 3D creation and spatial reasoning are also key areas of this Kimi K3.1 Pro upgrade.

Moonshot AI says Kimi K3.1 Pro excels at 3D creation and spatial reasoning, and is good at parametric modeling, such as mechanical parts, architectural models, multi-parameter configurable products, and custom furniture. In the BenchCAD benchmark test, the model achieved a score of 92.7% with tools, close to Claude Opus 5.5's 96.2% and GPT-6 Astra's 95.9%.

BenchCAD mainly examines whether the model can reconstruct 3D models by generating CAD code based on rendered images from multiple viewpoints. The model needs to understand the object's shape, proportions, and spatial relationships, and then convert visual information into usable three-dimensional structures.

Moonshot AI says Kimi K3.1 Pro can infer object structures from text descriptions, sketches, or reference images, and directly write and run scripts for professional tools such as Blender and CAD to complete parametric modeling, geometric adjustment, material modification, and scene layout, without relying on MCP connectors.

Cases demonstrated by Moonshot AI include building an airport indoor rainforest scene in Blender, using FreeCAD and Python to generate a turbofan engine model containing 2,100 entities, and designing a desk lamp that can be directly 3D printed.

Among them, the turbofan engine model supports step-by-step assembly, internal structure display, and rotation animations of multiple rotors; the desk lamp draws on the circular architectural form of Fujian tulou and is designed as a hollow structure composed of five parts.

At the same time, the Kimi official website and App have launched the 3D Make function, supporting online preview and editing of 3D models, and allowing users to download models for 3D printing, or export videos and live photos.

This means Moonshot AI is attempting to further transform the model's spatial reasoning and code generation capabilities into directly usable creative tools.

# **Financial Research Becomes a Key Breakthrough Direction, Factual Error Rate Declines**

Financial research is another key area strengthened by Kimi K3.1 Pro.

Moonshot AI says the new model can complete relatively full workflows around financial modeling, investment analysis, and industry research, rather than merely answering financial questions or summarizing research reports.

In the OfficeQA Pro v2 benchmark test, Kimi K3.1 Pro scored 64.4 points, and Moonshot AI says it achieved the current state of the art (SoTA). This test focuses on complex data analysis based on real financial documents, involving information retrieval, cross-document verification, and multi-step numerical reasoning.

In the company's internal financial modeling test, the new model improved by 14% compared with K3; financial research tests improved by 20%; and the score in business operations tests reached about 4.2 times that of K3.

In actual application cases, the model generated a financial model for Nvidia, supporting adjustment of operating assumptions, formula linkage, and sensitivity analysis; it also produced an investment committee report for CATL, sorting out the differences between market consensus expectations and the company's actual performance, and organizing possible future catalysts.

Moonshot AI also demonstrated a quantum computing industry research report, emphasizing that the data and citations in the report can all be traced back to primary sources.

However, financial research has much higher accuracy requirements than general content generation. The model's ability to produce a complete financial workbook or research report does not mean that every piece of data, assumption, and conclusion in it is necessarily correct, and actual use still requires verification of key figures and their sources.

To address this issue, Moonshot AI specifically conducted a factual accuracy stress test.

In the internal Kimi Hallu Bench Hard test, the new model's error rate for individual factual statements dropped from 23.4% in K3 to 8.3%. The proportion of responses containing at least one factual error dropped to about 42.4% of the previous generation.

Moonshot AI particularly points out that this test deliberately selects high-difficulty scenarios where the model is prone to errors, and the test results do not represent the overall error rate in daily use.

Nevertheless, reducing the factual error rate is still an important condition for models to enter professional scenarios such as finance and enterprise operations. For these tasks, whether the model can provide traceable evidence, verify calculation processes, and promptly detect errors may be more important than simply increasing response speed.

# **Standard Version API Output Price 68 Yuan per Million Tokens, Security Assessment Before Opening Weights**

In terms of commercialization, Moonshot AI has adopted an arrangement in which product services come first and model weights are opened later.

At present, Kimi K3.1 Pro has been integrated into the Kimi official website, App, Kimi Work, Kimi Code, and Kimi API. Developers can call the model through the API, and enterprise users can use it for programming, research, and other agent workflows.

According to the prices announced by Moonshot AI, the API input price of the standard version Kimi K3.1 Pro is 1.3 yuan per million Tokens for cache hits and 13 yuan for cache misses, with an output price of 68 yuan. The output speed of the high-speed version is about 2 to 3 times that of the standard version, and the price is twice that of the standard version.

Moonshot AI also launched the Kimi Hosted Agents managed agent service, providing cloud runtime environments for enterprises and developers, supporting long-duration task execution and multi-agent collaboration, and reducing the cost of building and maintaining agent runtime frameworks on their own.

What attracts more attention from the open-source community is the release plan for the model weights. Moonshot AI says the full weights will be opened before November 4. This will give developers the opportunity to deploy, research, and adapt the model on their own infrastructure, and the specific scope of use will still be subject to the final released license terms.

At the same time, Moonshot AI also disclosed a cybersecurity assessment of the open-weight model. The company says that when preparing to open the Kimi K3.1 Pro weights, it identified and removed data related to cyber offense and defense from the mid-training and reinforcement learning stages to limit the model's vulnerability research capabilities.

In its self-developed xdev-bench-hard test, the model without relevant data removal processing scored 90.5%, while the final version scored 81.0%. The company says this indicates that targeted data cleaning reduced the model's vulnerability research capabilities, but stronger general programming and long-horizon debugging capabilities may still transfer to cybersecurity tasks.

This also reflects the dilemma faced by open-weight models: on the one hand, open models help enterprises and researchers deploy locally, inspect code, and conduct defensive security work; on the other hand, as model capabilities continue to increase, developers are also required to assess their potential risks in dual-use scenarios such as cyber offense and defense.
