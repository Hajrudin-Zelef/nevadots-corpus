---
id: collect-261121-veille-ia/claude-anthropic/claude-fable-5-2-vs-fable-5-1-which-tasks-got-better-and-which-claude-model-to-use-for-each-memeburn
title: "Claude Fable 5.2 vs Fable 5.1: Which Tasks Got Better, and Which Claude Model to Use for Each - Memeburn"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["fable", "claude", "effort", "output", "tokens", "model", "about", "opus"]
source: https://memeburn.com/claude-fable-5-2-vs-fable-5-1/
source_anchor: ""
source_lines: [1, 116]
section: "Claude_Anthropic"
---
TL;DR

- Claude Fable 5.2 costs 8/40 per million input/output tokens vs 10/50 for Fable 5.1, outputs up to 30% faster and blocks about 55% fewer requests.
- The biggest gains are in robotics (+37.3 points), chart reading (+28.6), remote freelance work (17.9% to 41.3%) and science agents (+22.0); computer use (+5.8) and knowledge work (+4.6) moved least.
- The cybersecurity and protein scores in Anthropic's Fable 5.2 column are Mythos 5.2 results; public Fable 5.2 still blocks most of that work.
- For research, reasoning and search, Fable 5.2 at low or medium effort should replace Opus 5.5, while Opus 5.5 stays the default for everyday coding.

Claude Fable 5.2 beats Fable 5.1 on every task Anthropic measured and costs 20% less per token, so if you’re a developer or a Claude subscriber, the real decision is no longer 5.1 or 5.2 but when Fable 5.2 is worth twice the per-token price of Opus 5.5. Anthropic released Fable 5.2 on November 9 alongside Mythos 5.2, its unrestricted twin for vetted organizations, with output up to 30% faster and about 55% fewer safety blocks, while Fable 5.1 drops to legacy status. Here’s how the two models compare across coding, agents, research, search, computer use, robotics, office work, security and writing, which tasks still belong on Opus 5.5 or Sonnet 5.5, and the few reasons to stay on 5.1 for now.

|  | **Claude Fable 5.1** | **Claude Fable 5.2** | 
| Released | September 1, 2026 | November 9, 2026 | 
| Status | Legacy, retirement no sooner than Sept 1, 2027 | Current Fable model | 
| Price per million tokens (input / output) | $10 / $50 | $8 / $40 | 
| Cache reads per million tokens | $0.25 | $0.20 | 
| Fast mode | Not listed | $16 / $80, up to 2.5x faster output | 
| Context window / max output | 1M / 128K tokens | 1M / 128K tokens | 
| Default effort (API) | High | High (medium in Claude apps and Claude Code) | 
| Safety blocks vs 5.1 | Baseline | About 55% fewer, per Anthropic | 

## **Fable 5.2 Costs 20% Less per Token and Far Less per Task**

Claude Fable 5.2 lists at $8 per million input tokens and $40 per million output tokens, against $10 and $50 for Fable 5.1, and cache reads fall from $0.25 to $0.20, according to Anthropic’s Fable 5.2 migration guide. A token is the chunk of text a model reads or writes, roughly half a word on Anthropic’s current tokenizer, and the effort setting decides how long the model “thinks” before answering, from low through medium, high, xhigh and max. Fable 5.2 also adds a fast mode at double the price, $16 and $80, for up to 2.5 times faster output.

The per-token cut is the smaller story, because Fable 5.2 also needs less effort to reach the same score. Reading Anthropic’s cost charts, which plot score against dollars spent per task, we estimate that Fable 5.2 at medium effort scores about 37.6% on the FrontierCode 2.0 coding benchmark for roughly $2.50 a task, already above Fable 5.1’s best result of 32.4%, which cost about $8.70 a task, so the new model beats the old one’s peak at around 30% of the cost. The gap is wider on search and reasoning, where Fable 5.2 at its lowest effort beats Fable 5.1 at its highest for about a quarter of the cost on the WANDR search test and roughly a twentieth on Humanity’s Last Exam.

Subscribers feel this differently, since claude.ai plans don’t bill per token. Anthropic says Fable counts for up to 50% of weekly usage limits on Pro plans and standard Team seats, that eligible plans get one to three Fable usage resets depending on subscription length, and that medium is the default effort in the Claude apps and Claude Code, which means most chat users run Fable 5.2 below the effort level the benchmark headlines use.

## **Fable 5.2 Is the First Fable Worth Paying For Over Opus 5.5 on Big Code Changes**

Claude Fable 5.2 scores 49.1% on FrontierCode 2.0 and 69.9% on FrontierSWE v2, up from 32.4% and 56.3% on Fable 5.1, and that matters because Fable 5.1 lost both tests to the cheaper Opus 5.5, which scores 45.5% and 62.3%. Across the 12 rows of Anthropic’s launch table that compare the public models directly, we counted Opus 5.5 ahead of Fable 5.1 on 11, and one Reddit commenter asked whether Fable 5.1 had always been weaker than Opus while burning several times the tokens, while another said he hadn’t used Fable at all since Opus 5.5 came out.

Effort is where coding cost gets decided. Anthropic says Fable 5.2 at high effort matches Opus 5.5 at max effort on FrontierCode for about half the cost per task, xhigh gives the top score, and max actually scores lower than xhigh, at about 45.7% on our reading of the chart, while costing roughly three times as much. Anthropic’s documentation adds that at xhigh and max, Fable 5.2 may spin up subagents (helper copies of itself) to review its own work, and those can make up much of a session’s bill, which explains the “insane bills” some early users describe.

Early owners on Reddit describe Fable 5.2 fixing bugs that Opus 5.5 couldn’t solve in two to three days within a few hours, and auditing a stalled project into a list of everything that needed fixing. Another developer found it six times as expensive as Opus 5.5 per coding task with both on medium, even though it found more related issues, and now uses it as an “on demand focused architect” for design reviews while a cheaper model writes the code. That split matches Anthropic’s own model selection guide on claude.dev, which sends everyday engineering and debugging to Opus 5.5 at medium and keeps Fable 5.2 for major refactors, migrations, architecture and runs that lead many subagents.

## **Long Unattended Agent Runs Changed the Most**

Claude Fable 5.2 more than doubles Fable 5.1’s score on the Remote Labor Index, from 17.9% to 41.3%, a test built from real freelance projects, and lifts AutomationBench, which measures business workflows, from 31.4% to 51.4%. Those are the gains that matter for agents left running for hours or days, which is the job Anthropic built Fable for. One Reddit user argued that nobody will know how good the model really is until people finish their first multi-day task with it, which is a fair warning for any launch-day verdict, ours included.

Fable 5.2 isn’t the top scorer everywhere here, since Google’s Gemini 4 Carbon still leads AutomationBench at 53.9%. Anthropic’s documentation also flags behaviors to plan for. On long multistep tasks, Fable 5.2 sometimes ends a turn with a list of remaining work or an offer to continue, more often at low and medium effort. Long stretches of tool calls can pass with little visible text, and when a prompt doesn’t say who owns the code, the model tends to report nearby problems rather than fix them, so unattended runs need high effort and a prompt that spells out what the agent is allowed to change.

## **Research, Reasoning and Search Favor Fable 5.2 Even at Low Effort**

Claude Fable 5.2 reaches 74.2% on Humanity’s Last Exam with tools, up from 65.6%, and 74.6% on Terminal-Bench-Science, up from 52.6%, while its agentic search score on WANDR rises from 68.7% to 82.1%. On Anthropic’s reasoning chart, low and medium effort land almost on top of each other at about 68% for around 20 cents a question, which already beats Fable 5.1 and Opus 5.5 at their highest settings and sits well above GPT-6 Astra’s 57.2%, the OpenAI model in our roundup of every major AI model from Kimi K3 to GPT-6 Astra.

A Reddit commenter spotted the same thing and called medium the high-value setting for reasoning. For research and analysis, that makes low or medium effort the sensible starting point, with high reserved for questions where a few extra points matter more than the bill.

## **Chart Reading Jumped While Computer Use Barely Moved**

Claude Fable 5.2’s biggest everyday gain may be reading charts and figures, where its Chartography score rises from 44.8% to 73.4%, ahead of Opus 5.5’s 64.4%, which helps anyone feeding the model dashboards, financial reports or scientific plots. Computer use, where the model clicks through apps and websites on its own, moved far less, from 83.5% to 89.3% on OSWorld 2.1, close to Opus 5.5’s 87.2%, so Anthropic’s guide still puts computer use and vision-heavy workflows on Opus 5.5. Developers migrating a computer-use agent also need to switch tools, because Fable 5.2 rejects the older computer_20251124 tool everywhere except Amazon Bedrock.

## **Robotics and CAD Show the Largest Gains, With Limits**

Claude Fable 5.2 jumps from 27.6% to 64.9% on RoboDojo Sim, the largest gain in the launch table at 37.3 points, and on Anthropic’s BenchCAD hardware engineering chart, Fable 5.2 at high effort scores around 61, well above Fable 5.1’s best of about 44, by our reading. Those are simulated benchmarks, though, and game developers on Reddit say Claude models, Fable 5.2 included, still bend a character’s wrists oddly when rigging and posing 3D models, which shows how uneven progress remains in work that mixes spatial sense with code.

## **Office Work Still Belongs on Opus 5.5 or Sonnet 5.5**

Claude Fable 5.2 improves on AA-Briefcase, a knowledge-work benchmark, from 61.5% to 66.1% on the rubric score, but on the same test’s head-to-head Elo rating, Opus 5.5 still edges it, 1,800 to 1,785. Anthropic’s guide sends everyday documents, slides and spreadsheets to Sonnet 5.5, with Opus 5.5 for complex financial models, so paying Fable rates for memos and decks buys little.

## **The Cybersecurity and Biology Scores Belong to Mythos, Not Fable**

The Fable 5.2 column in Anthropic’s launch table shows 76.3% on CyScenarioBench, 89.6% on CVE Rediscovery and 85.1% on protein binder design, but the small print marks all three as Mythos 5.2 results. Fable 5.2 is the same model with safety classifiers that still block high-risk cyber work such as penetration testing and exploit generation, as well as most research biology, according to Anthropic’s Fable 5.2 announcement, and blocked requests fall back to Opus 4.8 for cyber or Opus 5 for biology.

Anthropic says Fable 5.2 blocks about 60% fewer cyber requests and 40% fewer biology requests than Fable 5.1, which helps defenders and researchers at the margin, but anyone choosing a model for security or life-sciences work should read those three rows as what Mythos does, not what a Fable subscription gets. One Reddit user summed up the frustration by saying he reads the chart as zero in every category because the guardrails stop him anyway.

## **Fable 5.2 Writes More Clearly but Follows Rules Literally**

Anthropic says testers found Fable 5.2 communicates more clearly than earlier Fable models, and early Reddit impressions agree, with users calling it noticeably faster, “clear and to the point,” and less inclined to “spew ten thousand words.” The documentation adds two quirks writers should know. Fable 5.2 follows style rules closely, so a hard sentence-length limit can make prose choppy, and at medium effort and above it keeps thinking even when a prompt asks it to answer directly, so the fix is to lower effort rather than add instructions.

## **Reasons to Stay on Fable 5.1 a Little Longer**

Claude Fable 5.1 stays available until at least September 1, 2027, so nobody has to move today, and developers have a few concrete reasons to test before switching:

- Fable 5.2 checks every account’s conversation history against its thinking blocks, so tools that edit earlier turns can hit a 400 error that Fable 5.1 only enforced for newer accounts.
- If a router or fallback moves a conversation from 5.2 to 5.1, Fable 5.1 can’t read 5.2’s reasoning and continues without it.
- Advisor setups break, since a Fable 5.2 agent only accepts Fable 5.2 or Mythos 5.2 as its advisor.
- Fable 5.2 has its own rate limit, separate from the shared Fable 5.1 and Fable 5 limit, which helps capacity but needs re-checking in production.

Launch week also brought reports on Reddit of Fable 5.2 conversations on claude.ai compacting, or summarizing older messages, far more often than expected, as if the context window were 200,000 tokens instead of 1 million. Users described it as an intermittent glitch rather than a new limit, and several said updating the Claude desktop app or Claude Code fixed it, since older versions treat the new model as unknown and fall back to the smaller window.

## **Fable 5.2’s Real Rival Is Opus 5.5, Not Fable 5.1**

Memeburn’s reading of Anthropic’s launch charts is that for reasoning- and search-heavy work, Fable 5.2 at low or medium effort should replace Opus 5.5, not just Fable 5.1, while Opus 5.5 should stay the default for coding. On Humanity’s Last Exam, Fable 5.2 at low effort scores about 68% for roughly 19 cents a question, above Opus 5.5’s best of 67.7% at about $2.20, and close to what Opus 5.5 costs at medium effort, so the doubled per-token price disappears once you count cost per task. On FrontierCode, Fable 5.2 at low effort and Opus 5.5 at a similar spend both land around 34%, which means coding gets no free upgrade and Fable only earns its keep above high effort.

Our call has a clear limit, since these are Anthropic’s own tasks and one developer already reports paying six times Opus prices for real coding work, so if your own tests show Fable 5.2 burning more tokens than the charts suggest, the old rule of Opus first holds for research too.

| **Task** | **Start with** | **Effort** | **Why** | 
| Major refactors, migrations, architecture reviews | Fable 5.2 | Medium, then high | Beats Opus 5.5 at max for about half the cost at high | 
| Everyday coding and debugging | Opus 5.5 | Medium, then high | Ties Fable 5.2 at low effort for similar spend | 
| Long unattended agent runs | Fable 5.2 | High | Largest gains, and fewer early stops above medium | 
| Research, analysis, hard reasoning | Fable 5.2 | Low or medium | Beats Opus 5.5’s best for less on Anthropic’s charts | 
| Deep web research | Fable 5.2 | Low, then high | Low already beats Fable 5.1 and Opus 5.5 at max | 
| Computer use and vision-heavy flows | Opus 5.5 | Medium | Fable 5.2 gains only about 2 points over Opus | 
| Reading charts and reports | Fable 5.2 | Medium | Biggest everyday jump, 44.8% to 73.4% | 
| Documents, slides, spreadsheets | Sonnet 5.5 | High | Opus 5.5 still leads Fable on knowledge-work Elo | 
| Security testing and life-sciences research | Mythos 5.2 (verified orgs) | High | Fable 5.2 still blocks most of this work | 

The cheapest habit for teams this week is to run their own ten hardest tasks on Fable 5.2 at low, medium and high, record the cost of each from the API’s usage data, and compare that with Opus 5.5 at high before switching anything, because the gap between Anthropic’s charts and one developer’s six-times bill is exactly what that test settles. Five months after the original Fable 5 launch, Fable 5.2 is the first version where that test is likely to favor Fable on anything beyond the hardest coding jobs.

## **FAQs**

### **Is Claude Fable 5.2 better than Fable 5.1?**

Yes, on every benchmark Anthropic published. Fable 5.2 scores higher on coding, agents, research, search, computer use, robotics and knowledge work, costs $8 and $40 per million input and output tokens instead of $10 and $50, and generates output up to 30% faster.

### **How much does Claude Fable 5.2 cost?**

Fable 5.2 costs $8 per million input tokens and $40 per million output tokens on the Claude API, with cache reads at $0.20, a 50% Batch API discount, and an optional fast mode at $16 and $80.

### **When will Claude Fable 5.1 be retired?**

Anthropic lists Fable 5.1 as a legacy model with retirement no sooner than September 1, 2027.

### **Should I use Fable 5.2 or Opus 5.5?**

Anthropic recommends starting most coding and knowledge work on Opus 5.5 at medium effort and moving to Fable 5.2 for major refactors, migrations, architecture, long unattended runs and deep research. Opus 5.5 costs half as much per token.
