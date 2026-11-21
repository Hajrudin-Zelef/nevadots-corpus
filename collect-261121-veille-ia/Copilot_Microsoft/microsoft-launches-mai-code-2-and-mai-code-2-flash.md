---
id: collect-261121-veille-ia/copilot-microsoft/microsoft-launches-mai-code-2-and-mai-code-2-flash
title: "Microsoft Launches MAI Code 2 and MAI Code 2 Flash"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["code", "microsoft", "cost", "flash", "tasks", "developers", "coding", "models"]
source: https://aiarabai.com/en/microsoft-mai-code-2-flash/
source_anchor: ""
source_lines: [1, 83]
section: "Copilot_Microsoft"
---
Microsoft has introduced MAI Code 2 and MAI Code 2 Flash, two AI models built to help software developers write code, debug problems, and handle tasks across large software projects. Announced at Microsoft Ignite 2026 on November 17, the models target different workloads: MAI Code 2 is designed for complex software engineering, while Flash focuses on routine coding tasks that developers need to run frequently.

Microsoft says MAI Code 2 delivers 96% of Claude Opus 5.5’s performance at just 5% of its cost, based on an average across four coding benchmarks. Both models are available through GitHub Copilot, giving developers another option as Microsoft expands its own AI model portfolio.

## MAI Code 2 vs. MAI Code 2 Flash: What’s the Difference?

The two models share a similar architectural foundation, but differ in how they were trained, their parameter size, and the types of coding work they handle.

MAI Code 2 is the larger model, built for software engineering tasks that require an understanding of multiple files and a project’s overall structure. Its intended workloads include investigating difficult bugs, refactoring code, making extensive changes across repositories, and preparing modifications that developers can review and merge into their projects.

MAI Code 2 Flash takes a different approach. It targets everyday development work, including explaining sections of code, making focused edits, fixing common errors, and testing alternative solutions. Its smaller size and higher speed make it suitable for handling large volumes of routine tasks at a lower cost.

This division gives developers a choice based on the complexity of each task. Larger engineering problems go to MAI Code 2, while straightforward edits do not justify a larger, costlier model.

## MAI Code 2 Benchmarks: Performance Compared With GPT and Claude

Microsoft highlights operating cost as one of the main advantages of MAI Code 2. According to the company’s results, the model achieves 98% of GPT-6.1 Sol’s performance while costing 40% as much. Against Claude Opus 5.5, it reaches 96% of the competing model’s performance at only 5% of its cost.

The comparisons use the average results from four coding benchmarks: DeepSWE 1.1, Terminal Bench 2.1, SWE-Atlas, and FrontierCode v1.1.

These evaluations cover tasks that require models to understand existing code, identify and fix problems, implement changes across projects, and produce results that can be used in development environments. Microsoft ran its internal evaluations at the highest reasoning-effort level, using GitHub Copilot CLI without sub-agents.

There is an important pricing detail behind the reported cost figures. Microsoft calculated MAI Code 2’s cost using a 50% promotional launch discount. The advertised savings therefore reflect the temporary launch pricing and the conditions used in the tests. They should not be treated as fixed cost ratios after the promotion ends or across different production environments.

These benchmarks show Microsoft prioritizing lower operating costs for routine engineering over chasing the single highest benchmark score.

## MAI Code 2 Flash Targets Everyday Coding at Lower Cost

MAI Code 2 Flash is designed for a different part of the development workflow. Instead of focusing on complex changes across large repositories, it targets tasks developers perform repeatedly throughout the day.

Microsoft says Flash delivers performance comparable to GPT-6 Luna at 65% of its cost. Compared with Claude Haiku 5.5, the company reports similar performance at 20% of the cost.

The evaluation uses three benchmarks: DeepSWE 1.1, Terminal Bench 2.1, and SWE-Atlas. The tests focus on high-volume, routine coding tasks, and the cost comparisons also account for a 50% promotional discount.

This pricing structure directly benefits projects built around frequent small edits, code checks, and repeated debugging. When developers or coding agents run these tasks at scale, their combined cost can become a significant part of the development budget.

## Visual Coding and AI-Generated User Interfaces

MAI Code 2 is not limited to generating text-based code. Microsoft has also trained it to create dashboards, visualizations, and user interfaces.

Its visual coding capabilities cover user interfaces, games, charts, 3D scenes, and SVG graphics. The model also accepts image inputs, allowing developers to provide a screenshot or an initial design and ask the model to turn it into code for a software interface.

Microsoft evaluated this capability through a blind human review comparing MAI Code 2 outputs with those generated by GPT-6 Sol. Reviewers preferred Microsoft’s model in 67% of the comparisons. The evaluation involved 258 reviewers and focused on the quality of the resulting designs and their functionality, rather than code structure alone.

## MAI Code 2 Technical Specifications and Context Window

The MAI Code 2 family uses a Transformer architecture with Mixture of Experts layers and an integrated visual encoder for image understanding.

According to Microsoft’s model card, the models support a context window of up to 1.05 million tokens. The maximum input length is 922,000 tokens, while the maximum output length reaches 128,000 tokens.

That context size lets developers load large portions of a repository into a single session-enough to analyze interconnected files or trace bugs across multiple parts of an application.

Both models also support five reasoning-effort levels: Low, Medium, High, XHigh, and Max. Developers can select an appropriate level based on task complexity instead of using the highest setting for every request.

Microsoft says MAI Code 2 was trained from scratch, while the Flash version was distilled from a larger internal model. The two models also differ in their active parameter counts: approximately 80 billion for MAI Code 2 and 12 billion for MAI Code 2 Flash.

## How to Use MAI Code 2 in GitHub Copilot

Both models are available through GitHub Copilot, although supported subscription plans differ.

MAI Code 2 is available to users on Copilot Pro, Pro+, Max, Business, and Enterprise plans. MAI Code 2 Flash also supports Free and Student plans.

Developers can access the models across several development environments and Copilot products, including Visual Studio Code, Visual Studio, GitHub Copilot CLI, and the Copilot cloud agent. Availability also extends to the Copilot app, GitHub’s website and mobile app, as well as JetBrains, Xcode, and Eclipse.

Usage follows a consumption-based billing system. Administrators on Business and Enterprise subscriptions can also manage access to MAI models through their organization settings. Both models are also available to eligible GitHub Copilot subscribers on annual plans.

For developers who want to try the models outside Copilot, MAI Code 2 Flash is available through a limited-access preview in Microsoft Foundry. Microsoft has also announced plans to bring the models to additional platforms, including OpenRouter, Vercel, Hermes, OpenCode, and Kilo.

## MAI Code 2 Limitations and Intended Use

Despite the benchmark results, Microsoft defines a specific scope for the two models. They are designed for everyday software engineering and have not been tuned for foundational scientific research or long-horizon agent tasks that require an extended sequence of actions.

MAI Code 2 Flash is also less suited to highly complex projects than the larger model, given its size and intended workload. Microsoft’s model card recommends reviewing, testing, and security-checking generated code before integrating it into production products.

The documentation also points to limited evaluation coverage for languages other than English. Output quality may therefore be lower when developers provide instructions or interact with the models in languages other than English.

## What Microsoft’s MAI Code 2 Launch Means for AI Coding

With MAI Code 2 and Flash, Microsoft is shifting toward specialized models built directly into its developer tools instead of relying on one general-purpose model. Offering a larger model alongside a smaller version gives the company a way to address different development needs, from complex debugging to frequent everyday edits.

The release also sharpens price competition across the AI coding market, putting direct pressure on rival platforms to justify higher per-token costs.

Now that both models are live in GitHub Copilot, developers can test whether those reported savings hold up in production codebases-and whether they actually cut down on writing and review time compared with rival tools.
