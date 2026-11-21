---
id: collect-261121-veille-ia/copilot-microsoft/microsoft-mai-code-2-and-flash-price-quality-access
title: "Microsoft MAI Code 2 and Flash: Price, Quality, Access"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["microsoft", "flash", "code", "model", "cost", "work", "gpt-6", "microsofts"]
source: https://www.digitalapplied.com/blog/microsoft-mai-code-2-flash-price-quality-access
source_anchor: ""
source_lines: [1, 68]
section: "Copilot_Microsoft"
---
Microsoft says its new MAI Code 2 Flash matches GPT-6 Luna on three coding benchmarks for 65% of the cost. A footnote changes the buying decision. Microsoft priced Flash at half its regular rate for the comparison. Apply GitHub’s scheduled prices to the same work and keep Luna’s price fixed, and Flash would cost about 130% of Luna. A team choosing its coding model for the next year has two different budgets to consider.

Released on November 17, MAI Code 2 is the larger model for debugging, multi-file changes and work that requires understanding a repository. Flash is the cheaper option for everyday fixes. Both are available in GitHub Copilot, where there are published prices, plan rules and a date for the increase. Only Flash has a limited-access Microsoft Foundry preview. That distinction matters to anyone choosing between a developer seat today and an API for a product. Microsoft’s launch announcement

1. 01The larger model has the stronger case for difficult edits.Microsoft’s four-benchmark result reached 98% of GPT-6.1 Sol’s performance at 40% of its cost. The comparison uses maximum reasoning and promotional MAI pricing.
2. 02GitHub’s standard rates start May 17, 2027.The published input, cached-input and output rates each double at that point. A budget based on today’s rate alone will understate the same usage after the change.
3. 03Flash has the widest access.Its Copilot entitlement includes Free and Student. The larger model starts with Pro, and an organisation’s model policy can control access.
4. 04A benchmark can narrow the pilot.A team still needs to check the time to an accepted change, including review, retries and repairs. Microsoft says the models have yet to be tuned for very long-horizon work.

## 01 — Task selectionChoose by the *change*

The quickest way to choose is to ask what the next developer would have to read to verify the answer. For a test whose expected label changed, a single assertion and the component that renders it may be enough. Start with Flash. For a bug that appears only after saving an edited order, the trail might run from a form through a request handler to a persistence layer. Start with Code 2 if the difficult part is tracing which layer lost the update.

Microsoft describes the models as members of the same family, trained with reinforcement learning in the Copilot CLI and VS Code agent environments. Both can accept visual evidence such as screenshots and produce code. That makes an editor a reasonable place to evaluate them, because the model can see the project and work with the tools in which it was trained. It also means that moving a successful prompt into a product with a different toolset deserves a separate trial. Microsoft’s model card

Microsoft draws a boundary around its launch claims. It focused on frequent engineering work and says fundamental science and very long-horizon agentic work still need tuning. A few linked code changes with passing tests fit that description of an everyday task. An open-ended migration with unresolved design decisions, weeks of handoffs and no reliable automated check would make a poor first assignment.

## 02 — EvidenceWho was in the *benchmark*?

Microsoft averaged four benchmarks for the larger model. On that average, it reports 98% of GPT-6.1 Sol’s performance for 40% of the cost and 96% of Claude Opus 5.5’s performance for 5% of the cost. These are ratios to other models’ scores. A score equal to 98% of Sol’s score does not tell you how many issues either model solved. OpenAI’s GPT-6.2 Sol arrived on the same day, but Microsoft’s comparison used the earlier GPT-6.1 Sol. Microsoft’s results and footnotes

The four results came from two sources. Microsoft ran DeepSWE 1.1, Terminal-Bench 2.1 and SWE-Atlas on MSBench with the GitHub Copilot CLI and subagents disabled. The FrontierCode v1.1 cost and score came from Artificial Analysis. Microsoft specifies maximum reasoning effort across the comparison. Flash’s figures use the first three benchmarks only. Microsoft describes its quality as comparable to GPT-6 Luna and Claude Haiku 5.5 at 65% and 20% of their respective cost.

For interface work, there is a different piece of evidence. In a blind visual coding comparison involving 258 annotators, Microsoft reports a 64% preference for Code 2 over GPT-6 Sol. The comparison covered appearance and function across interfaces, games, charts and other graphics. Here the comparator is GPT-6 Sol. A preference in these paired judgments is a reason to try a dashboard task, but it tells you little about whether the generated chart will handle a missing series or a keyboard-only user. Put those cases in the brief.

## 03 — BillingPrice both sides of *May 17*

GitHub’s price table allows a team to put dollars on these claims. Each figure below is USD per million tokens in Copilot. Separate the ordinary input from tokens GitHub bills as cached input, then price the output. For accounts using consumption billing, one AI credit represents one US cent. The amount a team pays out of pocket also depends on its plan’s included allowance and any permitted additional usage. GitHub Copilot model pricing and footnotes

Consider a hypothetical job using 40,000 ordinary input tokens and 5,000 output tokens, with no cache hits. At the launch price, Code 2 uses $0.020 of input and $0.0125 of output for a total of $0.0325. Flash costs $0.00325 on the same volumes. If a team ran 2,000 such jobs, it would consume $65 or $6.50 of model usage. At the announced standard prices the same month would consume $130 or $13.

That example deliberately holds token counts constant. A model that reopens the same files, writes longer responses or needs a second correction can use more tokens to reach the same result. Cached tokens also change the relative price. Flash ordinary input is one tenth of Code 2’s at launch, while its cached input is one fifth. Exporting a week of actual usage by token category gives a better estimate than counting prompts.

## 04 — After launchReprice the *same run*

Because GitHub lists a doubling in every token category, there is a simple way to read Microsoft’s discount footnotes. Hold the observed workloads, token mix and rival prices fixed, and double only the cost attributed to MAI. The larger model’s reported 40% of GPT-6.1 Sol’s cost becomes roughly 80%. Its 5% of Opus 5.5’s cost becomes roughly 10%. The quality measurements do not change in this calculation.

For Flash, 20% of Haiku 5.5’s cost becomes around 40%, while 65% of Luna becomes around 130%. That second comparison crosses the break-even point. It is the one most likely to change a decision for a team already using Luna. These are sensitivity calculations using Microsoft’s rounded ratios and GitHub’s announced price change. New competitor pricing, a different harness or a different mix of tasks would require new arithmetic.

A team with a renewal in April could include both periods in its proposal. Count the projected jobs before May 17 at the launch price and the remaining jobs at the standard price. Separate the few troublesome jobs that frequently escalate. Folding them into a single average can obscure exactly where the larger model earns its premium. Guide to auditing Copilot AI credits

## 05 — EligibilityCheck what the *seat* includes

GitHub lists Code 2 for Copilot Pro, Pro+, Max, Business and Enterprise. Flash is available on those plans plus Free and Student. Both can appear in the model picker on supported clients, including VS Code, Visual Studio, Copilot CLI, JetBrains, Xcode, GitHub’s cloud agent and GitHub Mobile. Having a licence and seeing a model can still be two different experiences. For Business and Enterprise, administrators control model availability, and an explicit disable or a change to global defaults can keep it out of the picker. GitHub’s November 17 availability notice

Most teams should use the token table. The exception matters if someone asks why their usage screen still shows premium requests. GitHub retains a legacy request-based system for eligible existing annual Copilot Pro and Pro+ subscribers who stayed on it. On that system Code 2 has a 1× multiplier and Flash has a 0.33× multiplier. Those figures do not describe the credit cost of a token-billed Copilot interaction. Check which billing system the seat actually uses before comparing two developers’ figures. GitHub’s legacy annual-plan multipliers

## 06 — DeploymentDeveloping with the *API*

For an application that needs its own endpoint, Microsoft announced Code 2 Flash in a limited-access Foundry preview. The model card describes an OpenAI-compatible Responses API on a Foundry deployment. The larger Code 2 model’s confirmed launch surface is GitHub Copilot. That distinction matters when looking up the price of an API deployment. Model card, deployment details

Azure’s Retail Prices API lists three global Flash consumption meters under East US 2, effective November 1. They price each million tokens at $0.05 for ordinary input, $0.005 for cached input and $0.25 for output. Those are public retail meter prices. For a purchase, confirm the applicable meter and any agreement-specific pricing in the Azure subscription. GitHub’s May 17 increase applies to its Copilot table. The Azure meter records supply no corresponding future date. Azure retail meters for MAI Code 2 Flash

Microsoft named OpenRouter, Vercel, Hermes, OpenCode and Kilo as future distribution channels. Its announcement supplies no rollout schedule for those channels. Neither Code 2 model appeared in OpenRouter’s public API model listing checked November 19. If your product requires a particular gateway, wait for a callable model identifier there before putting it into a design or promising a delivery date. OpenRouter’s model listing

MAI Work Flash is a related model Microsoft further tuned from Code 2 Flash for products including Excel, PowerPoint and FabricIQ. A benchmark for the version embedded in Office does not establish that a generic coding endpoint will handle the same spreadsheet equally well. The earlier MAI family guide explains why those product-specific variants exist.

## 07 — Trial designFollow a bug through *review*

Suppose customers using a keyboard cannot submit an edited address. A focused first task for Flash is to locate the form event and add a reproducible failing interaction test. The test has a clear finish line. If tracing the problem reveals multiple code paths for saving the address, give Code 2 the test, the reproduction steps and the affected files. Ask for the smallest change that preserves both keyboard and pointer behaviour.

Run the same starting issue through your current model using the same tools. Keep the runner, repository state and acceptance check fixed. Record the cost of both attempts if one model hands the work to another. Add the reviewer’s time and whether a second bug report followed. A saving of a few cents on a first attempt is easy to spend again when a reviewer has to reconstruct why an edit works.

Teams already trialling newer OpenAI models can use the GPT-6.2 release guide to identify the model they actually run. If commissioning a pilot, specify these cases and the permitted tools in the brief for your AI transformation work. The deciding result should be reproducible when the launch discount is over.

### Make the discounted run earn a place at full price

Flash is a natural first candidate for a steady queue of simple fixes. Code 2 deserves the harder tasks where understanding the repository saves a second pass. Measure both at launch, reprice the accepted changes at May’s rates, and keep the cheapest option that consistently delivers the result your maintainers will merge.
