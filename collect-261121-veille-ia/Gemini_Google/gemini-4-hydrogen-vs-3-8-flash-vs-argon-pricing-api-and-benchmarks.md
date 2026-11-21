---
id: collect-261121-veille-ia/gemini-google/gemini-4-hydrogen-vs-3-8-flash-vs-argon-pricing-api-and-benchmarks
title: "Gemini 4 Hydrogen vs 3.8 Flash vs Argon: Pricing, API and Benchmarks"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["hydrogen", "gemini", "flash", "argon", "model", "carbon", "game", "google"]
source: https://aimlapi.com/blog/gemini-4-hydrogen-vs-3-8-flash-vs-argon
source_anchor: ""
source_lines: [1, 123]
section: "Gemini_Google"
---
Google has a new fast model, Gemini 4 Hydrogen, and it's meant to take over from Gemini 3.8 Flash for everyday coding. We wanted to see whether it's worth the switch, so we compared it with 3.8 Flash and Gemini 4 Argon on published benchmarks and on three coding tasks of our own. Gemini 4 Carbon, the strongest model in the family, joined two of the tasks.

Hydrogen costs $1.50 per million input tokens on AI/ML API. That's twice as much as 3.8 Flash and 25% less than Argon, and one AI/ML API key runs all four models.

Every model passed our Python checks and built a working game. Hydrogen finished the game and the black hole page first, and it was the only one to draw a dark center on the black hole.

On an independent intelligence index, Hydrogen scores a point above Argon and 20 points above 3.8 Flash. Below you'll find the prices, our test results, the benchmarks and our advice on which model to use.

## What is Gemini 4 Hydrogen?

The Gemini 4 Hydrogen model is the fast member of Google's Gemini 4 family. Google says it's built for speed in everyday coding and agent work, and its docs call it the upgrade from Gemini 3.8 Flash. Hydrogen is adapted from Gemini 4 Carbon, and Google already runs it in Search, the Gemini app, Antigravity and Android Studio.

Hydrogen has the same 1M-token context window as Argon and Carbon, so a long document or a large part of a codebase fits into one request. Input, thinking and output all count toward that limit, and you choose how long the model thinks with the thinking level.

## What is the Gemini 4 Hydrogen price?

On price, Hydrogen sits between 3.8 Flash and the other two Gemini 4 models. The Gemini 4 Hydrogen API price on AI/ML API matches Google's, with no higher rate for long prompts, and you pay for Hydrogen, 3.8 Flash, Gemini 4 Argon and Gemini 4 Carbon from one balance.

Two things can change this picture. First, Google sells 3.8 Flash, Argon and Carbon at early prices that double later: Argon and Carbon from December 1, and 3.8 Flash from January 1, 2027. AI/ML API's model pages don't list prices after those dates yet. Second, the token price isn't the whole bill: thinking tokens are paid as output, so a model that thinks longer can cost more per task even with cheaper tokens. Our tests below show how that worked out. We checked all prices on November 12, 2026.

## How did Gemini 4 Hydrogen do in our tests?

To see how Hydrogen handles real work, we gave it three coding tasks of our own: a Python bug fix, a small 3D game and a 3D black hole page. Hydrogen, 3.8 Flash and Argon ran all three through AI/ML API, and Carbon ran the bug fix and the game.

Every model got the same prompt for each task. We ran most of the tests on November 12, 2026, and reused three runs from November 10 with the same prompts: Argon's bug fix and black hole, and Carbon's bug fix. In the tables, time is the length of the whole request, and cost is what AI/ML API charged for it, taken from our account log. Carbon ran the game at medium, its default thinking level, and the other models ran at high.

### Bug fix in Python

The first task was to fix a Python file with three small functions. Among other bugs, it counted a day as 12 hours and merged time intervals wrong. We ran 22 checks on each fix.

All four fixes passed, and the difference was in how long each model thought first. Hydrogen thought for about 6,000 tokens before it answered, and 3.8 Flash for almost 25,000. That's why 3.8 Flash took four times as long and cost twice as much here, even with its lower token price.

### 3D racing game

The second task was a small game in one HTML file with three.js: a car on an endless neon road that dodges obstacles and picks up coins.

We played each game ourselves and checked seven things from the prompt: the road and car, lane switching, crashes, coins, the score, the end screen and restarting.

All four games are playable. Hydrogen finished first with the plainest scene, a road with purple posts under a pink sun. Carbon took about five minutes and added a city skyline along the road, with twice as much code as Hydrogen. 3.8 Flash called its end screen Crashed instead of Game Over.

### 3D black hole page

The third task was a 3D black hole page in three.js. It's the prompt from our Carbon review, where Carbon at medium drew a bright ball in the center.

We checked four things on every page: a dark center, a thin bright ring around it, a disk that bends over and under the hole, and a tilt slider.

A real black hole has a dark shadow in the middle. Hydrogen was the only model that drew one, with a thin ring around it and stars bent into arcs near the hole. 3.8 Flash and Argon put a bright ball in the center instead.

Added up, the three tasks took Hydrogen about 3.5 minutes and $0.26. 3.8 Flash needed about 8 minutes and $0.28, and Argon about 8 minutes and $0.33.

## What are the Gemini 4 Hydrogen benchmark results?

Our three tasks are a small sample, so we also looked at larger benchmarks. Hydrogen has scores from two sources: the independent Artificial Analysis index and Google's own table.

### Artificial Analysis Intelligence Index

The Artificial Analysis Intelligence Index combines many reasoning, coding and agent tests into one score. Hydrogen at high thinking, its default, scores 51, and at xhigh it scores 54.

Artificial Analysis also reports what one index task cost each model at each setting. It counts Google's current prices, including the early prices of 3.8 Flash, Argon and Carbon.

3.8 Flash has the lowest token price of the four, but its index tasks cost the most. Hydrogen at high has the lowest cost per task in the table, at less than half of Argon's.

### Google's benchmarks: Gemini 4 Hydrogen vs Sonnet 5.5 and GPT-6.1 Terra

Google's table compares Hydrogen with two rivals, Claude Sonnet 5.5 and GPT-6.1 Terra. Hydrogen ran at xhigh, except on Terminal-Bench 4.1, where it ran at high. Most rival scores come from public leaderboards, and Google ran three of them itself.

Hydrogen scores higher than Sonnet 5.5 on 7 of the 10 tests and higher than Terra on 8 of the 9 tests Terra has. Sonnet 5.5 leads on both Terminal-Bench tests and on OSWorld-2.1. Hydrogen's other nine scores are at xhigh, and at high, the top level on AI/ML API, they may be lower.

### Gemini 4 Hydrogen speed

On output speed, Hydrogen leads the group. Artificial Analysis measures 170 output tokens per second for Hydrogen at high thinking, against 112 for 3.8 Flash, 75 for Carbon and 64 for Argon.

Google says Hydrogen finishes tasks 2.5 times faster than 3.8 Flash at high, but it doesn't say how it measured that. On the Artificial Analysis index, a task takes Hydrogen 8.3 minutes on average and 3.8 Flash 14.8, both at high. At high, Hydrogen's first answer token arrives after about 19 seconds.

Taken together, the Artificial Analysis numbers put Hydrogen close to Argon, well ahead of 3.8 Flash and behind Carbon, and it's the fastest of the four.

## Gemini 4 Hydrogen vs 3.8 Flash, Argon and Carbon

In code, you switch between these models by changing the model name. Here's how we'd choose.

Start with Hydrogen. It scores about the same as Argon on the index for 25% less per token and 20 points more than 3.8 Flash. In our tests, it was faster than both on the game and the black hole.

Keep 3.8 Flash only if your app already runs on it and its answers pass your checks. Its tokens cost half as much, but it thinks longer. 3.8 Flash still works in the API, and Google hasn't announced a shutdown date. If you switch, note that Hydrogen doesn't accept `reasoning_effort` set to `none`.

Move a task to Carbon when Hydrogen fails your checks. Carbon scores nine points higher on the index and its tokens cost a third more. Each Carbon run in our tests took about five minutes.

If you're not sure, run one of your own prompts on Hydrogen and on the model you use now. The Playground below lets you do that side by side.

## How to use the Gemini 4 Hydrogen API

You can try Gemini 4 Hydrogen in the browser first and then call it from code. To start, sign up on AI/ML API and add funds to your balance.

### Option 1: Test Gemini 4 Hydrogen in the Playground

Open the AI/ML API Playground with Gemini 4 Hydrogen and Gemini 3.8 Flash side by side. Type one prompt and compare the two answers before you write any code.

### Option 2: Call Gemini 4 Hydrogen through the API

Create a Gemini 4 Hydrogen API key in your AI/ML API account. AI/ML API works with the OpenAI SDK, so you only need a different base URL and model name. The code needs Python 3.10 or newer and the openai package (`pip install openai`).

`reasoning_effort` picks the thinking level: low, medium or high. We tested at high. On the Artificial Analysis index, Hydrogen scores 41 at medium and 27 at low, and a medium task costs less than half of a high one. For routine edits, try a lower level and keep the lowest one that passes your checks. The Gemini 4 Hydrogen API docs list every other setting.

## FAQ

### Is Gemini 4 Hydrogen good for coding?

For everyday tasks, yes. On Terminal-Bench 4.1, Google measured 57.0% for Hydrogen against 18.2% for 3.8 Flash. In our tests, it fixed the Python bug and built a working game in about a minute each.

### How much does the Gemini 4 Hydrogen API cost?

On AI/ML API, it's $1.50 per million input tokens and $7.50 per million output tokens, and thinking tokens count as output. Cached input costs $0.075. Each of our three test tasks cost between $0.05 and $0.13.

### How do I get a Gemini 4 Hydrogen API key?

Sign up on AI/ML API, add funds and create a key on the API keys page. The same key works for 3.8 Flash, Argon, Carbon and the rest of the catalog.

### Is Gemini 4 Hydrogen better than 3.1 Pro?

We can't compare them on data yet: we didn't test 3.1 Pro, and Artificial Analysis has no current index score for it. Hydrogen is cheaper on AI/ML API, where Gemini 3.1 Pro costs $2 per million input tokens and $12 per million output tokens. 3.1 Pro is still a preview model, and Hydrogen is generally available.

### What's the difference between Gemini 4 Hydrogen, Argon and Carbon?

They're three Gemini 4 models. Argon came first, Carbon followed as the strongest model in the family, and Hydrogen is the fast one, adapted from Carbon. On AI/ML API, Argon and Carbon cost the same per token, and Hydrogen costs 25% less than both.
