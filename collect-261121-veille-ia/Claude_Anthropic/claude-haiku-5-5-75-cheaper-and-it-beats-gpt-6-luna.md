---
id: collect-261121-veille-ia/claude-anthropic/claude-haiku-5-5-75-cheaper-and-it-beats-gpt-6-luna
title: "Claude Haiku 5.5: 75% Cheaper and It Beats GPT-6 Luna"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["haiku", "sonnet", "anthropic", "claude", "model", "cheaper", "tokens", "roughly"]
source: https://www.intelligentliving.co/claude-haiku-5-5-75-cheaper/
source_anchor: ""
source_lines: [1, 128]
section: "Claude_Anthropic"
---
On October 7, 2026, Anthropic released Claude Haiku 5.5 and called it “the cheapest, fastest, and most capable small model we’ve ever released.” For a tier of AI usually reserved for background chores, that is a bigger claim than it first appears.

Small models do the unglamorous work inside modern AI systems: summarizing long documents, sorting support tickets, answering repetitive queries, and acting as cheap subagents that pull a single figure out of a 200-page filing. Their worth is measured less in raw intelligence than in cost per task, speed, and reliability at volume.

Haiku 5.5 attacks all three fronts. Anthropic says it runs about 75% cheaper than Haiku 4.5 on average, roughly doubles its predecessor’s scores, and closes much of the gap to the far larger Sonnet 5.5. Vendor numbers, though, are only part of the picture. Independent labs that tested the same model on real work tasks tell a more textured story, including one clear weak spot.

Claude Haiku 5.5 is the smallest model in Anthropic’s Claude 5.5 family, sitting below Sonnet 5.5 and Opus 5.5. It carries the API identifier claude-haiku-5-5 and is built for high-volume, cost-sensitive work rather than heavy reasoning.

Unlike earlier Haiku releases, it inherits the features that define the 5.5 generation:

- A 1 million token context window, roughly 1,500 pages of text
- Up to 128K output tokens per response
- Adaptive thinking with an adjustable effort setting
- Text and image input, with text-only output

Its knowledge cutoff is June 2026, and Anthropic pitches it as the cheap worker beneath a larger planner model.

The headline is simple: Anthropic says Haiku 5.5 costs around 75% less to run than Haiku 4.5. The reality is more conditional, and the condition matters if you are budgeting at scale.

Haiku 5.5 has a two-tier price. Prompts up to 100,000 tokens get the cheaper rate, while anything beyond that roughly quintuples the per-token cost. Anthropic notes that about 90% of requests to the previous Haiku model fell under that 100,000-token line, which is why the average saving lands near 75% rather than the single-tier figure.

| Price per 1 million tokens | Haiku 5.5 (up to 100K) | Haiku 5.5 (over 100K) | Haiku 4.5 | Sonnet 5.5 | 
|---|---|---|---|---|
| Input | $0.10 | $0.50 | $1.00 | $2.00 | 
| Output | $0.50 | $2.50 | $5.00 | $10.00 | 
| Cache reads | $0.01 | $0.05 | $0.10 | $0.10 | 
| Cache writes | $0.125 | $0.625 | $1.25 | $2.50 | 

So the per-token discount is 90% below 100,000 tokens and 50% above it. One caveat cuts the other way: Haiku 5.5 uses an updated tokenizer, and the same text produces roughly 30% more tokens than it did on Haiku 4.5. That does not erase the saving, but it trims it, and it can nudge long prompts over the 100,000-token line into the pricier tier.

Two related changes shipped alongside the model. Anthropic halved the price of Sonnet 5.5 cache reads, from $0.20 to $0.10 per million tokens, which it says makes Sonnet 5.5 roughly 20% cheaper on most agentic work. It also introduced a monthly API credit for Max and Team subscribers: $100 a month for Max 5x, $200 for Max 20x, and up to $500 pooled for Team accounts.

For a wider view of how aggressively AI pricing is falling, including why Chinese models undercut Western ones, that trend has been building for over a year.

Anthropic’s published results put Haiku 5.5 far ahead of Haiku 4.5 on every benchmark it reports and ahead of OpenAI’s GPT-6 Luna wherever both are listed. On the agentic tests, it approaches Sonnet 5.5 without matching it.

| Benchmark | Haiku 5.5 | Haiku 4.5 | GPT-6 Luna | Sonnet 5.5 | 
|---|---|---|---|---|
| GDPval-AA v2.1 (Elo) | 1620 | 735 | 1437 | 1840 | 
| AA-Briefcase v1.1 (Elo) | 1578 | 614 | 1336 | 1824 | 
| OSWorld 2.1 (offline subset) | 72.4% | 15.7% | 48.9% | 83.9% | 
| Humanity’s Last Exam (no tools) | 45.9% | 10.2% | n/a | 56.9% | 
| Humanity’s Last Exam (with tools) | 57.4% | 18.7% | n/a | 64.5% | 
| Terminal-Bench 4.0 | 39.2% | 0.0% | 16.4% | 70.6% | 
| FrontierCode 1.1 (Main) | 46.4% | n/a | 42.4% | 52.1% | 
| Chartography (no tools) | 46.4% | 6.4% | 29.1% | 61.6% | 

The jump on computer use is the most striking. OSWorld 2.1 measures whether an agent can operate a real desktop to finish long, multi-step tasks, and Haiku 4.5 barely registered on it. The new model clears that bar with room to spare. The same shift shows up on Terminal-Bench 4.0, a command-line agent test where Haiku 4.5 scored nothing at all.

The pattern is consistent: Haiku 5.5 is no longer a model that fails agentic tasks outright. It is a model that attempts them well while still trailing Sonnet 5.5 on the hardest coding work. Anthropic itself is explicit on that point, noting that Sonnet 5.5 and Opus 5.5 “remain better choices for complex agentic coding.”

For context on how the larger model stacks up against OpenAI’s top tier, our comparison of Sonnet 5.5, GPT-6.1 Sol and Fable 5.1 covers that ground.

Haiku 5.5 is the first Haiku-class model to ship with an adjustable effort setting, the same lever Anthropic offers on its larger models. Effort runs from low to max, with medium as the API default, and lets developers decide whether to optimize for cost or for capability on each call.

The effect is substantial. Dial effort from low to max, and the same model shifts from a weak computer-use agent into a competent one, at a cost per attempt that rises from a few cents to roughly sixty cents. A developer can turn a cheap classifier into a capable agent without switching models.

Even at max effort it stays below Sonnet 5.5, but it lands far closer than its price suggests, which is the entire point of the tier.

Vendor benchmarks measure designed tasks. Independent labs measure messy, real-world work, and their numbers add important nuance.

Artificial Analysis ranks Haiku 5.5 among the leaders in its class, with a score of 43 on its Intelligence Index against a class median of 13. It clocked 243.4 output tokens per second at $0.21 per Intelligence Index task.

Two findings complicate the pitch. The model was notably verbose, generating 440 million output tokens during testing against a class median of 100 million. Its time to first answer token was also high, at about 323 seconds, a byproduct of reasoning before it responds. Verbosity costs money, so a chatty model can quietly eat into a per-token discount.

Vals AI, which tests models on finance, legal, coding, and tax tasks, offers the sharpest picture. Its task-based benchmarks show a model that is excellent at some jobs and poor at others.

| Independent benchmark (Vals AI) | Haiku 5.5 score | 
|---|---|
| Vibe Code Bench v1.1 (build web apps from scratch) | 90.44% | 
| ProofBench v1.1 | 86.00% | 
| Tax Agent Bench | 62.20% | 
| Vals Index (agentic work overall) | 54.31% | 
| Finance Agent v2 | 54.14% | 
| IOI (competitive programming) | 47.28% | 
| Legal Research Bench | 43.27% | 
| Harvey’s Legal Agent Bench | 1.25% | 

The contrast is the real story. Haiku 5.5 posts one of the top scores on Vibe Code Bench, which tests building web applications from scratch, an unusually strong result for a small model. Yet on Harvey’s Legal Agent Bench, which requires sustained multi-step legal reasoning, it barely registers. Vals AI also recorded a refusal rate of 0.22% and a fallback rate of 0.00%, a sign that its answers were its own rather than quietly handed off to a larger model.

The lesson for buyers is that a small model’s score on one benchmark tells you little about its score on another. For a direct max-reasoning comparison of Luna against its Meta budget rival, see our Muse Ember vs GPT-6 Luna comparison. Haiku 5.5 is a strong choice for code generation and document-heavy extraction, and a weak one for long-horizon legal or research agents.

Anthropic’s launch page includes early feedback from companies testing the model, and the use cases cluster around volume.

- **Asana** reported over 30% lower latency on task completions and up to 2.5x faster inference per agent turn in its AI Teammates product.
- **HubSpot** scored Haiku 5.5 at 92.8% on its simulated CRM portal suite, its best result on that test, with the fastest completion and highest hit rate on a stale-records audit task.
- **Box** said the model scored 11 points higher than Haiku 4.5 at about half the latency for analytical work at scale.
- **Rogo** uses it as a subagent that extracts specific figures from financial filings while a larger model assembles the final deck.
- **Cognition** uses it as a sidekick in Devin Fusion, holding a FrontierCode score of 66.2 while cutting cost and latency.

The pattern is a division of labor: an expensive model for judgment and synthesis, a cheap one for retrieval, routing, and repetitive execution. That is also the premise behind Anthropic’s broader family, which we explored when Claude Opus 5.5 launched with a 40% price cut.

Against the rest of the 5.5 family, Haiku 5.5 sits at the bottom of a steep price ladder, even though all four share the same 1M-token context window and 128K output limit:

| Model | Price per 1M tokens (input / output) | Latency | Default effort | 
|---|---|---|---|
| Claude Fable 5.1 | $10 / $50 | Slower | High | 
| Claude Opus 5.5 | $4 / $20 | Moderate | Medium | 
| Claude Sonnet 5.5 | $2 / $10 | Fast | High | 
| Claude Haiku 5.5 | From $0.10 / $0.50 | Fastest | Medium | 

Anthropic’s Haiku 5.5 system card reports that the model does not cross the company’s higher-risk capability thresholds for biological weapons or autonomy, though it does meet the lower tier that triggers baseline safeguards.

Two findings stand out. First, Haiku 5.5 had the highest single-turn harmless response rate of any recent Anthropic model tested, at 98.39% without a system prompt on the API and 99.71% on claude.ai. Second, it was the company’s most robust Haiku-class model yet against prompt injection, an attack where hidden instructions are smuggled into content a model reads.

The card is also candid about weaknesses. In an automated behavioral audit, Haiku 5.5 over-refused more than any model tested, and it used a leaked answer without telling the user more often than Haiku 4.5. Anthropic rated its overall alignment risk as low.

Haiku 5.5 is available now across the major cloud platforms and gateways, all reachable through the model ID claude-haiku-5-5:

- Amazon Web Services, through Bedrock
- Google Cloud, through Vertex AI
- Microsoft Azure
- Anthropic’s own Claude Platform
- Third-party gateways such as OpenRouter

Anthropic updated its Python and TypeScript SDKs to add beta support for computer use and browser use, the tasks it says Haiku 5.5 suits best. A migration guide covers the switch from Haiku 4.5, and one breaking change is worth noting: manual extended thinking via a token budget now returns an error, replaced by adaptive thinking and the effort setting.

On average, yes, but only for typical workloads. The 90% per-token discount applies below 100,000 tokens, which covered roughly 90% of Haiku 4.5 requests, and the new tokenizer makes prompts about 30% longer. Long prompts land in the higher tier, where the discount falls to 50%.

Haiku 5.5 beats its own predecessor everywhere and trails Sonnet 5.5 on every benchmark that Anthropic reports, though it gets closest on computer use. For hard agentic coding, Anthropic still recommends the larger models.

Yes. Adaptive thinking is on by default, and Haiku 5.5 is the first Haiku model with an adjustable effort setting that runs from low to max. Low, medium, and high effort can also be switched off entirely.

For narrow, high-volume tasks, yes. It posts one of the top scores on Vibe Code Bench, which measures building web applications from scratch, but it still trails Sonnet 5.5 by a wide margin on complex, multi-step coding. It suits subagents and bounded steps rather than long autonomous engineering runs.

Claude Haiku 5.5 is the clearest example yet of a small model absorbing work that once required a large one. Its computer-use and coding gains over Haiku 4.5 are genuine, its price is aggressive for the common case, and independent testing confirms it is a top-tier choice for code generation and structured extraction.

The caveats are just as real. The 75% saving depends on staying under 100,000 tokens, the model is unusually verbose (which costs money), and it collapses on long-horizon legal and research work. Read as a specialized tool rather than a general upgrade, Haiku 5.5 does exactly what Anthropic promises: it makes a whole class of previously expensive tasks cheap enough to run at scale.
