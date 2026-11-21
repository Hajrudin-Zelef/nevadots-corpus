---
id: collect-261121-veille-ia/gpt-openai/gpt-6-2-astra-review-why-early-testers-are-disappointed
title: "GPT-6.2 Astra Review: Why Early Testers Are Disappointed"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["astra", "testers", "gpt-6", "early", "claude", "fable", "model", "scores"]
source: https://www.mindstudio.ai/blog/gpt-6-2-astra-early-reviews
source_anchor: ""
source_lines: [1, 94]
section: "GPT_OpenAI"
---
## What is GPT-6.2 Astra, and why are testers unhappy with it?

GPT-6.2 Astra is OpenAI’s new flagship model, released alongside a cheaper sibling called GPT-6.2 Soul. It posts strong benchmark numbers and costs noticeably less to run than its predecessor, but early testers, including Theo from T3, Matt Schumer, Dan Shipper’s team at Every, and Ethan Mollick, found it harder to steer in practice. It cuts off long tasks early, compresses its writing to the point of confusion, and asks questions the user can’t always answer. The complaint isn’t that it’s dumb. It’s that it’s annoying to work with.

## TL;DR

- **Astra is OpenAI’s top-tier model** and Soul is the cheaper everyday option, positioned against Anthropic’s Claude Opus 5.5 and Claude Fable 5.2 respectively.
- **Pricing dropped 20% from the previous Astra** , now $8 per million input tokens and $40 per million output tokens, matching what Anthropic charges for Fable 5.2.
- **The efficiency story is real** : on Artificial Analysis benchmarks, Astra scored 62 (versus Fable 5.2’s 65) while costing about $4 per task compared to Fable’s $26, because it uses far fewer tokens to get there.
- **Coding barely moved** , scoring 53 on Artificial Analysis inside Codex, the same as the previous Astra generation, while Soul actually dropped three points.
- **Named testers including Theo, Matt Schumer, Dan Shipper, and Ethan Mollick described it as uneven, hard to follow, or not worth switching to** from Claude, even though none called it unintelligent.
- **3D design, CAD modeling, and computer use are where Astra clearly stands out** , with demos showing full mechanical assemblies, RC buggy builds, and simulated physics that reviewers across multiple channels agreed were genuinely impressive.
- **OpenAI says Astra is the first public model to hit its “high threshold” for AI self-improvement** , a safety classification tied to how much the model accelerates OpenAI’s own research, not a claim about autonomous self-editing.

### Everyone else built a construction worker.

We built the contractor.

    One file at a time.

UI, API, database, deploy.

## How does Astra actually perform on benchmarks?

On Artificial Analysis’s combined intelligence index, Astra scores 62, a five-point jump over the prior Astra generation, putting it ahead of Gemini 4 Carbon and Claude Opus 5.5 but still behind Claude Fable 5.2 at 65. It also leads on science and 3D-specific tests, and one tester cited Astra hitting 100% on ARC-AGI 3 when paired with OpenAI’s own adapter, reportedly the first model to max out that benchmark.

The more interesting number is cost. Running the Artificial Analysis task suite cost about $4 per task on Astra versus $26 on Fable 5.2, a direct result of Astra needing far fewer tokens to reach a comparable answer. On the Pareto frontier chart that plots accuracy against cost, Astra sits close to the top left, meaning it holds scores around 77 to 80% at a fraction of Fable’s price on efficiency-focused tasks like Terminal Bench Science.

Coding tells a flatter story. Inside Codex, Astra scores 53, unchanged from the previous generation, while Soul dropped three points. Fable 5.2 inside Claude Code scores 58. So Astra is cheaper and roughly as capable at coding, but not better.

## Why do testers say it feels worse despite good scores?

The consistent complaint across independent testers is that Astra is harder to steer, not less intelligent. Theo described it stopping mid-task to ask questions he couldn’t answer and said its explanations were hard to follow, concluding he wouldn’t pick it over Opus 5.5. Matt Schumer said his experience was bad enough that he wouldn’t write a full review. Dan Shipper’s team at Every found the writing had regressed and that Astra cut off bigger builds too early. Ethan Mollick called it “uneven and weird,” noting it struggled with steering but could still deliver on large, ambitious projects when given enough room.

One likely explanation ties back to the efficiency pitch itself. If Astra is trained to produce the same quality of output using far fewer tokens, that same compression can show up as terser writing, premature stopping, and reasoning that’s harder for a human to trace. The model isn’t failing at the task, it’s optimizing for token economy in a way that changes how the interaction feels.

## Where does Astra actually win?

3D design and spatial reasoning are the one area where critics and fans agree. Demos included a playable racing game generated from a single prompt, an RC buggy modeled part by part from reference photos, a 3D-printable mechanical clock with every gear simulated in CAD and Python, and architectural reconstructions of real locations. One tester built a landing page showcasing a photorealistic glass of water with a custom ray tracer, generated from a one-word prompt in under a minute.

Remy AI Engineer Certification

Companies want to build their own software.

### Few people can build it right.

Become the one they trust.

- Access
- Audit trails
- Security
- Compliance

Astra was also named the new leader in AI Robotics benchmark version 0.1, ahead of Fable 5.2 and Gemini 4 Hydrogen, and testers noted sharper computer-use skills, down to manipulating individual parts and connections inside CAD and simulation tools. Independent game-clone tests (recreations of titles like MegaBonk) rated Astra’s visual output as strong, arguably better than Gemini’s Carbon model, though not quite matching Opus 5.5 in every category.

## What does “AI self-improvement” actually mean here?

OpenAI’s announcement states Astra is the first publicly released model to hit its “high threshold” for AI self-improvement. That doesn’t mean the model is rewriting its own code inside ChatGPT. OpenAI’s internal definition is narrower: the model helps its researchers about as much as adding one more skilled research engineer to the team. They tested this by having Astra debug failed experiments, write low-level chip code, and speed up training runs for smaller models, where it cut training time by roughly 43%, compared to a best human result of about 72%. So it’s helpful, but not yet beating top human researchers.

On a modified recursive self-improvement index, Astra scored 29%, behind Gemini 4 Argon, with 50% calibrated to represent human-level research performance. An outside evaluator, METR, concluded the model is unlikely to fully automate AI research on its own, but does give researchers a meaningfully bigger boost than prior models.

OpenAI treats this capability as a risk category because faster internal research could mean new models shipping faster than safety review can keep pace. In response, the company built what it calls a centralized control system with a kill switch (reported as acting within 35 seconds of detecting misalignment) and infrastructure to halt inference if safety monitoring goes down. Notably, Anthropic made the opposite call with Fable 5.2, reportedly holding back capabilities specifically in this self-improvement category rather than shipping them with added monitoring.

## Is GPT-6.2 Astra worth switching to?

For most existing Claude users, testers suggest the answer is no. Multiple people with early access to both models said they’d stick with Fable 5.2 or Opus 5.5 for daily work, citing Astra’s compressed writing style and tendency to stop short on longer builds. Fable 5.2 still leads on the general Artificial Analysis intelligence index, and coding performance between the two is close enough that price becomes the deciding factor for many teams.

Where Astra makes a stronger case is cost-sensitive, high-volume use: tasks involving 3D design, scientific modeling, CAD work, or large batches of similar jobs where token efficiency compounds into real savings. If you’re already inside ChatGPT, Codex, or the API on a paid plan, Astra and Soul are live now, and testing them on a visual or spatial task is a reasonable way to see the strengths firsthand. If responses feel clipped or the model stops too early on bigger projects, switching to Soul for lighter, cheaper tasks is a sensible fallback rather than fighting Astra’s quirks on something it wasn’t built to shine at.

## Frequently Asked Questions

### What is the difference between GPT-6.2 Astra and GPT-6.2 Soul?

Astra is OpenAI’s flagship model aimed at the hardest projects, roughly comparable to Anthropic’s Claude Opus 5.5. Soul is the cheaper, everyday model, positioned against Claude Fable 5.2, priced at $2 per million input tokens and $10 per million output tokens, unchanged from the prior generation.

### How much does GPT-6.2 Astra cost to use?

**›**Build a loan-approval app for 2,000 bankers.

*already included:*✓ SOC 2✓ role-based access✓ audit log✓ self-hosted

Astra costs $8 per million input tokens and $40 per million output tokens through the API, a 20% price cut from the previous Astra version and identical to what Anthropic charges for Claude Fable 5.2.

### Is GPT-6.2 Astra better than Claude Opus 5.5 or Fable 5.2?

Not clearly. On combined intelligence benchmarks, Astra trails Fable 5.2 slightly but costs far less per task. Coding performance is roughly even with the prior Astra generation. Several early testers said they preferred Claude models for daily work despite Astra’s price advantage.

### Why do testers say Astra is hard to use despite good benchmarks?

Reviewers consistently report that Astra stops tasks early, compresses its writing in ways that are hard to follow, and asks clarifying questions mid-task. This likely stems from the model being optimized to use far fewer tokens than before, which improves efficiency but changes how natural the interaction feels.

### What does OpenAI mean by Astra hitting a “high threshold” for AI self-improvement?

It means Astra measurably speeds up OpenAI’s own researchers on tasks like debugging experiments and writing low-level chip code, roughly equivalent to adding one skilled engineer to a team. It does not mean the model autonomously rewrites itself, and outside evaluation suggests it is not yet capable of fully automating AI research.
