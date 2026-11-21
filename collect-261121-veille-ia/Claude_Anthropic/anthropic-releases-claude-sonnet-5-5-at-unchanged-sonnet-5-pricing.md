---
id: collect-261121-veille-ia/claude-anthropic/anthropic-releases-claude-sonnet-5-5-at-unchanged-sonnet-5-pricing
title: "Anthropic Releases Claude Sonnet 5.5 at Unchanged Sonnet 5 Pricing"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-09-28"]
keywords: ["sonnet", "anthropic", "claude", "opus", "output", "pricing", "tokens", "model"]
source: https://www.unite.ai/anthropic-releases-claude-sonnet-5-5-at-unchanged-sonnet-5-pricing/
source_anchor: ""
source_lines: [1, 47]
section: "Claude_Anthropic"
---
## Key PointsSonnet 5.5 matches Sonnet 5 pricing while Anthropic claims over 30% faster output and…

View more

- Sonnet 5.5 matches Sonnet 5 pricing while Anthropic claims over 30% faster output and up to 30% lower cost per task.
- Anthropic gave Sonnet 5.5 the cyber safeguards and fallbacks previously reserved for its most capable models.
- Early testers report Sonnet 5.5 matches stronger models while spending far fewer output tokens on real workloads.

Anthropic released Claude Sonnet 5.5 on September 28, 2026, the second model in its Claude 5.5 family. The model keeps Claude Sonnet 5’s pricing at $2 per million input tokens and $10 per million output tokens, and Anthropic says it generates output more than 30% faster while costing up to 30% less per task in its testing.

In its release announcement, Anthropic described Sonnet 5.5 as a faster, lower-cost complement to Claude Opus 5.5, which the company says is built for complex work requiring careful judgment. Sonnet 5.5 is strongest at well-scoped everyday tasks, fixing bugs, and producing polished documents, slides, and spreadsheets, Anthropic said, adding that it also has a sharp eye for design.

Claude Sonnet 5.5 is available on all platforms, including Amazon Web Services, Google Cloud, and Microsoft Azure, under the model ID claude-sonnet-5-5, and is offered with zero data retention, as with Opus 5.5 and Sonnet 5.

## Reported Benchmark Results

Anthropic reports that Sonnet 5.5 scores 70.6% on Terminal-Bench 4.0, an agentic coding evaluation, against Sonnet 5’s 10.3%, with Opus 5.5’s listed score of 66.4% reflecting its Xhigh-effort result. On FrontierCode 1.1’s main set, Sonnet 5.5 posts 46.2% at Max effort and 52.1% at Xhigh, compared with 42.4% for Sonnet 5, 54.4% for Opus 5.5, and 49.3% for OpenAI’s GPT-6 Sol. The reported CursorBench 4.0 scores are 55.5% for Sonnet 5.5, 34.1% for Sonnet 5, and 57.8% for Opus 5.5.

On knowledge-work evaluations, the company reports a GDPval-AA v2.1 score of 1844 for Sonnet 5.5, two points below Opus 5.5’s 1846 and about 400 points above Sonnet 5’s 1449, and an AA-Briefcase v1.1 score of 1811 against 1822 for Opus 5.5 and 1359 for Sonnet 5. The announcement also lists 64.5% with tools on Humanity’s Last Exam, 80.1% partial credit on OSWorld 2.1, and 61.6% without tools on Chartography, a visual chart recognition test. Anthropic says Sonnet 5.5 is the first Sonnet model to beat Pokémon Red working only from screenshots.

The company cautions that benchmark scores capture only one facet of a model’s capabilities, and says that in its own testing and that of external testers, Opus 5.5 remains clearly stronger at complex, open-ended work requiring sustained judgment. A footnote states that Artificial Analysis ran GDPval-AA and AA-Briefcase on a pre-release deployment with a structured-outputs bug that has been fixed and would, if anything, understate Sonnet 5.5’s performance. Another footnote notes that OpenAI recently fixed an image-understanding bug in GPT-6 Sol that third-party scores may not yet reflect.

## Early Tester Results

CodeRabbit’s vice president of AI, David Loker, said Sonnet 5.5 shows better judgment than Sonnet 5 across complexity levels while spending significantly fewer output tokens, and that CodeRabbit plans to move simple and moderate reviews to the model now, with more in the coming weeks. Base44 AI engineering lead Gabriel Grinberg reported that across 118 real app builds, Sonnet 5.5 averaged 3.6 iterations per build against Opus 5’s 7.7, with the fewest failed tool calls of any model Base44 compared.

Slack principal engineer Curtis Allen reported roughly 14% fewer output tokens on offline Slackbot evaluations with no prompt changes. Zendesk’s director of AI, Abhinay Kathuria, said tickets were processed 20% faster than with the Claude models Zendesk runs in production. Balyasny Asset Management reported about 121,000 tokens per answer versus Sonnet 5’s 497,000 across a private suite of 2,441 finance tasks. Box reported results 2.4 times faster with 12% fewer total tokens, and Atlassian said customers can run Rovo Agents up to 30% faster than with Sonnet 5.

## Pricing, Speed, and Migration

Sonnet 5.5’s listed prices match Sonnet 5 exactly: $2 per million input tokens, $10 per million output tokens, $0.20 per million cache-read tokens, and $2.50 per million cache-write tokens. Opus 5.5 lists at $4 input, $20 output, and $5 cache writes. Anthropic says Sonnet 5.5 typically needs far fewer tokens for the same work and is its fastest Sonnet model to date.

The model offers five recalibrated effort levels: low, medium, high, xhigh, and max. Defaults are Medium in Claude Code and the Claude apps and High on the Claude Platform, which is also the API default.

Anthropic’s migration guide lists breaking changes for developers moving from Sonnet 5. Adaptive thinking now runs by default, the disabled thinking setting returns a 400 error, and developers who ran Sonnet with thinking off must switch to the new `between_tools` setting, the lowest available, before migrating. Forced tool choice is rejected in favor of automatic tool choice paired with strict tools, and the minimum cacheable prompt drops to 512 tokens from 1,024 on Sonnet 5. The documentation also lists five refusal stop-reason categories, covering cyber, bio, `frontier_llm`, `reasoning_extraction`, and `general_harms`, and notes that server-side fallback retries only cyber and frontier_llm declines on Sonnet 5.

## Safety Findings and Safeguards

Because Sonnet 5.5’s cyber capabilities are comparable to Opus 5’s, Anthropic said, it is the first Sonnet model to launch with the cyber safeguards and fallbacks used on the company’s most capable models. The system card reports that with safeguards off, Sonnet 5.5 averaged 11.53 capability flags and an 80% capture rate on ExploitBench and produced 178 full arbitrary-code-execution exploits, against one for Sonnet 5. It completed 46.1% of CyScenarioBench challenges versus Sonnet 5’s 0.7%, and produced 50 control-flow hijacks on the Binary Exploitation Benchmark versus Sonnet 5’s 3.

The cyber safeguards run in three stages: a probe on the model’s internal activations, a lightweight on-model classifier, and a separate trained LLM classifier. The card reports 99.43% recall on the cyber harm coverage set and a 21.0% attack success rate on the rewind-attacker robustness evaluation, improved from 57.2% for Sonnet 5, while warning users to expect increased refusals even on benign cybersecurity tasks. Higher-risk requests visibly fall back to Sonnet 5, and an expanded Cyber Verification Program will soon offer vetted defenders tiered access to advanced capabilities on Sonnet 5.5, Opus 5.5, and Claude Mythos models.

Biology safeguards are the same harmful-misuse classifiers deployed for Opus 5, with no fallback model, and Anthropic’s Life Sciences Verification Program covers access for the full breadth of biology work. Sonnet 5.5 is also the first Sonnet model with safety classifiers that block reasoning extraction, and it expands preserved thinking so a model’s thinking cannot be decoupled from the account that created it.

On Anthropic’s automated behavioral audit of roughly 1,850 scenarios, the company reports that Sonnet 5.5 improves on or matches Sonnet 5 on most measures of alignment, misuse resistance, and honesty, records the lowest rate of attempted sandbox escapes among tested models, and shows more illegible thinking than many previous models. The system card states that Sonnet 5.5 crosses no new Responsible Scaling Policy thresholds, is treated as meeting the policy’s CB-1 and Autonomy-1 thresholds, and carries a low assessed risk of catastrophic misalignment. The model’s reliable knowledge cutoff is June 2026.

Anthropic said Claude Haiku 5.5, built for high-volume and cost-sensitive applications, will join the Claude 5.5 family in the coming weeks.
