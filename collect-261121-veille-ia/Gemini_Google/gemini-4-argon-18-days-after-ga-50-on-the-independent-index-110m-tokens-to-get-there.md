---
id: collect-261121-veille-ia/gemini-google/gemini-4-argon-18-days-after-ga-50-on-the-independent-index-110m-tokens-to-get-there
title: "Gemini 4 Argon, 18 Days After GA: 50 on the Independent Index, 110M Tokens to Get There"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["argon", "index", "gemini", "high", "tokens", "october", "google", "astra"]
source: https://dev.to/gabrielanhaia/gemini-4-argon-18-days-after-ga-50-on-the-independent-index-110m-tokens-to-get-there-2cf6
source_anchor: ""
source_lines: [1, 83]
section: "Gemini_Google"
---
- 
**Book:** LLM Observability Pocket Guide
- 
**Also by me:***Thinking in Go* (2-book series) — Complete Guide to Go Programming + Hexagonal Architecture in Go
- 
**My project:** Hermes IDE | GitHub — an IDE for developers who ship with Claude Code and other AI coding tools
- 
**Me:** xgabriel.com | GitHub

Google says Gemini 4 Argon "leads the frontier on a majority of benchmarks". On the independent Artificial Analysis leaderboard, Argon (high) now scores 50. At the same high setting, Claude Opus 5.5 scores 52 and GPT-6 Astra scores 55.

Argon went generally available on the paid Gemini API on October 13, as `gemini-4-argon`. That is 18 days of real use. On October 28, Google announced Gemini 4 Carbon, a stronger model almost nobody can call yet.

Start with the leaderboard. Artificial Analysis ranks Argon (high) #13 of 58 on its Intelligence Index v5.0. It lists only the high setting for Argon, which is why the comparison above uses other models at high too. At max effort, Opus 5.5 scores 58 and Astra 57 on the same leaderboard, but Argon has no max entry to set against them.

The score has moved since R&D World's October 1 check. R&D World reported 53 at the highest reasoning setting then. Artificial Analysis has since moved to Intelligence Index v5.0, which Google's Carbon post calls "recently updated," so the two numbers may not be directly comparable. I don't know whether that change explains the whole gap.

Token use is the bigger story. Artificial Analysis says Argon generated 110M tokens to run the index, against a median of 70M, and calls that "somewhat verbose." At launch, R&D World put it at about 62,000 output tokens per index task versus about 27,000 for GPT-6 Astra.

That verbosity is what makes the price promo matter. In the same R&D World piece, Argon cost $1.99 per index task at the introductory price and $3.98 at the standard price. Astra cost $3.26. On October 31 the leaderboard lists $7.68 per task for Argon (high), next to $3.46 for Opus 5.5 (high) and $3.24 for Astra (high). Those current numbers run on the v5.0 index, where Argon used 110M tokens, and Artificial Analysis prices Argon at the $4 / $20 per million tokens standard rate rather than today's promo. Both $3.98 and $7.68 use that list price, so the jump likely reflects the new index version; on its own it doesn't show the model got worse.


*Launch charts measure the score. Your bill measures the tokens it took to get there.*

Google's own table already showed the coding gaps. Argon scores 55.0% on FrontierSWE v2 against Astra's 65.5%, and 57.4% on Terminal-Bench 4.0 against Opus 5.5's 66.4%, per R&D World's reading of Google's numbers. The same article reports that on Arena's agent leaderboard Argon ranked 8th overall when checked on October 1, from 3,417 sessions, with a rank range of 3rd to 16th.

These are anecdotes from small groups, not studies. Read them as signals.

Every's "vibe check" panel, with Dan Shipper and his team, was the most detailed early test, as summarised by BigGo. Argon was a strong structural editor. Shipper said it shaved days off an 8,000-word piece, and it generated more ideas than rival models in the panel's 3D and game tests. Where it fell down: it was weaker than Claude at checking and revising its own code. One racing game it built had a wall that made the track unplayable. Its writing tested about two grade levels harder to read than rivals'. Shipper called the Antigravity harness "a terrible harness."

On Google's own developer forum, a thread titled "How bad is Gemini 4 Argon?" opened on October 16. The first post called its coding "underwhelming." A later reply described a large codebase where Argon finished a task, then kept rereading old specs and tests and started doubting work that was correct. That poster said it "prioritizes rules over correctness." Another reply granted that it is strong at knowledge work.

A separate October 20 thread listed three problems: audio uploads not recognised in the Gemini web app, repeated approval prompts during pull request workflows in Antigravity, and quota that runs out fast during normal development. A reply the same day asked for reproduction steps, the Antigravity version and the OS.

Not every test went against it. A Puter writing test of 140 runs, published October 19, found Argon answered opinion questions more directly than Gemini 3.8 Flash and hedged less, though it missed length limits more often.

The forum reports and the Every panel point at the same weak spot: it doesn't check its own code well.

Gemini 4 Carbon is not something you can test. Google's announcement says it goes first to "a subset of cyber defenders," then to trusted partners through the Fairwind Program, and to developers and businesses after more work on guardrails, with no date. It was not in the Gemini API changelog as of October 31.

One number does come from outside Google. Artificial Analysis lists Carbon (high) at 59, #1 on its index, and Google quotes that score in its post. The same page shows Carbon generated 200M tokens to run the index, against a 70M median, at $7.76 per task. So the verbosity pattern carries over.


*Carbon on October 31: announced, benchmarked, and behind the rope for most developers.*

Everything else in the post is Google's own testing: SEC-bench Pro 88.2%, ExploitGym 54.8%, CWE-bench 71.0%, about half as many unsupported claims as Argon, 26% less overreach, and 0.0% attack success on Gray Swan's indirect prompt injection benchmark against Argon's 0.7%. Those may hold up. Nobody outside the access programs can check them yet. Treat them the way you would have treated Argon's launch table on day one.

Argon costs $2 / $10 per million input / output tokens through November 30, then $4 / $20 from December 1. It is not on the free tier. Output pricing includes thinking tokens, which is where a verbose model costs you.

So measure tokens per task on your own prompts, and price them at both rates. With the Google Gen AI SDK for Python:


```
from google import genai
client = genai.Client()  # reads GEMINI_API_KEY
resp = client.models.generate_content(
    model="gemini-4-argon",
    contents="Review this diff for bugs: ...",
)
u = resp.usage_metadata
inp = u.prompt_token_count or 0
out = (u.candidates_token_count or 0) + (
    u.thoughts_token_count or 0
)
now = inp * 2 / 1e6 + out * 10 / 1e6
dec = inp * 4 / 1e6 + out * 20 / 1e6
print(f"in={inp} out={out}")
print(f"${now:.4f} now, ${dec:.4f} from Dec 1")
```
Run your ten most common tasks through it, and the same tasks through the model you use today. Compare cost per finished task, not price per token.

If you'd rather compare agents than raw API calls: I build Hermes IDE, so weigh this with that in mind. Hermes runs Antigravity CLI as a built-in agent next to Claude Code and Codex. Its model list comes from `agy models`, so Argon shows up if your Google account has it (Antigravity offers it on Google AI Pro, Ultra and Enterprise plans). Each task can get its own branch and worktree, so you can give Antigravity and Claude Code the same ticket on one repo. For the cost side, use the snippet above. Hermes is not affiliated with Google.


*The ⌘N agent picker in Hermes 2.3.4, with Antigravity CLI among the supported agents.*

Have you moved real work to Argon since October 13? Did your token counts match the leaderboard's, or your own expectations? Tell me in the comments.

If you want to try Argon next to the agents you already use, Hermes IDE is free on macOS, Windows and Linux, and the source is on GitHub.

Benchmark scores don't tell you what a model costs on your traffic. The LLM Observability Pocket Guide covers how to trace token use, cost and quality per request, and how to pick the tools that do it, so a model switch is a measured decision.

## Top comments (0)
