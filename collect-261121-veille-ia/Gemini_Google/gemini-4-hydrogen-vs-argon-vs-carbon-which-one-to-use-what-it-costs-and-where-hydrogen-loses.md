---
id: collect-261121-veille-ia/gemini-google/gemini-4-hydrogen-vs-argon-vs-carbon-which-one-to-use-what-it-costs-and-where-hydrogen-loses
title: "Gemini 4 Hydrogen vs Argon vs Carbon: Which One to Use, What It Costs, and Where Hydrogen Loses"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-11-12"]
keywords: ["hydrogen", "google", "argon", "gemini", "carbon", "model", "costs", "loses"]
source: https://www.hyperautomationlabs.co/blog/gemini-4-hydrogen-vs-argon-vs-carbon-which-to-use-price
source_anchor: ""
source_lines: [1, 143]
section: "Gemini_Google"
---
# Gemini 4 Hydrogen vs Argon vs Carbon: Which One to Use, What It Costs, and Where Hydrogen Loses

Google now has three Gemini 4 models named after elements, and the names don't tell you which one to use. Here's the short answer. Gemini 4 Hydrogen, released November 12, 2026, is the cheapest of the three, it scored 51 on a major independent test against Argon's 50, and free users get it too. Make it your default, and move up only when it gets stuck.

But there's one test where it clearly loses. If you write code, that's the one you need to see. Below: what each model is, what it costs, what Hydrogen built in Google's demos, where it wins and loses, and exactly which one to pick on any plan.

## Hydrogen, Carbon, Argon: the names don't help

On the periodic table, Argon is the heaviest of the three. In Google's lineup, the most powerful model is Carbon. So forget the chemistry. Think of them as a crew.

- **Carbon is the specialist** you call for the hardest problems. Google says it uses more tokens than Argon, so it can explore, reason deeply and check its own work for longer. (A token is a small chunk of text, roughly three quarters of a word. You pay per token.)
- **Argon is the senior engineer.** Google calls it the ideal choice for the majority of software engineering and knowledge work.
- **Hydrogen is the fast one that gets the work done.** It's built for quick execution: writing code, running tests, checking changes, and your everyday tasks.

## What they cost

| Model | Input / output per 1M tokens | Notes | 
|---|---|---|
| Hydrogen | $1.50 / $7.50 | Flat across the full 1M token context window | 
| Argon | $2 / $10 | Introductory. Doubles to $4 / $20 after Nov 30, 2026 | 
| Carbon | $2 / $10 | Introductory. Uses the most tokens of the three | 

So after November 30, Hydrogen costs less than half of what Argon will. A cheaper model that scores like a premium one sounds too good. So let's check.

## How big the jump from Gemini 3.8 Flash is

Hydrogen replaces Gemini 3.8 Flash, Google's previous fast model. On Terminal-Bench 4.1, which tests whether a model can finish real tasks in a computer terminal, Flash scored 18.2%. Hydrogen scores 57%. That's more than three times higher.

Google also says it finishes tasks two and a half times faster, cuts made-up claims by more than five times, and keeps going when a tool fails or a search comes back empty. Outside companies saw it too. Box says it ran about twice as fast with 12% fewer tokens. Pixelcut ran it 276 times on design briefs, and it passed every one.

## Did it really beat Argon?

Artificial Analysis runs an independent intelligence index across many tests. At its default setting, Hydrogen scores **51**. Argon scores **50**. Turn Hydrogen up to its highest setting and it reaches 54.

To be fair: one point is basically a tie. The real story isn't that Hydrogen is smarter than Argon. It's that you now get Argon-level scores for a much lower price. Google's own chart also shows GPT-6.1 Terra reaching a similar score for less money per task. But on the time chart, Hydrogen gets there in well under half the time.

## What Hydrogen built in Google's demos

Scores are just numbers. Here's what Hydrogen built in Google's own demos:

- **A full product store in one shot** in Google Antigravity, using Google's new front end design skill, with a 3D shader, scroll animations and a live color picker.
- **A Google Slides deck** from a YouTube keynote and a design template. Then it checked its own slides and fixed the overlapping text by itself.
- **An interactive financial model in Google Sheets** with what-if sliders. Google says over 30% faster than previous models.
- **Animated scenes inside a live spreadsheet** , by shrinking thousands of cells into pixels. A party trick, but it shows how far it will follow a strange instruction.
- **A 3D globe that plays music from live lightning strikes** , a glass and water simulator from pure math, and a short film about hydrogen it edited from public domain footage.

## The scoreboard vs Claude Sonnet 5.5 and GPT-6.1 Terra

Google put Hydrogen next to its real rivals. All numbers below are Google's published figures.

| Test | Hydrogen | Claude Sonnet 5.5 | GPT-6.1 Terra | Winner | 
|---|---|---|---|---|
| SWE-sweep (find and fix bugs) | 13.5% | 3.1% | 5.8% | Hydrogen | 
| Vending-Bench 2 (run a pretend business) | $14,549 | $11,165 | $9,107 | Hydrogen | 
| Terminal-Bench 4.1 (terminal coding) | 57% | 63.9% | n/a | Sonnet | 
| OSWorld-2.1 (controlling a computer) | 77.6% | 83.9% | n/a | Sonnet | 
| Terminal-Bench-Science | 51.4% | 59.9% | n/a | Sonnet | 

SWE-sweep is a brutal test, with low scores everywhere, but Hydrogen is clearly ahead. It also leads on Agents' Last Exam (71.6%), on reading charts, and on HLE-Diamond (58.8%), a very hard reasoning test.

And here's the test it loses. On Terminal-Bench 4.1, Claude Sonnet 5.5 beats it 63.9% to 57%. Sonnet also wins on OSWorld, where the model controls a real computer, and on a science and math terminal test. So if your work is long coding sessions in the terminal, or an agent clicking around your computer, Hydrogen is not automatically the best pick. On most of the rest of the table, it leads.

## Thinking levels: which to use

One setting changes all of these results: the thinking level. Hydrogen has four. Low, medium, high (the default) and a new extra high. More thinking means better answers, but slower and more expensive. On Google's chart, going from high to extra high adds three points and costs noticeably more per task.

Our rule: leave it on high. Use low for quick, simple jobs like sorting or rewriting. Save extra high for the one problem you're stuck on.

## Use two models: Orchestrator

The smartest move isn't picking one model. It's using two. Google added a feature to Antigravity, its AI coding tool, called Orchestrator. It switches between Carbon and Hydrogen in the same session, on its own.

On Terminal-Bench 4.0, that combo came close to Carbon's own success rate while using 24% less quota and finishing 19% faster. On bigger projects, Carbon can act as the coordinator while Hydrogen runs the workers and checkers that build and test changes in parallel. A senior architect with a fast crew.

## Gemini app and Google Search

If you don't code, this part is for you. In the Gemini app, Hydrogen follows your tone, format and rules more closely, so emails and study guides come back ready on the first try. It pulls context from your uploads, from Gmail and Google Drive, and from connected apps like Airtable and Dropbox, and it picks the right tool more often. Paid users can choose it from the model menu. Free users get it automatically, when Gemini decides your prompt needs it.

It's also now powering Google Search, for everyone. In Google's example, someone asks how to make a tiny studio apartment feel like two rooms for under $200. The new answer gives a real price range for each idea, with photos. Ask which Boston neighborhood to live in after a move, and you now get a map and a side-by-side comparison table. Google says Search also understands messy questions, with typos and several parts, and links to a wider range of sites.

## Can you trust it?

Can you trust it to act on its own? Google says Hydrogen goes beyond what you asked up to 81% less often, and it scored 96.7% on Google's internal honesty test for agents. It's more than twice as resistant to hidden prompt injection attacks (a web page hiding instructions meant to hijack the AI). And on HealthBench Pro, it wrongly refuses medical questions under 0.4% of the time.

## The cheat sheet: which one to pick

- **Hydrogen:** your default. Emails, research, search, quick builds, and any coding loop where speed and cost matter. On the API, it's the cheapest of the three.
- **Argon:** serious, multi-step work where quality matters more than speed.
- **Carbon:** your hardest problems only. Big multi-step builds, research and security work. It thinks the longest and uses the most tokens.

| Plan (Gemini app) | Models you get | 
|---|---|
| Free | Hydrogen, via auto selection | 
| Google AI Pro | Hydrogen + Argon | 
| Ultra | Hydrogen + Argon + Carbon | 

Our recommendation: make Hydrogen your default, and only move up when it gets stuck.

## 3 things to try today

1. Open Google AI Studio, pick Gemini 4 Hydrogen, and drop in a long PDF or spreadsheet. Ask for a summary with sources and a table.
2. Give Google Search a messy question with several parts, typos and all, and see what comes back.
3. If you use Google Sheets on a Pro or Ultra plan, ask Gemini to turn your budget into an interactive model with what-if sliders.

Sources (read November 12, 2026): Google's Introducing Gemini 4 Hydrogen, Introducing Gemini 4 Argon and Gemini 4 Carbon is now available.

## Get the Gemini 4 Model Picker

We put all of this on one page: which model to use, the prices, which plan gets which, where Hydrogen loses, and the prompts for those three tests. Comment ELEMENT on the video, or grab it below.

How we sourced this: we did not run our own tests on Hydrogen. All numbers are Google's published figures. "One point is basically a tie", the thinking-level rule and "make Hydrogen your default" are our own takes. Independent, not affiliated with Google, Anthropic or OpenAI.

## Free PDF: The Gemini 4 Model Picker

The Gemini 4 Model Picker: which model to use, the prices, which plan gets which, where Hydrogen loses, and 3 test prompts, on one page.

Join **27,000+ readers** of the Hyperautomation AI Report

## Quick answers

## Is Gemini 4 Hydrogen better than Gemini 4 Argon?

On the Artificial Analysis Intelligence Index, Hydrogen scores 51 at its default setting and Argon scores 50. Turned up to its highest setting, Hydrogen reaches 54. One point is basically a tie, so the real story is that you get Argon-level scores for a much lower price.

## How much does Gemini 4 Hydrogen cost?

Hydrogen costs $1.50 per million input tokens and $7.50 per million output tokens, and that price is flat across its full 1 million token context window. Argon and Carbon both cost $2 / $10 at their introductory price. For Argon that ends November 30, 2026, when it doubles to $4 / $20.

## Can I use Gemini 4 Hydrogen for free?

Yes. Free users of the Gemini app get Hydrogen automatically, when Gemini decides your prompt needs it. Google AI Pro gets Hydrogen and Argon, and Ultra gets all three, including Carbon. Hydrogen is also now powering Google Search for everyone.

## Is Gemini 4 Hydrogen better than Claude Sonnet 5.5 for coding?

It depends on the coding. On Terminal-Bench 4.1, the terminal coding test, Claude Sonnet 5.5 wins 63.9% to 57%, and Sonnet also wins on OSWorld-2.1 (83.9% vs 77.6%). But Hydrogen leads on SWE-sweep, a find-and-fix-bugs test, with 13.5% against Sonnet's 3.1% and GPT-6.1 Terra's 5.8%.

## What is the difference between Gemini 4 Hydrogen, Argon and Carbon?

Hydrogen is the fast, cheap model for everyday work and quick coding loops. Argon is the one Google calls the ideal choice for most software engineering and knowledge work. Carbon is Google's strongest model, for the hardest problems, and it uses the most tokens.

## Which thinking level should I use with Gemini 4 Hydrogen?

Hydrogen has four levels: low, medium, high (the default) and a new extra high. On Google's chart, going from high to extra high adds three points and costs noticeably more per task. Our rule: leave it on high, use low for quick jobs like sorting or rewriting, and save extra high for the one problem you're stuck on.
