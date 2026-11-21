---
id: collect-261121-veille-ia/copilot-microsoft/mai-code-2-local-harness-removal-copilot-sdk-2-0-s5-labs
title: "MAI Code 2, Local Harness Removal, Copilot SDK 2.0 | S5 Labs"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-11-17"]
keywords: ["code", "copilot", "models", "flash", "model", "github", "changelog", "docs"]
source: https://s5labs.io/resources/insights/github-copilot-mai-code-2-local-harness-sdk-2/
source_anchor: ""
source_lines: [1, 150]
section: "Copilot_Microsoft"
---
GitHub added **MAI Code 2 and MAI Code 2 Flash to the Copilot model picker on November 17**, during Microsoft Ignite. MAI Code 2 is on Copilot Pro, Pro+, Max, Business and Enterprise, and Flash is on those plans plus Free and Student, where the docs say models come through auto selection only. GitHub’s pricing docs list MAI Code 2 at $0.50 per million input tokens and $2.50 per million output tokens, a promotional rate that doubles on May 17, 2027. The day before, GitHub said VS Code 1.146 deprecates the Local harness, which will be removed in February 2027. Microsoft also released Copilot SDK 2.0 under a new name, with a Rust runtime and breaking changes for 1.x code. MAI changelog · Models and pricing · Local harness changelog · SDK post

Microsoft’s posts give no per-token price for either Code model. The prices below come from GitHub’s docs, and every performance-per-dollar claim is Microsoft’s own. S5 Labs has not run MAI Code 2, the Copilot harness or SDK 2.0. This is a reading of GitHub’s changelogs and docs, Microsoft’s posts, the VS Code 1.146 release notes, the SDK repository and the public Terminal-Bench leaderboard as they stood on November 17.

## What you can select today

Both models are generally available and appear in the picker in VS Code, Visual Studio, Copilot CLI, the Copilot cloud agent, the Copilot app, github.com, GitHub Mobile for iOS and Android, JetBrains, Xcode and Eclipse. They succeed the MAI-Code line Microsoft introduced at Build in June. The changelog has no benchmark numbers. MAI changelog

The docs attach conditions:

- **Free and Student.** These plans “have access to models through auto model selection only”. The docs’ table for auto selection and Fusion lists MAI Code 2 Flash and not MAI Code 2.
- **Business and Enterprise.** Admins manage MAI access through the model policy, and “new models are enabled automatically unless an administrator has turned off the global default or explicitly disables the model.” An organization that left default enablement on already has both.
- **Context and reasoning.** The 1 million token context window works only in VS Code and Copilot CLI, and configurable reasoning only in VS Code, Copilot CLI and the cloud agent. Larger context or higher reasoning uses more AI credits.
- **Client versions.** Visual Studio needs 17.14.6. The minimum for VS Code, JetBrains, Xcode and Eclipse is listed as TBD.
- **Checkpoints.** A footnote says “MAI models are continuously improving models. Performance and behavior may evolve over time as new checkpoints are released.” Date any evaluation you run.

One changelog sentence is unexplained: “Both models are available to eligible GitHub Copilot subscribers on annual plans.” The docs’ per-plan table shows no annual-only restriction. MAI changelog · Supported models

GitHub “will deprecate MAI-Code-1.1-Flash across all GitHub Copilot experiences on December 18, 2026”, and Enterprise administrators “may need to enable access to the alternative model through their model policies”. The changelog lists the suggested alternative as the “Latest MAI Code Flash model”, which today is MAI Code 2 Flash. Our retirement tracker lists the other Copilot removals. Deprecation changelog

## What GitHub charges

Both models fall under usage-based billing, where 1 AI credit is $0.01. GitHub’s rates per million tokens:

| Model | Input | Cached input | Output | 
|---|---|---|---|
| **MAI Code 2, until May 17, 2027** | **$0.50** | **$0.025** | **$2.50** | 
| MAI Code 2, from May 17, 2027 | $1.00 | $0.05 | $5.00 | 
| **MAI Code 2 Flash, until May 17, 2027** | **$0.05** | **$0.005** | **$0.25** | 
| MAI Code 2 Flash, from May 17, 2027 | $0.10 | $0.01 | $0.50 | 
| MAI-Code-1.1-Flash | $0.20 | $0.02 | $1.20 | 
| GPT-6.1 Sol (up to 272K) | $2.00 | $0.10 | $10.00 | 
| Claude Opus 5.5 | $4.00 | $0.20 | $20.00 | 
| GPT-6 Luna (up to 272K) | $0.10 | $0.01 | $0.50 | 
| Claude Haiku 5.5 (up to 100K) | $0.10 | $0.01 | $0.50 | 

The ratios here are our arithmetic on those list prices. At the promotional rate MAI Code 2 costs 25% of GPT-6.1 Sol and 12.5% of Opus 5.5 on input and output, and from May 17, 2027 it costs 50% and 25%. Flash is half the price of GPT-6 Luna and Claude Haiku 5.5 today and the same price afterward. Against MAI-Code-1.1-Flash, Flash costs a quarter as much on input and about a fifth on output now, and half and about 42% after the promotion, so the December 18 move lowers the token price either way.

A request with 100,000 uncached input tokens and 10,000 output tokens costs $0.075 on MAI Code 2 today, or 7.5 AI credits, and $0.15 from May 17. It costs $0.30 on GPT-6.1 Sol and $0.60 on Opus 5.5. A model that needs more turns costs more per finished task than token prices suggest, and a budget built on the promotional rate needs a second line for the doubled one.

## How Microsoft measured its ratios

Microsoft’s Ignite post says MAI Code 2 delivers “98% of the performance of GPT-6.1 Sol at 40% of the price, and 96% of Claude Opus 5.5’s performance at just 5% of the price.” Its announcement post gives the same figures as “40% of the cost” and “5% of the cost”. Both comparisons are averages over DeepSWE 1.1, Terminal Bench 2.1, SWE-Atlas and FrontierCode v1.1. The Ignite post gives no per-benchmark scores. A chart in the announcement does: MAI Code 2 averages a 63.2% pass rate against 64.1% for GPT-6.1 Sol and 65.3% for Opus 5.5, at an average cost per task of $0.309 against $0.761 and $6.306. Results vary by benchmark. On SWE-Atlas, MAI Code 2 passes 43.1% against 57.5% for Opus 5.5, and on DeepSWE 1.1 it passes 73.2% against 69.0%. Ignite post · MAI Code 2 announcement

The announcement’s footer narrows the claim: “All models were run at max reasoning effort. DeepSWE 1.1, TerminalBench 2.1 and SWE Atlas were run on MSBench (Microsoft’s internal eval platform) with the GitHub Copilot CLI without subagents. FrontierCode 1.1 scores and costs are from Artificial Analysis; MAI-Code-2 cost is shown at its 50% promotional price.” Microsoft ran three of the four benchmarks itself, and the cost side uses the promotional price, which GitHub’s docs end on May 17, 2027. These are average costs per benchmark task, which is why they do not equal the 25% and 12.5% list-price ratios above. Beyond the FrontierCode figures Microsoft attributes to Artificial Analysis, we found no independent benchmark of either model.

Microsoft’s other claims:

- **Frontend preference.** Reviewers preferred MAI Code 2’s results over GPT-6 Sol 67% of the time in the announcement post and 68% in the Ignite post. The comparator is GPT-6 Sol, not GPT-6.1 Sol, and the footer reports 258 annotators.
- **Flash.** Microsoft calls it “comparable” to GPT-6 Luna and Claude Haiku 5.5 “at 65% and 20% of the price” across three of the same benchmarks, with a matching footer.
- **Scope.** Both Microsoft AI posts say the models are “explicitly not yet tuned for harder problems, like fundamental science research and very long horizon agentic work.” Microsoft’s Ignite live-news page says MAI Code 2 “excels at complex, long horizon tasks”. None of the three pages reconciles the two statements.
- **Outside Copilot.** Flash is in “limited access preview in Microsoft Foundry”. Neither post states Foundry availability for MAI Code 2.

MAI Code 2 announcement · Ignite post · Ignite live news

Microsoft’s OpenAI comparators are GPT-6.1 Sol, GPT-6 Sol and GPT-6 Luna. OpenAI released GPT-6.2 Astra and Sol the same day.

## The Local harness has a removal month and no day

VS Code 1.146, released November 11, deprecates the Local harness, the older software that coordinates the model, context and tools for Copilot agent sessions in the editor. GitHub’s changelog says it “will be removed in February 2027”, and neither the changelog nor the release notes give a day. The replacement, which the changelog calls the Microsoft Copilot harness, has been the default for new sessions since 1.143. It is built on the Copilot SDK and shares its runtime with Copilot CLI and the Copilot app, and choosing it “doesn’t move your files to the cloud”. Local harness changelog · VS Code 1.146

Until removal the Local harness stays available “where your organization’s policy permits”, but the release notes call it “functionally frozen”. The notes also say that because the Copilot harness is already the default, “you might not need to take any new actions.” The changelog adds review steps for people with customizations and for administrators. Plan for three things.

**Customizations do not all convert.** GitHub’s changelog says “Switching harnesses doesn’t automatically convert every customization.” VS Code’s migration guide, which labels user customization migration experimental, says “there is no automatic hook migration” and that tool sets and chat participants are “deprecated and not supported in the Copilot harness.” Local harness changelog · Migrate customizations

**Sandboxing is not switched on.** The Copilot harness lets administrators “enforce managed runtime permissions and sandbox restrictions that the Local harness cannot enforce”, but GitHub adds: “Configure these controls separately; selecting the Copilot harness doesn’t enable sandboxing.” The 1.146 notes describe a “(sandbox enabled)” indicator. Our piece on Claude Code’s sandboxed Bash prompts covers sandbox defaults in another tool. Local harness changelog · VS Code 1.146

**The admin exception expires with the harness.** The enterprise setting `vscode.temporaryDefaultToLocalHarness` “will be removed along with the Local harness, so keeping the exception configured won’t keep the Local harness available.” Local harness changelog

## Copilot SDK 2.0 for people building on the harness

Microsoft announced “the new Microsoft Copilot SDK (previously named GitHub Copilot SDK)” on November 17, and `microsoft/copilot-sdk` v2.0.0 was published the same day. When we covered the rename on October 30 it had no dates and no new package names. The SDK is available in TypeScript, Python, Go, .NET, Java and Rust. SDK post · SDK repository

The runtime was rebuilt in Rust and is hosted in-process by default. Microsoft’s figures: “creating a client and a session and completing one turn went from 5.25 seconds on the TypeScript CLI to 55.3 milliseconds with the Rust core in-process”, 10 clients use 126 MB of memory instead of 1,383 MB, and throughput rose from 7.55 to 120 sessions per second. The post gives no hardware, model or sample size and does not say whether the turn includes a model call, so do not read these as end-to-end latency figures. We found no independent reproduction. SDK post

The larger change is multi-tenancy. One process can host many sessions, each with its own “state, credentials, tools, files, and history. No session can see another.” A session created with `mode: "empty"` starts with “no ambient shell, files, or CLI defaults”. Passing `gitHubToken` to `createSession`, or a `gitHubTokenProvider` for rotating tokens, attributes calls to the requesting user. The post explains: “Before 2.0, the runtime acted under the identity that instantiated it.” SDK post

The isolation is your code’s job. In the shared-runtime design it “is enforced by the application rather than a container boundary”, and `resumeSession` is meant to be called “once you’ve checked that the caller owns it”. The repository’s breaking-changes document warns: “In-process hosting is not a sandbox and provides no crash isolation. A native runtime crash can terminate your application.” An agent that runs code still needs a sandbox around it. A section heading in the post promises six architectures and the section describes five. You host them yourself today, with GitHub Copilot cloud sandboxes and Microsoft Foundry managed agents named as the managed alternatives. SDK post · v2 breaking changes

The breaking changes are documented in the repository, not in the announcement. The persisted session and event format is unchanged. v2 breaking changes

| Package | 1.x | 2.0 | 
|---|---|---|
| npm | `@github/copilot-sdk` | `@microsoft/copilot-sdk` | 
| .NET | `GitHub.Copilot.SDK` | `Microsoft.Copilot.SDK` | 
| Python | `github-copilot-sdk` | `microsoft-copilot-sdk` | 
| Java | `com.github:copilot-sdk-java` | `com.microsoft:copilot-sdk` | 
| Go | `github.com/github/copilot-sdk/go` | `github.com/microsoft/copilot-sdk/go/v2` | 
| Rust | `github-copilot-sdk` | `copilot-sdk` | 

- **No default mode.** “A missing mode previously selected capabilities implicitly, including coding tools that can access the host filesystem.” Version 2 requires`coding-agent` or`empty` .
- **`connection` becomes `runtime`.** The announcement’s own code sample still uses`connection` , so copy code from the breaking-changes document.
- **No compatibility layer.** “Upgrade the SDK and runtime together: incompatible pairs fail connection negotiation, and legacy option names and flat lifecycle payloads have no compatibility adapters.”
- **Child processes are opt-in.** Use`RuntimeConnection.forSpawnedStdio()` or`COPILOT_SDK_DEFAULT_CONNECTION=stdio` . There is no automatic fallback.
- **Agent Framework users wait.** The .NET and Python Microsoft Agent Framework adapters “are not compatible with v2; keep those applications on v1 until compatible adapters are available.”
- **Python.** Uninstall the old distribution first, because old and new share the`copilot` import.

The README still says “No immediate changes are required for existing users”, which is true only if you stay on 1.x, where the latest tag is v1.0.34 from November 14. SDK repository

## Fusion on more surfaces, with a public Terminal-Bench result

Copilot Fusion can now be selected on github.com, the latest Visual Studio and the GitHub Mobile beta apps for iOS and Android. GitHub added Claude Sonnet 5.5 to Fusion Max, GPT-6.1 Terra to Fusion High and MAI Code 2 to Fusion Standard. As of November 17 the docs’ table for auto selection and Fusion still has no MAI Code 2 row. Fusion changelog · Supported models

GitHub says that on Terminal-Bench 4.1, “Copilot Fusion Max achieved a 71.2% resolution rate, 5.4 percentage points ahead of leading frontier models Opus 5.5 and Fable 5.2, while costing 88% less than Fable 5.2 and 57% less than Opus 5.5.” The changelog gives no cost method. The public leaderboard, which names Stanford, Harbor and the Laude Institute as hosts, shows the same figures: Copilot Fusion at max effort resolves 71.2% at a listed cost of $2.0k, and Opus 5.5 and Fable 5.2, both at max effort in Claude Code, resolve 65.8% at $4.7k and $16.5k. The 95% confidence intervals of first and second place overlap slightly, at ± 2.9 points for Fusion and ± 3.1 for Opus 5.5. Terminal-Bench leaderboard

## Microsoft-Decision-1.1 is generally available, outside Copilot

Microsoft-Decision-1.1 is not in the GitHub Copilot model picker. It is “now generally available in Microsoft Foundry and through OpenRouter and Vercel”, adds image input, and is “more than twice as fast as our first model, at the same cost”. Foundry charges per million input tokens, with output tokens free: $0.042 on Global Standard, $0.046 in the US Data Zone and $0.05 in the EU and APAC Data Zones. Microsoft reports a score of 68 on the Artificial Analysis Decision Index v2.1, “ahead of Jev 1.14, Google’s Gemini Decision 1, and OpenAI’s GPT-6 Luna (Decisions)”. Decision GA post

The post’s routing result, that the model decides “as accurately as a large language model, 2.2x faster and at 4.4x lower cost”, describes Microsoft’s own Copilot and Cowork products, not GitHub Copilot. The Ignite post’s “50x faster and 79x cheaper than GPT-Sol 6.1” comes with no method and does not appear in the GA post. Our comparison of open decision models covers what this class of model replaces. Decision GA post · Ignite post

## The dated checklist

| When | Who | What to do | 
|---|---|---|
| Now | Business and Enterprise admins | Open the model policy. With default enablement on, both MAI models are already enabled, so decide whether that is what you want. | 
| Now | VS Code users | Start a new session with Copilot under Session Target. List your hooks, tool sets and chat participants. | 
| Now | SDK maintainers | Pin 1.x to an exact version. Read the breaking-changes document before trying 2.0, and stay on v1 if you use the .NET or Python Agent Framework adapters. | 
| Before December 18, 2026 | Anyone using MAI-Code-1.1-Flash | Move workflows and integrations to MAI Code 2 Flash. Enterprise admins confirm the replacement is enabled in policy. | 
| Before February 2027 | VS Code admins | Verify required workflows, permission controls and sandbox configuration on the Copilot harness. Configure sandboxing as its own step, and expect `vscode.temporaryDefaultToLocalHarness` to stop working. | 
| Before May 17, 2027 | Whoever owns the Copilot budget | Re-price MAI usage at the doubled rates and rerun any comparison that used the promotional price. | 

Because GitHub gives a month and no day for the Local harness, plan to be finished by January 31. Teams that run review bots have an earlier date too: public repositories lose `pull_request_target` by default on December 2.

The models are the cheapest part of this to evaluate and the easiest to undo. Run MAI Code 2 against your current default on your own tasks at a fixed reasoning level and count AI credits per finished, reviewed change, since three of the four benchmarks behind Microsoft’s 98% and 96% figures ran on its internal platform. The harness and SDK changes take engineering time, so start with the hooks inventory and the sandbox configuration, the two items that selecting the Copilot harness leaves undone.

## Sources

**GitHub changelog**

- MAI Code 2 and MAI Code 2 Flash in GitHub Copilot — November 17, 2026
- Upcoming deprecation of MAI-Code-1.1-Flash in December — November 17, 2026
- GitHub Copilot Fusion updates: more availability, benchmarks, and models — November 17, 2026
- VS Code retires the Local harness in February 2027 — November 16, 2026

**GitHub and VS Code documentation**

- Supported AI models, Models and pricing — GitHub Docs
- Visual Studio Code 1.146 release notes, Migrate customizations — Visual Studio Code

**Microsoft**

- Microsoft Ignite 2026, Introducing MAI Code 2 and MAI Code 2 Flash — Microsoft AI
- Microsoft Copilot SDK 2.0, Microsoft-Decision-1.1 general availability — Microsoft Command Line
- Ignite 2026 live news — Microsoft
- microsoft/copilot-sdk, SDK v2 breaking changes — Microsoft on GitHub

**Benchmark leaderboard**

- Terminal-Bench 4.1 leaderboard — Stanford, Harbor and the Laude Institute
