---
id: collect-261121-veille-ia/gpt-openai/openai-gpt-6-2-astra-cuts-api-prices-20-adds-sol-model
title: "OpenAI GPT-6.2 Astra cuts API prices 20%, adds Sol model"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["astra", "gpt-6", "openai", "model", "tokens", "work", "more", "cost"]
source: https://betanews.com/article/gpt-6-2-astra-api-pricing/
source_anchor: ""
source_lines: [1, 33]
section: "GPT_OpenAI"
---
OpenAI released two new models, GPT-6.2 Astra and GPT-6.2 Sol, on Nov. 17 and sold them on cost per task as much as on raw scores. According to the company, each one burns through fewer output tokens to finish hard jobs, so paying subscribers stretch their usage caps further and developers spend less per request.

Embed: https://x.com/OpenAI/status/2122750124543926515

Astra succeeds GPT-6 Astra in the flagship role and targets the toughest professional and scientific work. Sol fills the lower-cost slot for daily use. Paid ChatGPT subscribers on the Plus, Pro, Enterprise and Edu tiers can pick either one inside ChatGPT Work or Codex, and developers can reach them in the API under the IDs gpt-6.2-astra and gpt-6.2-sol.

API rates for Astra drop 20% from its predecessor, to $8 per million tokens in and $40 per million out. Cached input, meaning text the system has already processed, falls 60% to $0.40 per million. Tokens are small chunks of text that a model reads or writes, and billing is counted in them.

Pricing for Sol stays where GPT-6.1 Sol set it: input runs $2, cached input 10 cents and output $10, each per million tokens. Subscribers on an identical paid plan can still finish more work with Sol than with its predecessor, OpenAI said.

Slide decks, spreadsheets and documents produced by Astra stick closer to the templates users supply, follow each field’s professional norms more faithfully and sound less machine-generated, the company claimed. Code from the new flagship is cleaner, and its front ends look and function better, with the biggest gains in 3D design, games and websites.

Agents’ Last Exam v1.1, a benchmark of lengthy, multistage professional tasks, showed Astra beating GPT-6 Astra by 5.2 points with per-task spending cut by more than two-thirds. Against Anthropic’s Claude Fable 5.2, the margin was 5.5 points, with Astra costing just 5% as much. A footnote on OpenAI’s chart states that the Fable 5.2 leaderboard figures omit nine tasks handled by a fallback model.

That chart also plotted Gemini 4 Carbon from Google and Claude Opus 5.5. Science is where OpenAI pushed hardest, with the post declaring, “We believe GPT-6.2 Astra is the best model in the world for scientists.”

Its Terminal-Bench-Science 0.1 result, from a test of research run inside a terminal, outscored GPT-6 Astra by 9 points, and each task cost below a third of what the older model needed. Vals Science Index results set a fresh record too, on a benchmark where models analyze genuine biology datasets and design experiments to deduce the behavior of a simulated system.

Cognition’s FrontierCode 2.0, which grades code edits on real projects against the bar maintainers set for merging, put Sol no more than 0.5 points behind Astra for about a fifth of the cost, with both at maximum settings. LifeVal, an OpenAI test built around everyday chores such as picking kindergartens following a relocation or drawing up meal plans, had Sol topping GPT-6 Astra at around one-seventh the cost. Internally, many OpenAI employees already treat Sol as their go-to model for technical and nontechnical work, per the announcement.

Legora CTO Jacob Lauritzen reported a 62% drop in token use against GPT-6.1 Sol on complicated legal-agent jobs. His team’s tests put Astra ahead of competing models in eight of the 10 legal research assignments, which covered litigation, tax and M&A.

Niko Grupen, who leads applied research at Harvey, said Astra finished more than triple the Legal Agent Benchmark tasks that GPT-6 Astra managed and cut output tokens by 34.5%. ClickUp’s head of AI, Jay Hack, wrote that Astra came out slightly ahead in blind comparisons against the model ClickUp runs in production for the slides, dashboards and apps ClickUp Brain generates, needing “about 40% fewer tokens and 30% fewer review rounds.”

Eros Wang, a technical staff member at Basis, said “our average time for long-horizon accounting work fell by 50%,” and the most complex jobs took up to 73% less time. Jump Trading’s Adam Alnasser described Astra hitting the ceiling on a hardware optimization benchmark that had held up for upward of three years, and doing it within a single day.

Cybersecurity carries the biggest caveat. Under its Preparedness Framework, OpenAI rates both models at the Critical level for cyber capability and shipped them with tighter guardrails on higher-risk security help. Legitimate bug hunting and penetration-style testing can get caught by those limits too, the company conceded in the launch post, which describes the safeguards on both models as its strongest so far.

Most performance figures in the announcement come from OpenAI’s own testing or from its customers. Independent benchmarker Artificial Analysis gave Astra a 62 on the firm’s Intelligence Index, behind Claude Fable 5.2 alone, which scored 65, and edging past Gemini 4 Carbon, which sat at 60. Coding results were flat, with the firm’s Coding Agent Index showing no gain for Astra over GPT-6 Astra.

Camila Nogueira covers artificial intelligence, security and privacy with a focus on real-world usability. She writes about generative-AI tools including ChatGPT and Microsoft Copilot, alongside the everyday security decisions that come with them: data breaches, VPNs, password managers and the protections already built into the services people use. With a background in UX and content strategy, Camila explains what a tool or a risk actually means in practice — what it does well, where it breaks down, and whether it is worth the reader's time or attention. She is particularly interested in the point where new technology meets ordinary caution: how much to trust an AI answer, and how much protection is proportionate to the actual risk. Her security and privacy coverage is informational and independent, with no affiliate relationships influencing the products she discusses.

## Discussion
