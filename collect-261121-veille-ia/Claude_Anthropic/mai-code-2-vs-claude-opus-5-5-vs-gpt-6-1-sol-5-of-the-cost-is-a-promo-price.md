---
id: collect-261121-veille-ia/claude-anthropic/mai-code-2-vs-claude-opus-5-5-vs-gpt-6-1-sol-5-of-the-cost-is-a-promo-price
title: "MAI Code 2 vs Claude Opus 5.5 vs GPT-6.1 Sol: '5% of the Cost' Is a Promo Price"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["code", "opus", "cost", "price", "gpt-6", "microsoft", "claude", "card"]
source: https://root.benhalpern.com/alanwest/mai-code-2-vs-claude-opus-55-vs-gpt-61-sol-5-of-the-cost-is-a-promo-price-4o4b
source_anchor: ""
source_lines: [1, 78]
section: "Claude_Anthropic"
---
Microsoft says MAI Code 2 gives you 96% of Claude Opus 5.5's coding performance at 5% of the cost, and 98% of GPT-6.1 Sol at 40%. The footnote says the MAI prices behind those numbers are a 50% promotional rate. The benchmark table in the model card shows MAI's own scores and not one competitor score next to them. And if you run Microsoft's own GPT-6.1 Sol figure against GitHub's price list, MAI Code 2 comes out using more tokens than Sol.

I spent an evening with the announcement, the 22-page model card, GitHub Copilot's pricing table and the Hacker News thread. I ran nothing myself. This post collects the official numbers and a few early user reports, then builds the MAI Code 2 vs Claude Opus 5.5 vs GPT-6.1 Sol table Microsoft didn't publish.

Two models, both trained from scratch by Microsoft:

- MAI Code 2, the big one, for "deep debugging, multi-file implementation, refactoring, and repository-wide reasoning" per the GitHub changelog.
- MAI Code 2 Flash, the cheap one, for explanations, targeted edits and routine fixes.

The card describes sparse Mixture-of-Experts transformers with a vision encoder, a 1,050,000-token context (922,000 input, 128,000 output), five reasoning levels (low, medium, high, xhigh, max), English-only optimization and an August 2026 knowledge cutoff. Weights aren't released. Flash was distilled from a larger in-house teacher model.

The card can't settle on a parameter count, though. The summary table says MAI Code 2 is "50B-100B (80B active)". On the next page it's "77 billion active" and "approximately 2.8 trillion total parameters". Flash is "5B-15B (12B active)" in one place and "approximately 398 billion total" in another. It reads like two drafts got merged.

Prices are GitHub Copilot's per-1M-token rates. Benchmarks are pass rates as each vendor reported them. A dash means no published number.

|  | MAI Code 2 | MAI Code 2 Flash | Claude Opus 5.5 | GPT-6.1 Sol | 
|---|---|---|---|---|
| Input / output (now) | $0.50 / $2.50 | $0.05 / $0.25 | $4.00 / $20.00 | $2.00 / $10.00 | 
| Input / output (from May 17, 2027) | $1.00 / $5.00 | $0.10 / $0.50 | same | same | 
| Cached input (now) | $0.025 | $0.005 | $0.20 | $0.10 | 
| Context | 1,050,000 | 1,050,000 | not stated in launch post | 1M; $4.00 / $15.00 past 272K | 
| Terminal-Bench 2.1 | 84.6% | 78.3% | — | — | 
| Terminal-Bench 4.0 | — | — | 66.4% (xhigh) | — | 
| DeepSWE 1.1 | 73.2% | 59.9% | — | — | 
| SWE Atlas | 43.1% | 44.8% | — | — | 
| FrontierCode v1.1 | — | — | 54.4% (Main) | — | 

Sources: MAI rows from the model card and GitHub pricing; Opus 5.5 from Anthropic's launch post; Sol context from Artificial Analysis.

Count the dashes. There's not a single row where MAI Code 2 and Opus 5.5 have published scores on the same benchmark version. Microsoft reports Terminal-Bench 2.1. Anthropic reports Terminal-Bench 4.0. The "96% of Opus" claim is an average over four benchmarks (DeepSWE 1.1, Terminal Bench 2.1, SWE-Atlas, FrontierCode v1.1), and the per-model numbers behind that average never show up anywhere.

Microsoft did run the rivals. The card says every model in a comparison went through the same harness (MSBench, GitHub Copilot CLI 1.0.102, subagents off, max effort, three runs averaged), and the launch post says the FrontierCode numbers come from Artificial Analysis. That's a reasonable setup. Microsoft just didn't publish what came out of it.

This is where I get annoyed. The launch post's footnotes say MAI costs are shown at the "50% promotional price." The promo ends May 17, 2027. Double the MAI side and the Sol claim turns into "98% of Sol for 80% of the price," which is a much duller headline. All four claims at list price:

| Claim in the launch post | At promo price | At list price | 
|---|---|---|
| MAI Code 2 vs GPT 6.1 Sol | 98% performance, 40% cost | 98% performance, ~80% cost | 
| MAI Code 2 vs Opus 5.5 | 96% performance, 5% cost | 96% performance, ~10% cost | 
| Flash vs GPT-6 Luna | comparable, 65% cost | comparable, ~130% cost | 
| Flash vs Claude Haiku 5.5 | comparable, 20% cost | comparable, ~40% cost | 

You can back out token usage from those ratios. Total cost is price per token times tokens used. At promo pricing MAI Code 2 is $0.50/$2.50 against Sol's $2.00/$10.00, so a quarter of the price per token. If the total bill is 40% of Sol's, MAI Code 2 used roughly 1.6x as many tokens to get there. Same exercise for Flash: half of Luna's per-token price, 65% of the cost, so about 1.3x Luna's tokens. The arithmetic is mine and assumes a similar input/output mix, but I can't find another reading where the published numbers add up.

Opus goes the other way. At $4/$20, Opus 5.5 is 8x MAI Code 2's promo price per token, and a 5% bill implies MAI Code 2 used about 40% of Opus's tokens. That's the one comparison that survives the doubling: Opus is still 4x MAI Code 2's list price per token. If MAI Code 2 really lands near Opus on your repo, that's a real saving.

Flash after May 2027 costs exactly what GPT-6 Luna and Claude Haiku 5.5 cost per token under their long-context thresholds: $0.10 in, $0.50 out. With 1.3x the tokens, it ends up pricier than Luna on Microsoft's own workload mix.

The rivals aren't current. The comparison uses GPT-6.1 Sol. GitHub already lists GPT-6.2 Sol at the same $2/$10, and Artificial Analysis marks 6.1 Sol as deprecated and recommends 6.2. Commenter oofbaroomf pointed out the 6.1 comparison in the thread.

The benchmark versions are behind too. Anthropic quotes Terminal-Bench 4.0 for Opus 5.5; Microsoft used 2.1. HN user johnfn questioned the benchmark picks and said DeepSWE 1.1 has been "busted for a while."

There's nothing from outside the US. No DeepSeek, Qwen, GLM or Kimi. "This trend of American models only comparing to other American models is pretty interesting," wrote okasaki. spwa4 built their own table and concluded both MAI models are "significantly worse than Qwen3.8-Max and Qwen4-Max," while admitting it's "not a controlled comparison."

The card doesn't agree with itself either. The quality section has MAI Code 2 at 73.2% on DeepSWE 1.1 and 43.1% on SWE Atlas. The autonomy-risk section of the same card shows DeepSWE 1.1 at 60.2% and SWE Atlas at 80.9%. There's no note explaining the gap. Also, Flash beats the big model on SWE Atlas in the headline table (44.8% vs 43.1%).

There's also no technical report. You get a model card and two data cards, and that's all.

The card does contain one paragraph I wish were in the launch post. Microsoft's red team rates both models "similar in alignment to GPT-5.6 Sol but less aligned than GPT-6," and reports "repeated instances of the model circumventing safeguards, misleading supervisors, interfering with human oversight" in agentic scenarios. The safeguarded configuration gave "limited improvement," though the card also says the models generally respected shutdown boundaries. If you run Copilot's cloud agent with write access to anything important, read section 3.2.6.4 before you turn it on.

Not much yet. chopete3 says MAI Code 2 in Copilot fixed tricky build and Azure Bicep problems: "Didn't feel the difference from Claude Opus." LaurensBER's team cancelled Copilot after most people "burned through" their credits "in days, some in hours," calling GitHub's AI integration "just not good enough at the moment"; they plan to try the new models but say cheap ones still don't compare to the "nearly unlimited usage" of Claude Code and Codex. And Microsoft's own launch post says the models are "explicitly not yet tuned" for very long-horizon agentic work, which is exactly where I'd reach for Opus.

- GitHub Copilot: MAI Code 2 on Pro, Pro+, Max, Business and Enterprise. Flash on every plan including Free and Student. The changelog limits both to "eligible GitHub Copilot subscribers on annual plans." They show up in the model picker in VS Code, Visual Studio, JetBrains, Xcode, Eclipse, Copilot CLI, the Copilot app, the cloud agent, github.com and GitHub Mobile.
- Microsoft Foundry: Flash only, limited-access preview.
- OpenRouter, Vercel, Kilo, Open Code, Hermes: Microsoft says "soon," and the sentence reads as if it means Flash.

The big model has no API yet, so for now it's Copilot or nothing.

1. Check your default model policy. Under default model enablement, new models turn on automatically unless you've disabled the global default or the model itself. MAI Code 2 is probably already live for your org.
2. Check "AI credits paid usage." It's on by default for Business and Enterprise. A Business seat adds 1,900 credits ($19) a month to a shared pool. At promo prices that's 7.6M MAI Code 2 output tokens versus 950K from Opus 5.5.
3. Grep for `MAI-Code-1.1-Flash` . It retires on December 18, 2026. Scripts, custom agents or workflows pinning that model string will break.
4. Put May 17, 2027 in the calendar. That's when your MAI spend doubles.

My take: for the size of task Copilot handles all day (explain this, fix that test, rename across five files), MAI Code 2 at promo pricing is worth trying, and Flash is a fine default for cheap seats. For a long refactor on a codebase I care about, I'm keeping Opus until someone publishes MAI Code 2 and Opus 5.5 scores from the same harness on Terminal-Bench 4.0. Microsoft has that harness and chose not to publish the comparison.

If you've already pointed MAI Code 2 at a real repo: did it actually feel like Opus, and did your token count look closer to 1.6x Sol's than to the price chart? Tell me what broke.

## Top comments (0)
