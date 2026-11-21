---
id: collect-261121-veille-ia/grok-xai/2026-08-12-spacexai-releases-grok-4-6-73a49b7e
title: "Spacexai releases grok 4 6"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: ["2026-08-12"]
keywords: ["grok", "spacexai", "model", "releases", "work", "engineering", "training", "knowledge"]
source: ""
source_anchor: ""
source_lines: [1, 34]
section: "Grok_xAI"
---
SpaceXAI just released Grok 4.6. The release is a post-training upgrade over Grok 4.5 rather than a larger base model. SpaceXAI held the foundation constant and spent the improvement on a longer supplemental training run, regenerated supervised fine-tuning trajectories, and reinforcement learning in agentic environments. Agents that stay on a task across many steps without drifting. Grok 4.6 scores 61 on the Artificial Analysis Intelligence Index, up five points from Grok 4.5 and tied with GPT-5.6 Sol Max. The model takes 500,000 context tokens, is live today in Cursor and Grok Build, and adds a new xhigh reasoning-effort level above the ladder Grok 4.5 shipped with.
Is it deployable?
Yes, in production, with a bounded set of workloads. The model is generally available through the xAI API as grok-4.6, is the default model in Grok Build, ships in Cursor on all plans, and is routable via OpenRouter, Vercel, and Cloudflare. There is no open-weights release and no self-hosting path, so air-gapped deployments are out.
▲ AI DEV SIGNALS · FREE NEWSLETTER
Get the AI releases that matter, before they trend.
Model releases, open weights, agents and papers with code — the 5 that matter, in 1 email. Free.
- Company stage: Seed-stage teams and indie developers can adopt it immediately, since Cursor and Grok Build need no harness work. Mid-market engineering orgs are the strongest fit: API-only integration, mTLS authentication, batch and priority processing are documented. Regulated enterprises should stage a pilot first — the vendor’s brand history is a live procurement question in several buying committees.
- Industries: Software and developer tooling, semiconductor and kernel engineering, hardware and CAD-adjacent design, financial research, and legal analysis. The training mix explicitly targeted several of these.
- Applications: Repository-wide refactors, migration agents, research-and-synthesis pipelines over 500K-token corpora, first-pass application scaffolding from a product brief, GPU kernel optimization, and document-heavy knowledge work.
What actually changed
Grok 4.6 is not a larger base model. SpaceXAI describes a longer supplemental training run than Grok 4.5 received, using curated model-generated data for reasoning and advanced technical concepts, high-quality engineering data, and an improved optimizer and training recipe.
Grok 4.5 was then used to regenerate supervised fine-tuning trajectories across reasoning-effort levels, agent harnesses, and domains spanning STEM, software engineering, and knowledge work, with problematic traces filtered by model-based checks. Reinforcement learning followed in agentic environments covering knowledge work, general coding, web development, computer-aided design, and kernel optimization.
The behavioral insight is an important one: on longer trajectories, SpaceXAI reports more self-testing and verification, with the model checking its own work before moving on. That is a vendor observation from internal testing, not an independently measured result.
The model takes 500,000 context tokens, accepts text and image input with text-only output, has no stated text output limit, and carries a February 1, 2026 knowledge cutoff. reasoning_effort now supports low, medium, high (default), and a new xhigh level. SpaceXAI did not publish a parameter count for Grok 4.6.
Benchmarks: read the losses first
On xAI’s launch table, Grok 4.6 (High) scores 61 on the Artificial Analysis Intelligence Index, up from 56 for Grok 4.5 and tied with GPT-5.6 Sol Max. It leads the table on GDPval-AA v2 (1753 Elo, versus 1526 for Grok 4.5), AA-Briefcase (1577, versus 1313), and Harvey LAB.
It trails on the coding rows that matter most to engineering teams. DeepSWE v1.1 lands at 65.9%, up 11.9 points generationally but behind GPT-5.6 Sol Max at 73%. Terminal-Bench v3.0 reaches 26%, nearly double Grok 4.5’s 15.7% and still last of the four listed models. CursorBench v3.2 is 69.9%, FrontierCode v1.1 Extended is 61.3%, and APEX-Agents is 57.5%.
Two things to note while evaluating. First, the table’s bolded wins on GDPval-AA v2 and AA-Briefcase sit inside Artificial Analysis‘ published confidence intervals — they are statistical ties, not leads. Second, the comparison set excludes Anthropic’s Claude Opus 5, which currently tops that index. The disclosed losses are the more reliable signal.
Pricing and access
Per the release notes, Grok 4.6 bills $2 / $0.50 / $6 per 1M tokens (input / cached input / output) below 200K prompt tokens, and $4 / $1 / $12 above that threshold. The launch page also references a faster variant at double the price, with no separate model ID published. Grok Build and Cursor are offering 2× included usage for the first week.
Teams should set a prompt_cache_key (or the x-grok-conv-id header on Chat Completions). Without it, requests scatter across servers and cache hits become unreliable, so full input price applies.
Interactive explainer
Key Takeaways
- Grok 4.6 is a post-training upgrade on Grok 4.5, not a bigger base model.
- 500K context, text and image input, and a new xhigh reasoning-effort level.
- Ties GPT-5.6 Sol Max at 61 on the AA Intelligence Index; trails on DeepSWE and Terminal-Bench.
- Same $2 / $6 headline price, with rates doubling above 200K prompt tokens.
- Available today via API, Cursor, and Grok Build — no open weights, no self-hosting.
Check out the FULL TECHNICAL DETAILS here. Also, feel free to follow us on Twitter and don’t forget to join our 150k+ML SubReddit and Subscribe to our Newsletter. Wait! are you on telegram? now you can join us on telegram as well.
Need to partner with us for promoting your GitHub Repo OR Hugging Face Page OR Product Release OR Webinar etc.? Connect with us
YOU READ TO THE END
Get the stories that didn’t make it on the homepage.
Last issue: ▲ what's inside: ai attackers, gemini 4 neon, daisyrose-27b, olala mini
Michal Sutter is a data science professional with a Master of Science in Data Science from the University of Padova. With a solid foundation in statistical analysis, machine learning, and data engineering, Michal excels at transforming complex datasets into actionable insights.
