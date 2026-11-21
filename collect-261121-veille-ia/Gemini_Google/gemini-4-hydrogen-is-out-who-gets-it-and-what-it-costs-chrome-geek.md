---
id: collect-261121-veille-ia/gemini-google/gemini-4-hydrogen-is-out-who-gets-it-and-what-it-costs-chrome-geek
title: "Gemini 4 Hydrogen Is Out: Who Gets It and What It Costs - Chrome Geek"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["hydrogen", "gemini", "google", "model", "says", "carbon", "high", "agent"]
source: https://www.chromegeek.com/gemini-4-hydrogen/
source_anchor: ""
source_lines: [1, 49]
section: "Gemini_Google"
---
# Gemini 4 Hydrogen Is Now Rolling Out to Everyone

**Google just made a Gemini 4 model available to everyone, not only AI Ultra subscribers.** Today it is introducing Gemini 4 Hydrogen, a faster model for everyday work, coding and agent tasks. The details are in Google's launch post, two days after we covered Gemini 4 Carbon now rolling out to AI Ultra and developers.

**A faster Gemini 4, not a new Flash:** Google is positioning Hydrogen as the everyday Gemini 4 model. Gemini 4 Carbon and Gemini 4 Argon still handle the deepest, longest jobs. Hydrogen is built to run quickly on coding, research and other agent workflows, either on its own or as the fast worker next to those larger models.

**A step up from 3.8 Flash:** Google calls Hydrogen a major upgrade for teams coming from Gemini 3.8 Flash. It says Hydrogen finishes tasks 2.5 times faster at its high default setting, more than triples Terminal-Bench 4.1 scores (57% vs. 18.2%), cuts fabricated claims by more than 5x, and copes better when a tool call fails or a search comes back empty.

**Context window:** Google says Hydrogen uses a 1 million-token context window shared across input, thinking and output. A normal response is capped at 64K tokens by default. Longer output, up to the rest of that 1 million-token window, is available through long decode.

**On a Chromebook or Googlebook:** nothing extra to install. Open the Gemini web app or Google Search in Chrome and, once the rollout reaches your account, Hydrogen is part of the experience. If you are on AI Pro or Ultra, you can also pick it in Google Antigravity.

**Flat pricing:** Google is charging the same rate across the entire 1 million-token context window, with no jump at a smaller context size. Hydrogen is also available on the free tier of the Gemini API and in Google AI Studio, under Google's usual free-tier limits.

**Four thinking levels:** you can set Hydrogen to low, medium, high or a new xhigh tier. High is the default. Google says quality, speed and cost scale in a predictable way, so you only wait and pay for the reasoning a task needs. Carbon and Argon default to medium and top out at high. Hydrogen defaults to high and adds xhigh.

These figures come from Google. They are a useful starting point, not an independent test.

**Against Argon:** Google says Hydrogen scores 51 on the Artificial Analysis Intelligence Index at its default high thinking level, just above Argon's 50, and reaches 54 at xhigh, while often finishing tasks faster than other models in its class.

**Other headline numbers:** Google says Hydrogen ranks fifth on Agent Arena, leads its class on HLE-Diamond (58.8%), ranks first overall on the Vals Tax Agent Bench (52.3%), ties for first on the Vals Legal Research Bench (55.8%), and leads its class on AutomationBench-AA. It also posts 53.4% on the new Multimodal Knowledge Agent benchmark and 71.6% on Agents' Last Exam v1.1.

**In the Gemini app:** Google says Hydrogen is better at sticking to your tone, format and rules, so drafts, study guides and rewrites come back closer to ready on the first try. It also says the model is more likely to call the right tools and Connected Apps, and to pull in context from uploads, Gmail, Drive and other connected services.

**The model picker:** according to 9to5Google, 4 Hydrogen is labeled “Everyday help” in the Gemini app picker. 4 Argon is still listed for code and analysis, and 4 Carbon for the most complex work. Free users get Hydrogen through Auto model selection when Gemini decides it is the right model for the prompt.

**Carbon for Pro users:** in the Gemini web app, 4 Carbon currently appears locked for Pro users. That matches Google's earlier Carbon launch, which limited the model in the app to AI Ultra. Google has not given a date for Pro access.

**In Search:** Google is rolling Hydrogen out globally for all users, and calls it the biggest model upgrade to Search yet. It says Search will better understand messy, multi-part questions, return more visual answers, and surface a wider range of links. 9to5Google reports that Hydrogen is the default model for AI Mode and is used for your hardest questions in AI Overviews.

The most interesting developer feature in this launch is Orchestrator, a new option in the Antigravity model picker. Instead of sticking with one model for a whole job, it pairs Gemini 4 Carbon and Gemini 4 Hydrogen and switches between them as the task evolves.

**How the hand-off works:** Carbon starts in a read-only phase to explore dependencies, clear up ambiguities and build a plan, including points where it should re-plan. Hydrogen then writes the edits, updates packages and runs tests. If Hydrogen hits friction, it hands back to Carbon to inspect the state, fix the plan and resume. Google says this is different from a static model router that guesses difficulty from the first prompt and then stays on one model.

**The result Google reports:** on Terminal-Bench 4.1, Orchestrator reached near-Carbon task success rates while, on average, using 24% less quota and finishing 19% faster.

**Who gets Orchestrator:** it is rolling out to Google AI Ultra subscribers, with more models and plans promised later. Enterprises can use it in Gemini for business. Hydrogen itself is rolling out to paid AI Pro and Ultra subscribers, so Pro users can pick Hydrogen today, but Orchestrator is not part of the Pro rollout yet.

**The Antigravity SDK:** if you want to assign the roles yourself, the SDK lets you wire Hydrogen and Carbon together in an explicit pipeline. In Google's open-source issue triage demo, Hydrogen agents handle triage, coding and fast UI iteration from pull-request comments, while a Carbon agent takes larger refactors, new features and security checks.

**Where /swarm still fits:** Google launched Carbon earlier this week with /swarm, which splits a huge, multi-worktree project across parallel agent teams. Orchestrator is aimed at everyday feature work and debugging inside a single session, where running a frontier model on every routine step would be slower and more expensive.

Google says Hydrogen brings the Gemini 4 safety stack to a much wider audience. Compared with 3.8 Flash, it says Hydrogen reduces overreach in agent tasks by up to 81% and under-service by up to 79%, cuts misaligned actions and task shortcutting by 12x, and is up to twice as unlikely to bypass permission boundaries on impossible or blocked tasks. Google also cites a 96.7% score on an internal agent honesty test, and says Hydrogen is more than 2.3x more resilient on the Gray Swan indirect prompt injection benchmark. These are Google's figures.

This is the Gemini 4 launch that actually reaches everyday users. In our first hands-on time in the Gemini web app and Antigravity, Hydrogen felt noticeably quick. That is an early impression, not a benchmark, but it lines up with what Google is promising.

If you are on a Chromebook or Googlebook, you can try it today in the Gemini web app and in Search, with nothing extra to install. Pro and Ultra subscribers can also pick it in Antigravity. Orchestrator, which hands work between Carbon and Hydrogen, is Ultra-only for now.

For most work, Hydrogen looks like the practical default. Save Carbon for the messy, long jobs. In the Gemini web app, 4 Carbon currently appears locked for Pro users, and Google has not said when, or if, that will change.
