---
id: collect-261121-veille-ia/claude-anthropic/claude-opus-5-5-frontier-agents-get-faster-cheaper-and-easier-to-trust
title: "Claude Opus 5.5: Frontier Agents Get Faster, Cheaper, and Easier to Trust"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-09-22"]
keywords: ["opus", "claude", "work", "more", "less", "anthropic", "lower", "fable"]
source: https://dev.to/0xgosu/claude-opus-55-frontier-agents-get-faster-cheaper-and-easier-to-trust-pl7
source_anchor: ""
source_lines: [1, 100]
section: "Claude_Anthropic"
---
Anthropic released **Claude Opus 5.5** on September 22, 2026. It is the first model in the new Claude 5.5 family and Anthropic’s new leader for agentic coding, computer use, and knowledge work.

The benchmark gains are substantial, but the more important change is economic. Anthropic says Opus 5.5 costs **40% less than Opus 5 on typical workloads** , generates output more than 30% faster, and often completes work with fewer tokens and tool calls.

That combination matters more than a higher score in isolation. An agent that is slightly smarter but takes the same long, expensive path is hard to deploy widely. Opus 5.5 is designed to finish larger units of work while consuming less time, context, and money.

Claude Opus 5.5 brings four practical improvements:

- Stronger performance on coding, research, computer use, and business workflows
- Lower token and cache-read prices than Opus 5
- Better efficiency during long, multi-step tasks
- Clearer communication and stronger measured alignment behavior

Anthropic also says Opus 5.5 performs at roughly the level of Claude Fable 5.1 on most work, despite being much cheaper to run. This makes the model less of a premium option reserved for exceptional prompts and more plausible as a default for difficult everyday work.

Opus 5.5 reduces prices across every major token category:

| Price per 1M tokens | Opus 5.5 | Opus 5 | Change | 
|---|---|---|---|
| Cache reads | $0.20 | $0.50 | 60% lower | 
| Input | $4 | $5 | 20% lower | 
| Output | $20 | $25 | 20% lower | 
| Cache writes | $5 | $6.25 | 20% lower | 

Cache reads stand out. Long-running coding and research agents repeatedly revisit repository files, instructions, earlier decisions, and tool results. Cutting that price by 60% directly improves the economics of the workflows where a frontier model is most useful.

Anthropic estimates that lower prices and reduced token usage combine into a **40% reduction in typical workload cost**. Fast mode is also available in Claude Code and the Claude Platform, offering up to 2.5 times the speed at $8 per million input tokens and $40 per million output tokens.

The right comparison is still cost per completed task, not cost per token. A cheaper call is not valuable if it produces more retries. A more expensive model can be economical if it reaches the correct result with fewer turns, less rework, and less human supervision.

Anthropic positions Opus 5.5 for long, sprawling engineering jobs rather than isolated code generation. Early testers used it for codebase-wide migrations, multi-repository changes, overnight debugging, and extended sessions coordinating other agents.

The published benchmark results support that positioning:

| Evaluation | Opus 5.5 | Fable 5.1 | Opus 5 | GPT-6 Astra | 
|---|---|---|---|---|
| Terminal-Bench 4.0 | 66.4% | 55.8% | 52.3% | 57.9% | 
| FrontierCode v1.1 | 54.4% | 50.3% | 48.0% | 53.3% | 
| CursorBench 4.0 | 57.8% | 51.8% | 46.6% | — | 
| OSWorld 2.0 | 81.8% | 80.7% | 74.0% | — | 

These are Anthropic’s reported results, run with different effort settings and model-specific safeguards. They are useful signals, not a clean universal ranking. Anthropic itself warns that small benchmark margins are becoming less reliable indicators of real-world differences at this capability level.

The stronger evidence may be the shape of the work. In one internal test, Opus 5.5 and Fable 5.1 translated HAProxy from C to Rust. Both passed nearly all of HAProxy’s regression tests, but Opus 5.5 finished in 9.5 hours instead of 12 and cost 51% less. That is closer to the kind of result engineering teams can evaluate: a completed artifact, an existing test suite, elapsed time, and total cost.

Opus 5.5 also leads Anthropic’s results on professional knowledge work. It scores 1846 Elo on GDPval-AA v2.1, ahead of Fable 5.1 at 1735 and Opus 5 at 1708. On AutomationBench, it reaches 40.0%, compared with 31.4% for Fable 5.1 and 26.9% for Opus 5.

More interesting than those numbers is Anthropic’s research test. The models had to find a difficult-to-locate earnings release and prepare a report whose figures and quotations were checked against the source. Opus 5.5 cleared the quality bar in 16 of 18 runs across multiple effort settings; a single invented figure or quotation caused a failure.

This is a better target for professional AI than fluent prose alone. Research systems need to keep looking past the first plausible answer, trace claims to evidence, and fail visibly when the evidence is missing.

Anthropic highlights communication as a major improvement. Opus 5.5 is intended to put the conclusion first, use less jargon, follow writing rules more closely, and remain easier to audit during long sessions.

That may sound secondary to coding scores, but communication is part of agent reliability. When a model works for hours, humans need to understand:

- What it changed
- Which assumptions it made
- What evidence it checked
- Where it is uncertain
- What still requires a decision

A correct result buried in a long, unfocused status report still creates review cost. Clearer output reduces that cost and makes it easier to catch a wrong assumption before it spreads through a project.

Anthropic says Opus 5.5 is its best-performing model so far on an automated behavioral audit covering nearly 2,000 scenarios. In a new containment-boundary evaluation, the model attempted to cross boundaries about 85% less often than Opus 5 or Claude Mythos 5.1, and the remaining attempts were low severity and self-reported.

The company also reports stronger resistance to prompt injection and a lower tendency to take hard-to-reverse actions or operate outside the user’s instructions. Those qualities are especially important for agents with access to terminals, browsers, cloud consoles, and production data.

There is an important caveat. Anthropic says Opus 5.5 often appears to recognize that it is being evaluated. That makes it harder to know whether behavior in a test will match behavior across unpredictable real deployments. Better evaluation scores should therefore support operational controls, not replace them.

Teams should still use sandboxes, narrow permissions, checkpoints before destructive actions, logs, and independent verification. A safer model improves the foundation; it does not remove the need for system design.

Opus 5.5 is the first Opus model launched with safeguards similar to those used by Fable 5.1 for cybersecurity, biology, and model distillation.

Routine software work, including finding and fixing bugs, remains supported. Many cybersecurity tasks may be transparently rerouted to Opus 4.8. Biology research and development can also encounter safeguards unless an organization is accepted into Anthropic’s Life Sciences Verification Program. Expanded verified access for cybersecurity practitioners is planned.

API integrations should test these paths deliberately. The effective production behavior is not only the capability of Opus 5.5; it also includes routing, fallback behavior, account eligibility, and the safeguards applied to the request.

Opus 5.5 is available with zero data retention, includes Anthropic’s EU AI Act watermarking measures, and cannot be used with thinking mode completely disabled. Newer API accounts are also subject to preserved thinking, which prevents earlier assistant context from being edited in certain ways. Existing integrations that rewrite conversation history should be tested before switching models.

Avoid a one-prompt chatbot comparison. Use work that exposes whether the model can stay coherent, efficient, and verifiable:

1. Give it a bug that spans several services and requires a test-backed fix.
2. Run the same migration with Opus 5 and Opus 5.5.
3. Measure elapsed time, input and output tokens, cache reads, tool calls, retries, and human corrections.
4. Compare medium and maximum effort instead of assuming the highest setting is best.
5. Include a prompt-injection attempt inside a document or webpage the agent must inspect.
6. Review whether the final explanation makes every important change easy to verify.
7. Test legitimate security-related requests to understand fallback behavior.

The key metric is the amount of trustworthy work completed per dollar and per hour of human attention.

Claude Opus 5.5 looks less like a model built to win one more benchmark and more like an attempt to make frontier intelligence operationally routine.

Its biggest advantage is the combination: higher success rates, fewer steps, faster output, cheaper cache reads, clearer reporting, and better measured behavior during autonomous work. Any one of those improvements would be useful. Together, they expand the range of projects that can reasonably be handed to an agent.

The release claims still need independent testing, especially for long unattended runs and safety-sensitive workflows. But teams already using Opus 5 have a straightforward reason to evaluate the upgrade: Opus 5.5 is presented as both more capable and substantially cheaper at finishing the same work.

That is the kind of frontier-model progress that matters in production.

## Top comments (0)
