---
id: collect-261121-veille-ia/gemini-google/does-gemini-4-neon-actually-beat-opus-5-5-on-every-benchmark
title: "Does Gemini 4 Neon actually beat Opus 5.5 on every benchmark?"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["neon", "opus", "gemini", "output", "input", "carbon", "tokens", "claude"]
source: https://medium.com/@amitsrivastava115/does-gemini-4-neon-actually-beat-opus-5-5-on-every-benchmark-0f86926999de
source_anchor: ""
source_lines: [1, 467]
section: "Gemini_Google"
---
Google released Gemini 4 Neon on November 18, 2026. The launch chart puts Neon ahead of Claude Opus 5.5 on almost every row, at 25% lower token prices. Google made that chart, so I went through the model card row by row, then checked every independent leaderboard that had published Neon numbers by November 19: Artificial Analysis, Vals AI, Cursor’s CursorBench, Zapier’s AutomationBench and Arena.

The short answer is no. Neon loses a few benchmarks, ties far more than the launch chart suggests, and wins the rest by margins from 2 points to 28 points. The useful part is which ones, at what effort level, and at what cost per task.

What Gemini 4 Neon actually is

Neon is the fourth Gemini 4 model in about seven weeks, after Argon, Carbon and Hydrogen. The model card says Neon is “adapted from Gemini 4 Carbon,” and Google places it between Hydrogen (fast everyday work) and Carbon (the hardest long-horizon projects). In the Gemini app it replaces Argon as the “Deeper analysis” option for AI Pro subscribers.

Model ID: gemini-4-neon

Launch date: November 18, 2026

Context window: 1,048,576 tokens, shared across input, thinking and output

Output: up to 65,536 tokens per synchronous segment, up to 1M with background long decode

Input: text, images, audio, video, PDF, Office and OpenDocument files

Output modality: text only

Thinking levels: low, medium, high (default), xhigh

Knowledge cutoff: March 2026

API price per 1M tokens: $3 input, $0.15 cached input, $15 output (thinking billed as output)

Batch and Flex per 1M tokens: $1.50 input, $7.50 output

Priority per 1M tokens: $5.40 input, $27 output

Free tier: none

Where: Gemini API (AI Studio, Android Studio), Gemini for business, Google Antigravity for paid AI Pro and Ultra, Gemini app for AI Pro and Ultra

And the model it is measured against:

Claude Opus 5.5 release date: September 22, 2026

Claude Opus 5.5 price per 1M tokens: $4 input, $20 output

Claude Opus 5.5 context window: 1M tokens

Claude Opus 5.5 max output: 128,000 tokens

Claude Opus 5.5 inputs: text and images

On list price Neon is exactly 25% cheaper on input and output. It also takes audio and video natively, which Opus does not.

How Neon relates to Carbon

Carbon and Argon are on promotional pricing of $2 input and $10 output until November 30, then move to $4 and $20 on December 1. So Neon is 50% more expensive per token than Carbon this month and 25% cheaper from next month.

The “middle” model is at least as strong as the flagship on several independent tests. On the Artificial Analysis Intelligence Index, Neon at xhigh scores 61 and Carbon at high scores 60. On CursorBench 4.1, Neon at high scores 63.7% and Carbon at high 61.5%.

Round one: Google’s own table

The model card compares Neon with Claude Opus 5.5 and GPT-6.2 Sol. These are self-reported. Some rows are Google’s runs, some come from third-party leaderboards, and some competitor figures come from the competitor’s own published results, so settings are not identical.

AA Intelligence Index v5.0.1: Neon 61, Opus 5.5 59

Vals Index v2.2: Neon 70.9%, Opus 5.5 70.1%

CursorBench 4.1: Neon 65.4%, Opus 5.5 58.1%

Terminal-Bench 4.1 (cheating adjusted): Neon 65.2%, Opus 5.5 65.8%

FrontierCode 2.0: Neon 46.2%, Opus 5.5 47.3%

SWE-sweep (provider harness): Neon 39.0%, Opus 5.5 10.1%

SWE-sweep (shared mini-SWE harness): Neon 12.8%, Opus 5.5 3.6%

AutomationBench: Neon 56.3%, Opus 5.5 42.5%

Agents’ Last Exam v1.1: Neon 75.8%, Opus 5.5 71.6%

RiemannBench: Neon 84.0%, Opus 5.5 80.0%

Terminal-Bench-Science 0.1: Neon 70.5%, Opus 5.5 63.3%

HLE-Diamond: Neon 71.3%, Opus 5.5 67.9%

MKA (Climb WSR): Neon 56.5%, Opus 5.5 53.0%

Chartography: Neon 73.5%, Opus 5.5 66.3%

LVBench: Neon 89.0%, Opus 5.5 83.7%

GraphWalks 256K to 1M (BFS F1): Neon 95.2%, Opus 5.5 66.8%

CWE-Bench v2: Neon 70.7%, Opus 5.5 67.1%

WebExploitBench v1.2: Neon 51.3%, Opus 5.5 37.1%

SEC-bench Pro: Neon 90.5%, Opus 5.5 not reported

For reference, GPT-6.2 Sol beats Neon on one row in the same table, Chartography, 75.1% to 73.5%.

That is 18 rows with an Opus number. Counted strictly, Neon wins 16 and loses 2: Terminal-Bench 4.1 by 0.6 points and FrontierCode 2.0 by 1.1 points.

A 0.8-point lead on a composite index is not a real win, so for the rest of this article a gap of 1.0 point or less counts as a tie. Under that rule Google’s own table reads 15 wins, 2 ties (Vals Index, Terminal-Bench 4.1) and 1 loss (FrontierCode 2.0).

So even Google’s chart doesn’t show Neon beating Opus everywhere, and the two rows it doesn’t win are both core coding benchmarks.

The SWE-sweep row needs a caveat before anyone quotes it. Neon’s 39.0% is inside Google Antigravity. In the shared mini-SWE harness the same model scores 12.8%, a 3x swing from the harness alone. Opus went from 3.6% to 10.1% in Claude Code, while Sol fell from 11.6% to 8.3% in Codex. Google says Antigravity’s ability to send subagents across a repository suits this test. The 39.0 vs 10.1 headline compares agent products as much as models.

A launch review that read Google’s methodology notes reports that LVBench gave Gemini one low-resolution frame per second while Sol got 500 frames and Opus 600, because of API limits. The same review says the Opus Chartography figure comes from Anthropic’s system card. I couldn’t open the methodology page myself, so treat both as secondhand.

Round two: Artificial Analysis

Artificial Analysis runs its own index and published a Neon breakdown on launch day. It is the strongest independent source because it also measures cost and time per task.

Intelligence Index v5.0.1: Neon (xhigh) 61, Opus 5.5 (max, default fallback) 59

Cost per index task: Neon $6.44, Opus 5.5 $10.04

Time per task: Neon 21.5 min, Opus 5.5 24.5 min

Output speed: Neon 110 tok/s, Opus 5.5 118 tok/s

Time to first answer token: Neon 56.51 s, Opus 5.5 335.85 s

Output tokens used on the index: Neon 283M, Opus 5.5 218M

Coding Agent Index: Neon in Antigravity CLI (xhigh) 59, Opus 5.5 in Claude Code (max) 58

Coding Agent Index cost per task: Neon $6.86, Opus 5.5 $12.61

The Intelligence Index gap is 2 points, a win. The Coding Agent Index gap is 1 point, a tie. Google’s launch post calls that “#1 overall (58.6),” while Artificial Analysis described Neon as scoring “in line with” Opus 5.5 and Fable 5.2 on coding agents.

The cost result is cleaner than the score result: 36% less per task than Opus on the Intelligence Index and 46% less on the Coding Agent Index. That saving comes from price, not efficiency. Neon used 30% more output tokens than Opus on the index, and Artificial Analysis ranks it 93rd of 95 on verbosity.

Where Neon sits among all frontier models on this index:

Claude Fable 5.2 (max): 65, $26.54 per task

GPT-6.2 Astra (max): 62, $4.00 per task

Gemini 4 Neon (xhigh): 61, $6.44 per task

Gemini 4 Carbon (high): 60, $7.76 per task

Claude Opus 5.5 (max): 59, $10.04 per task

GPT-6.2 Sol (max): 57

Neon is third, not first. GPT-6.2 Astra scores one point higher for $2.44 less per task even though its list price is $8 input and $40 output, because it used 65.8M output tokens on the index against Neon’s 283M. Per-token price and per-task cost are different numbers.

Two more Artificial Analysis details missing from Google’s chart:

AA-Briefcase (agentic knowledge work): Neon (xhigh) 1700 Elo, Fable 5.2 1785, Sonnet 5.5 1815

AA-Briefcase Presentation Quality: Neon 1771 Elo, the highest Artificial Analysis has measured

GDP.pdf: Neon (high) 33.4%, Neon (xhigh) 28.0%, Carbon (high) 28.4%

On GDP.pdf, Neon at high beat Neon at xhigh by 5.4 points. More thinking is not always better.

Round three: Vals AI

Vals evaluated Neon at xhigh across its suite on launch day. I pulled the same benchmarks from the Vals page for Opus 5.5, run at max. Vals ran Opus with older Claude models as server-side fallbacks for refused steps, and its Opus scores include those fallback-assisted tasks.

Vals Index: Neon 70.87%, Opus 5.5 70.14%

Vals Index cost per test: Neon $7.40, Opus 5.5 $24.77

Vals Index latency per test: Neon 31 min 25 s, Opus 5.5 84 min 31 s

Vals Finance Index: Neon 68.31%, Opus 5.5 65.34%

Vals Legal Index: Neon 78.36%, Opus 5.5 72.37%

Vals Science Index: Neon 62.63%, Opus 5.5 58.65%

Earnings Modeling Bench v1.1: Neon 76.46%, Opus 5.5 75.84%

Finance Agent v2.1: Neon 68.56%, Opus 5.5 67.85%

Legal Research Bench: Neon 56.25%, Opus 5.5 50.48%

Tax Agent Bench: Neon 54.89%, Opus 5.5 44.54%

Harvey Legal Agent Benchmark: Neon 19.58%, Opus 5.5 9.17%

Vibe Code Bench v1.1: Neon 93.70%, Opus 5.5 90.29%

BioMysteryBench: Neon 80.00%, Opus 5.5 79.26%

MysteryMechanism: Neon 45.05%, Opus 5.5 49.55%

Terminal-Bench 4.0: Neon 61.62%, Opus 5.5 65.15%

Terminal-Bench-Science: Neon 62.86%, Opus 5.5 47.14%

The Vals Index is a tie: 0.73 points apart, with error bars of ±0.94 and ±1.00. Vals ranks Claude Fable 5.2 first at 71.31%, Neon second and Claude Sonnet 5.5 third at 70.85%. The top five sit within 1.2 points.

Cost and time are where the gap opens. Neon costs 70% less per test than Opus on the Vals Index and finishes in 37% of the time, using 15.15M tokens per test against 59.46M. That is the opposite of the Artificial Analysis token picture, so token use depends heavily on the task mix.

Neon’s clearest Vals wins are legal and tax: Legal Research Bench (+5.8), Tax Agent Bench (+10.4), Harvey’s legal agent benchmark (+10.4) and the Legal Index (+6.0). Vals ranks Neon first of 99 on Legal Research Bench and first of 90 on Tax Agent Bench.

Opus wins MysteryMechanism (+4.5) and Terminal-Bench 4.0 (+3.5). On Terminal-Bench 4.0, 22 Opus tasks (11.11%) used a fallback model. At Opus’s September launch, Vals reported that counting fallback-assisted tasks as failures cut its Terminal-Bench 4.0 score from 61.62% to 53.54%. The summary page doesn’t print the current fallback-excluded number, so I’m counting this as an Opus win with an asterisk.

Terminal-Bench-Science shows how much harness and settings matter. Google’s table had Neon at 70.5%. Vals measured the same model at 62.86%. That’s 7.6 points on one benchmark, so no single number should be trusted to the decimal.

Round four: Cursor, Zapier and Arena

CursorBench 4.1 is Cursor’s benchmark of ambiguous, multi-file tasks taken from real Cursor sessions. It publishes cost, tokens and steps for every effort level, which makes it the best effort-matched comparison available.

CursorBench top effort: Neon (xhigh) 65.4%, Opus 5.5 (max) 58.1%

CursorBench cost per task at top effort: Neon $10.67, Opus 5.5 $11.88

CursorBench steps per task at top effort: Neon 73, Opus 5.5 144

CursorBench at high: Neon 63.7% for $6.54, Opus 5.5 54.5% for $3.87

CursorBench at medium: Neon 57.7% for $3.07, Opus 5.5 50.0% for $2.85

CursorBench at low: Neon 45.5% for $1.26, Opus 5.5 42.7% for $1.43

Neon wins at every matched effort level. Neon at high beats Opus at max by 5.6 points for 45% less per task, and it also tops Claude Fable 5.2 at max (61.3% for $48.11). The effort curve is the warning: going from Neon high to xhigh buys 1.7 points for 63% more cost per task.

AutomationBench is Zapier’s cross-app business workflow test. A task passes only if the final CRM, inbox or calendar state is correct, with negative checks to catch agents that email everyone.

AutomationBench 1.0.6: Neon (xhigh) 56.32%, Opus 5.5 (max, default fallbacks) 42.47%

AutomationBench cost per task: Neon (xhigh) $1.69, Neon (high) $0.88

Same board: Gemini 4 Carbon (high) 53.88% at $2.12, GPT-6.2 Astra (max) 52.36% at $0.83, Claude Fable 5.2 (max) 51.45% at $4.64

The 13.9-point margin over Opus is the biggest independent gap in this article. Neon at high (54.34%) also beats Astra at max for about the same cost.

Arena is the human-preference leaderboard. Arena ran Neon at high.

Text Arena: Neon (high) 1,518, Opus 5.5 (high) 1,507

Text Arena, longer queries: Neon (high) 1,521, Opus 5.5 (high) 1,522

WebDev Arena: Neon (high) 1,764, Opus 5.5 (max) 1,818

Agent Arena: Neon (high) 1,333, Opus 5.5 (max) 1,336

Agent Arena cost per task (median): Neon (high) $2.62, Opus 5.5 (max) $5.88

Neon’s Text rating carries a ±10 margin, so an 11-point lead is a tie, and longer queries are a tie too. WebDev is a real loss: 54 points behind Opus at max, with Fable 5.2 (1,824) and GPT-6.2 Astra (1,797) also ahead, leaving Neon 7th. Agent Arena is a tie. Google’s launch post cited 1,336 and second place from an earlier snapshot. By the November 18 update Neon had slipped to third, 3 points behind Opus at max. Against Opus at high (1,273), Neon at high leads by 60 points, at $2.62 vs $2.01 per task.

A launch review that read Arena’s Agent breakdown says Neon ranked 2nd on steerability and 49th on Bash-command reliability. I couldn’t verify the Bash figure directly, so it’s secondhand, but anyone running terminal-heavy agents should check it.

The full independent scorecard vs Opus 5.5

Every independent head-to-head I could verify, comparing each model’s best published setting, with the 1-point tie rule (and published error bars for Arena):

AA Intelligence Index: Neon win (61 vs 59)

AA Coding Agent Index: tie (59 vs 58)

CursorBench 4.1: Neon win (65.4% vs 58.1%)

AutomationBench: Neon win (56.32% vs 42.47%)

Vals Index: tie (70.87% vs 70.14%)

Vals Finance Index: Neon win (68.31% vs 65.34%)

Vals Legal Index: Neon win (78.36% vs 72.37%)

Vals Science Index: Neon win (62.63% vs 58.65%)

Earnings Modeling Bench: tie (76.46% vs 75.84%)

Finance Agent v2.1: tie (68.56% vs 67.85%)

Legal Research Bench: Neon win (56.25% vs 50.48%)

Tax Agent Bench: Neon win (54.89% vs 44.54%)

Harvey Legal Agent Benchmark: Neon win (19.58% vs 9.17%)

Vibe Code Bench v1.1: Neon win (93.70% vs 90.29%)

BioMysteryBench: tie (80.00% vs 79.26%)

MysteryMechanism: Opus win (49.55% vs 45.05%)

Terminal-Bench 4.0 (Vals): Opus win (65.15% vs 61.62%)

Terminal-Bench-Science (Vals): Neon win (62.86% vs 47.14%)

Text Arena: tie (1,518 vs 1,507)

WebDev Arena: Opus win (1,818 vs 1,764)

Agent Arena: tie (1,333 vs 1,336)

Independent tally: 11 Neon wins, 7 ties, 3 Opus wins, out of 21 rows.

Google’s own table: 15 Neon wins, 2 ties, 1 Opus win, out of 18 rows (16 to 2 counted strictly).

Neon wins 83% of the rows on Google’s chart. On independent boards it wins 52%, ties 33% and loses 14%. It beats Opus 5.5 more often than it loses, and on all five independent boards that publish comparable per-task costs (two from Artificial Analysis, plus Vals, CursorBench and Agent Arena) it is cheaper. No source, including Google’s own, supports “beats Opus on every benchmark.”

What’s not published yet

Several benchmarks people ask about have no Neon result as of November 19:

SWE-bench Verified: Google did not report it, and Vals dropped it from its index in August as saturated. Google reports SWE-sweep instead.

ARC-AGI: no Neon entry on the ARC Prize results page, which lists Carbon and Argon.

Epoch AI: no Neon entry yet, so no Epoch Capabilities Index or FrontierMath score.

Full Safety & Security Report: the model card says it “will be available in the coming days.”

Where Neon clearly wins

1. Cross-app business workflows: AutomationBench (+13.9 over Opus) and the Vals Finance Index. Google also reports first place on Vending-Bench 2 ($16,843).

2. Legal and tax work: every Vals legal and tax row.

3. Long-context retrieval: GraphWalks at 256K to 1M, 95.2% vs 66.8%. Self-reported, but a 28-point margin.

4. Coding in Cursor’s harness, at every effort level.

5. Cost per task: 36% less on the AA index, 46% less on the AA coding index, 70% less on Vals, 10% less on CursorBench at top effort, 55% less on Agent Arena.

6. Time to first answer token: 56.5 s vs 335.9 s on Artificial Analysis at top effort.

Where Opus 5.5 still holds

1. Terminal work: Terminal-Bench 4.1 in Google’s own table (65.8% vs 65.2%) and Terminal-Bench 4.0 on Vals (with the fallback asterisk).

2. FrontierCode 2.0, which grades code changes against reviewer criteria: 47.3% vs 46.2% in Google’s table.

3. Web apps judged by humans: WebDev Arena, 54 points.

4. MysteryMechanism on Vals, 4.5 points.

5. Output per request: Opus offers 128K output tokens. Neon caps synchronous segments at 65,536 and needs background long decode or client continuation beyond that.

Opus holds the tasks closest to hands-on software engineering in a terminal or browser. Neon takes knowledge work, legal and finance, automation and long-context retrieval, and wins on price nearly everywhere.

Pricing math

Standard list prices per 1M tokens:

GPT-6.2 Sol (up to 272K input): $2 input, $10 output

Claude Sonnet 5.5: $2 input, $10 output

Gemini 4 Carbon through Nov 30: $2 input, $10 output

Gemini 4 Neon: $3 input, $15 output

Claude Opus 5.5: $4 input, $20 output

Gemini 4 Carbon from Dec 1: $4 input, $20 output

GPT-6.2 Astra (up to 272K input) and Claude Fable 5.2: $8 input, $40 output

One job with 100,000 input tokens and 10,000 output tokens, no cache:

Neon: 0.1 x $3 + 0.01 x $15 = $0.45

Opus 5.5: 0.1 x $4 + 0.01 x $20 = $0.60

Sol or Sonnet 5.5: $0.30

Astra or Fable 5.2: $1.20

Same job with 80,000 of the input tokens served from cache:

Neon: $0.060 + $0.012 + $0.150 = $0.222

Opus 5.5 (cached input $0.20): $0.080 + $0.016 + $0.200 = $0.296

At 1,000 jobs a day for 30 days: Neon $6,660, Opus 5.5 $8,880, Neon on Batch about $3,330

Those numbers assume identical token counts, which never happens. Artificial Analysis found Neon used 30% more output tokens than Opus. Vals found it used about a quarter as many. The leaderboard per-task costs above are the better guide.

Against Sol, the per-token comparison flips: Sol’s standard rate is a third cheaper. Sol’s long-context surcharge changes that above 272K input tokens, where the whole request bills at $4 input and $15 output, while Neon’s rate stays flat across 1M. A 300,000-token input with 10,000 output tokens costs $1.05 on Neon and $1.35 on Sol.

Speed and the thinking dial

Artificial Analysis measured Neon at about 110 output tokens per second at xhigh, against about 57 for Carbon, cutting time per task by roughly 40%. In absolute terms xhigh is still slow, 21.5 minutes per index task against a 12.3-minute median for models at a similar price.

Low: AA index 44, 3.7 min per task, CursorBench 45.5% at $1.26

Medium: AA index 55, 8.3 min per task, CursorBench 57.7% at $3.07

High: AA index 60, 14.8 min per task, CursorBench 63.7% at $6.54

Xhigh: AA index 61, 21.5 min per task, CursorBench 65.4% at $10.67

High to xhigh adds 1 index point and 6.7 minutes per task. Google’s docs say to start at high and keep xhigh “for hard, multi-step problems where high falls short.”

How to use it

The model ID is gemini-4-neon on the paid tier, through generateContent or the Interactions API. Minimal Python with the Google GenAI SDK:

from google import genai

client = genai.Client()

interaction = client.interactions.create(model=”gemini-4-neon”, input=”Trace the failing checkout test and propose a fix.”, generation_config={“thinking_level”: “high”})

print(interaction.output_text)

Migration notes from Google’s docs:

1. Set thinking_level explicitly. Neon defaults to high. Carbon and Argon default to medium and cap at high.

2. Remove thinking_budget and the minimal level. Both return HTTP 400.

3. Remove temperature, top_p and top_k. In-range values are ignored, out-of-range values return 400.

4. Raise max_output_tokens. It caps thinking plus answer, so an old 8,192 cap can truncate answers. Google suggests 65,536.

5. For output past 64K, set background to true with max_output_tokens above 65,536.

6. Declare tools natively. Raw-text JSON tool calls with no declared tools can return MALFORMED_FUNCTION_CALL.

7. Remove explicit caching. Neon supports implicit caching only, on prompts of at least 4,096 tokens.

8. Set client timeouts to at least 15 minutes, or use background execution.

In Antigravity, Orchestrator can pair Neon as planner with Hydrogen as executor in one session.

Gotchas

1. The harness is part of the score. SWE-sweep moved from 12.8% to 39.0% for the same model. In your own agent loop or a third-party IDE, expect something closer to the mini-SWE number until you test it.

2. xhigh is not a free upgrade: 1 index point and 1.7 CursorBench points, a loss on GDP.pdf, and much more time and money.

3. Verbosity can eat the 25% token discount on some workloads.

4. Carbon’s price cliff. Through November 30, Carbon is cheaper per token than Neon. Rerun your cost math with December prices.

5. Cache control with TTLs arrives for Gemini 4 Hydrogen and later models on January 12, 2027, per Google Cloud docs, with cache writes billed at 1.25x input.

6. The safety evaluation was carried forward. Google says Neon’s gain over Argon fell below its Material Capability Change threshold, so it carried Argon’s frontier safety determinations forward. For biological and cybersecurity uplift those say the critical capability level “cannot be ruled out.”

Google also reports behavior numbers that speak to this week’s main complaint about GPT-6.2 Astra, which is stopping early and under-reporting. Neon showed 0.0% cheating on Terminal-Bench 4.0 and took shortcuts on 2.45% of tasks in Google’s coding cheating evaluation. Google also reports a 0.09% misaligned action rate, a 99.2% pass rate on its agent honesty evaluation, and refusals on 1.98% of Terminal-Bench 4.0 tasks. All of these are internal and none has been independently replicated.

Who should switch

Legal, tax, finance or document-heavy agent work on Opus 5.5: test Neon at high first. Every independent legal and tax row favors it, and Vals cost per test is 70% lower.

Cross-app business automation: Neon has its biggest independent margin here.

Gemini 4 Carbon users: compare Neon high against Carbon high before December 1. Both score 60 on Artificial Analysis, and Neon was faster.

Retrieval over 256K tokens: worth testing, but wait for an independent long-context number before betting on the 28-point margin.

Terminal-heavy coding or web front ends on Opus 5.5: keep Opus as default for now. Terminal-Bench, FrontierCode and WebDev Arena still lean Opus.

Highest composite score at any cost: Claude Fable 5.2 still leads Artificial Analysis (65) and the Vals Index (71.31%).

The verdict

Gemini 4 Neon does not beat Claude Opus 5.5 on every benchmark. Google’s own table shows it behind on Terminal-Bench 4.1 and FrontierCode 2.0. The independent record is 11 wins, 7 ties and 3 losses, and two of the three losses are hands-on coding.

What Neon does is match or beat Opus on most of what has been measured, while costing 10% to 70% less per task on every independent board that reports comparable costs. On raw capability it is a peer rather than a clear replacement. On price-performance it is the better buy in nearly every comparison here. The practical setting is high, not xhigh: it gives up 1 AA index point and 1.7 CursorBench points, and saves about 31% of the time per index task and 39% of the CursorBench cost.

If you’ve moved an Opus 5.5 workload to Neon at high, did your cost per completed task actually drop, or did retries and verbosity eat the 25% token discount?

*If this helped, bookmark it , and follow* *@amitmuflis* *for more Claude guides in plain English*

Check out my blog for more claude content. — Amit Shrivastava
