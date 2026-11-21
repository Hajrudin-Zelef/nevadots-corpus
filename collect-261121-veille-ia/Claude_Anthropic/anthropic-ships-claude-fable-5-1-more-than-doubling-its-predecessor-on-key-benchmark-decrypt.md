---
id: collect-261121-veille-ia/claude-anthropic/anthropic-ships-claude-fable-5-1-more-than-doubling-its-predecessor-on-key-benchmark-decrypt
title: "Anthropic Ships Claude Fable 5.1, More Than Doubling Its Predecessor on Key Benchmark - Decrypt"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["fable", "opus", "anthropic", "claude", "model", "mythos", "benchmark", "scored"]
source: https://decrypt.co/377078/anthropic-claude-fable-5-1?amp=1
source_anchor: ""
source_lines: [1, 38]
section: "Claude_Anthropic"
---
Anthropic released Claude Fable 5.1 and Claude Mythos 5.1 on Tuesday, its first update to the Mythos-class model line since Fable 5 launched on June 9.

The company claims new AI models are the world's “most advanced for coding and knowledge work,” and it lands three months into a launch cycle that has already included an 18-day export-control shutdown, a mid-summer pricing fight over subscription access, and a July release of Claude Opus 5 that undercut Fable 5 on price.


Myriad: When will OpenAI release GPT-6? Click to make your prediction.

Fable 5.1 and Mythos 5.1 are, per Anthropic, the same underlying model with different safety filters bolted on. Fable 5.1 is generally available to anyone with a Claude account. Mythos 5.1 stays restricted to vetted cybersecurity and life-sciences professionals through Anthropic's Cyber Verification Program and Life Sciences Verification Program, the successor to the Project Glasswing access track that gated Mythos 5.

### **What the benchmarks actually measure**

Anthropic's headline number comes from Terminal-Bench-Science 0.1, a benchmark that tests whether an AI agent can carry out scientific research tasks inside a command-line environment, scored as a pass rate.

Fable 5.1 hit 52.6% against Fable 5's 24.7% and Opus 5's 29.0%, more than double its predecessor.

Terminal-Bench 4.0 tests agentic coding done through a terminal—writing, running, and debugging code across multi-step command-line sessions, scored as a percentage of tasks completed correctly. Fable 5.1 scored 55.8%, up from Fable 5's 42.0% and ahead of Opus 5's 52.3%.

Mythos 5.1, running with lighter cybersecurity filters, scored 60.9% on the same test; Anthropic says the gap reflects tasks its safeguards intercepted and rerouted to Opus 4.8.


On Humanity's Last Exam, a multidisciplinary reasoning test built from expert-level questions across dozens of academic fields and scored as a pass rate, Fable 5.1 reached 60.9% without external tools and 65.0% with them, both ahead of Opus 5, showing this is Anthropic’s most advanced model for academic purposes.


### **Where it beats Opus 5, and where the math gets murkier**

Opus 5 launched in July costing half of Fable 5's per-token price while outscoring Fable 5 on most major benchmarks, effectively making the flagship model redundant for most paying users.

Fable 5.1 reverses that: It now beats Opus 5 on every benchmark Anthropic published, including the ones where Opus 5 had previously beaten Fable 5.

But Fable 5.1 still costs twice as much per token as Opus 5—$10 input and $50 output versus Opus 5's $5 and $25. Tokens are the basic amount of information a model can handle (usually around two thirds of an average English word), and companies pay for use because heavy workflows usually deplete the quotas set in subscription plans very quickly.

Anthropic's own pitch for the model leans on effort levels rather than raw scores: it says that running Fable 5.1 at low or medium reasoning effort matches or beats Fable 5's old results at much lower cost, while reserving the top effort tiers for problems that stump everything else.

For routine work, that argument for skipping Opus 5 entirely gets weaker the lower the effort dial goes.

Access follows the same split that applied to Fable 5 after months of Anthropic adjusting the terms. On Pro plans and standard Team or Enterprise seats, Fable 5.1 draws entirely from pay-as-you-go usage credits billed at the API rate, since it isn't part of those plans' weekly limits. On Max plans and premium Team or Enterprise seats, it's included as a standard part of the subscription for up to 50% of weekly usage.

Claude Fable 5.1 is available now on the Claude API, Amazon Bedrock, Google Cloud, and Microsoft Foundry, under the model ID "claude-fable-5-1."
