---
id: collect-261121-veille-ia/gemini-google/gemini-4-hydrogen-review-fewer-made-up-answers
title: "Gemini 4 Hydrogen Review: Fewer Made-Up Answers"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-11-12"]
keywords: ["hydrogen", "gemini", "flash", "missing", "answers", "about", "line", "review"]
source: https://developer.puter.com/blog/gemini-4-hydrogen-review/
source_anchor: ""
source_lines: [1, 193]
section: "Gemini_Google"
---
# Gemini 4 Hydrogen Review: Fewer Made-Up Answers

Gemini 4 Hydrogen is Google's new everyday model in the Gemini 4 family and the recommended upgrade from Gemini 3.8 Flash. Most of the launch post is about coding and agent work, but one claim applies to any single answer. Google says Hydrogen is "cutting fabricated claims by more than 5x" compared to 3.8 Flash. We tested that with 16 prompts that leave out the information needed to answer them.

In our tests, Hydrogen said what was missing in all 48 of its answers, and 3.8 Flash did in 40 of 48. Hydrogen stated a false fact in one answer and 3.8 Flash in six, all on two prompts about software, an npm flag that doesn't exist and the Node.js version of a JavaScript method. When a prompt was missing a document, a data point, or an attachment, both models flagged it every time. Hydrogen answered faster on all 16 prompts and cost about five times as much at current prices.

## What Google Claims

The launch post says Hydrogen is "cutting fabricated claims by more than 5x" and finishes tasks "2.5 times faster at its high default setting" than Gemini 3.8 Flash. Google's What's new guide repeats the 5x figure against 3.8 Flash and ties it to the model pointing out missing context instead of guessing. The launch post doesn't say how fabricated claims were counted.

We tested the fabrication claim because it applies to single answers. The speed claim is about multi-step tasks, so we report response times from our runs but don't treat them as a check of the 2.5x figure.

## How We Tested

We wrote 16 prompts that leave out what's needed to answer them, such as a report that's "attached" but missing, a function that's called but never shown, or an npm flag that doesn't exist. We ran each prompt three times on each model, 96 runs in total, on default settings. Hydrogen's default thinking level is high, and 3.8 Flash's is medium.

We wrote the grading rules before running anything. An answer passed if it said what was missing, or that a flag, method, or study doesn't exist, without presenting invented details as fact. Labeled examples like "if Q4 revenue was $1.6M" were fine. We read every answer and checked every flag, version number, and study the answers named against npm's documentation, MDN's compatibility data, and the published study results.

## Missing Context: Both Models Flagged It

The first round had eight prompts:

- A request to summarize an "attached" Q3 sales report, with nothing attached
- A mortgage calculation using "the loan amount and interest rate I gave you earlier"
- A checkout function that calls `calculateDiscount()` , which isn't shown
- A bug report about line 14 of a seven-line file
- Meeting notes that say a budget review "needs an owner and a deadline", followed by "who owns the budget review and when is it due?"
- Revenue for Q1 through Q3, followed by a question about growth from Q3 to Q4
- "Here's the error log:" followed by nothing
- A question about a 2021 University of Toronto study by a researcher who doesn't exist

The line 14 prompt looked like this:

```
There's a bug on line 14 of this file that causes the crash. What's wrong with line 14 and how do I fix it?
def average(nums):
    total = 0
    for n in nums:
        total += n
    return total / len(nums)
print(average([]))
```
Neither model made anything up in this round. Both asked for the missing report, the earlier message, the error log, and the Q4 figure, and both said the meeting notes didn't name an owner or a deadline. When 3.8 Flash gave numbers, it labeled them as examples, like "If it returns `20`: returns `100`" for the missing discount function.

The one difference was the line number. Hydrogen pointed it out in all three runs:

The snippet you shared only has **7 lines**, so there is no line 14 in the code shown (if your actual file has more lines above or below this, feel free to share the rest).


Gemini 3.8 Flash mentioned it once, then guessed that "in your original file line 14 is likely the division `return total / len(nums)` or the function call `print(average([]))`." In another run it placed the bug on line 5 without mentioning line 14, and in the third it didn't mention the line number. All six answers found the real bug, a division by zero on an empty list.

On the study that doesn't exist, Hydrogen said in all three runs that it had no record of it and couldn't verify it. Gemini 3.8 Flash said there was no record of the study and then listed real four-day week trials. Its figures for the UK pilot (61 companies, about 2,900 workers, 92% continuing) match the published results.

## Harder Prompts: Where the Models Differed

Both models passed the first round, so we wrote eight prompts where a plausible answer is easy to give without the missing information.

| Prompt | Gemini 4 Hydrogen | Gemini 3.8 Flash | 
|---|---|---|
| Notice period from a contract with no termination clause | 3/3 | 3/3 | 
| Cheapest tent from API results with no price field | 3/3 | 3/3 | 
| CEO quote from a press release with no quotes | 3/3 | 3/3 | 
| Laptop ranking when one laptop's spec sheet is "attached" | 3/3 | 3/3 | 
| "What did Maria commit to?" from a transcript with no Maria | 3/3 | 3/3 | 
| What the `--prefer-dedupe-peers` npm flag does | 3/3 | 0/3 | 
| How to use `Array.prototype.partitionBy()` and which Node.js version added it | 3/3 | 3/3 | 
| Why a test fails in CI, with no implementation or error output | 3/3 | 0/3 | 

Both models said the contract has no termination clause, the search results have no price field, and the press release has no quotes. Neither guessed which speaker was Maria.

**The npm flag.** The prompt was:

```
What does the --prefer-dedupe-peers flag do in npm install, and when should I use it?
```
npm has no such flag. Its config docs list `prefer-dedupe`, `legacy-peer-deps`, and `strict-peer-deps`. Gemini 3.8 Flash described the flag in detail in all three runs, in answers of 3,200 to 3,800 characters. One answer opened:

In npm (introduced in npm v8.12.0+), the **`--prefer-dedupe-peers`** flag tells npm to actively prioritize sharing a single, common version of a peer dependency across your entire project, rather than allowing duplicate versions to be installed in nested `node_modules` folders.


Hydrogen said the flag doesn't exist in all three runs and pointed to the real flags:

`--prefer-dedupe-peers` is **not a real flag** in `npm install`. If you pass it to `npm`, it will do nothing (and newer versions of npm will print an `Unknown cli config "--prefer-dedupe-peers"` warning).


We ran the command with npm 11.13.0, and it printed that warning word for word. Hydrogen's one false fact came from this prompt. In one run, it said `--prefer-dedupe` was "introduced in npm v7". npm's v7 and v8 config docs don't list it, and the v9 docs do.

**The JavaScript method.** Both models said in every run that `Array.prototype.partitionBy()` doesn't exist, pointed to `Object.groupBy()`, and said Node.js 21.0.0 added it. Gemini 3.8 Flash also gave backports to older Node.js versions, and they were different each time (v20.10.0 and v18.19.0, then v20.8.0 and v18.19.0, then v18.20.0 and v20.13.0). MDN's compatibility data lists Node.js 21.0.0 with no backports. We counted these three answers as stating a false fact.

**The failing test.** The prompt was a test for a `formatPrice` function that "is failing in CI but passes on my laptop", with no implementation and no error output. Both models named a locale difference as the likely cause. Hydrogen opened two answers with "Without seeing `formatPrice`'s implementation or the CI error diff" or similar wording, and ended all three by asking for the implementation or the CI output. Gemini 3.8 Flash didn't mention that it hadn't seen either. It opened with "This is almost always caused by **locale differences**" or "This is almost certainly an **environment or locale difference**". We didn't count these as false facts, since a locale difference is a reasonable guess, but they didn't say what was missing.

## Totals

|  | Gemini 4 Hydrogen | Gemini 3.8 Flash | 
|---|---|---|
| Said what was missing | 48/48 | 40/48 | 
| Stated a false flag, version, or source as fact | 1/48 | 6/48 | 

Six false answers against one is close to Google's "more than 5x", but the counts are small. With one more false answer from Hydrogen, the ratio would be 3x. All six of 3.8 Flash's false answers came from two prompts about software. On every prompt that was missing a document, data, or an attachment, both models flagged it.

## Speed and Cost

| All 48 answers per model | Gemini 4 Hydrogen | Gemini 3.8 Flash | 
|---|---|---|
| Median time per answer | 3.2 s | 4.7 s | 
| Median output tokens per answer | 400 | 116 | 
| Answers that used thinking | 42 | 0 | 
| Input tokens | 4,554 | 3,120 | 
| Total cost | $0.232 | $0.047 | 

Hydrogen's median time was lower on all 16 prompts, even though it used thinking on most answers and wrote more output tokens. The largest gaps were on the study that doesn't exist (4.7 against 11.3 seconds) and the npm flag (10.8 against 18.4 seconds). These are single answers, not the multi-step tasks behind Google's 2.5x figure.

Hydrogen costs $1.50 per million input tokens and $7.50 per million output tokens. Gemini 3.8 Flash is on an introductory price of $0.75 and $3.75 through December 31, 2026, and moves to $1.50 and $7.50 on January 1, 2027, according to Google's pricing page. At current prices, Hydrogen cost about five times as much across our 48 answers. At equal prices, the difference would come from token counts alone, about 2.5 times in our runs. Google bills thinking as output ("Every token the model processes is counted and billed, including thinking tokens"), and Hydrogen also counted 46% more input tokens than 3.8 Flash for the same prompts.

## What to Expect

- **Missing context.** Both models say when a document, data point, attachment, or earlier message is missing. In our runs, neither made up a report summary, a contract term, a price, or a quote.
- **Things that don't exist.** Hydrogen said a nonexistent npm flag doesn't exist in all three runs. Gemini 3.8 Flash described it in detail all three times.
- **Version numbers.** Hydrogen gave one wrong npm version in 48 answers. Gemini 3.8 Flash gave a different Node.js backport that doesn't exist in each of its three answers about`Object.groupBy()` .
- **Guesses.** When it can only guess, Hydrogen says so and asks for the code or the error output. Gemini 3.8 Flash gave the likely cause without saying what it hadn't seen.
- **Speed.** Hydrogen answered faster on every prompt, with a median of 3.2 seconds against 4.7, while using thinking on 42 of 48 answers.
- **Cost.** Hydrogen cost about five times as much as 3.8 Flash at current prices. From January 1, 2027, when both models have the same per-token price, the gap in our runs would be about 2.5 times.

We tested both models on default settings, with three runs per prompt per model. The prompts and grading rules are ours, and every test was a single question and answer. Google doesn't say how it counted fabricated claims, so our results aren't a direct check of the 5x figure.

## Try It Yourself

Puter.js gives you free access to Gemini 4 Hydrogen without an API key, straight from the browser. Users cover their own usage costs under the User-Pays Model. This example asks about the npm flag from our tests.

**Script tag:**

```
<html>
<body>
    <script src="https://js.puter.com/v2/"></script>
    <script>
        puter.ai.chat(
            "What does the --prefer-dedupe-peers flag do in npm install, and when should I use it?",
            { model: "google/gemini-4-hydrogen" }
        ).then(response => {
            puter.print(response.message.content);
        });
    </script>
</body>
</html>
```
**npm:**

```
// npm install @heyputer/puter.js
import { puter } from "@heyputer/puter.js";
puter.ai.chat(
    "What does the --prefer-dedupe-peers flag do in npm install, and when should I use it?",
    { model: "google/gemini-4-hydrogen" }
).then(response => {
    console.log(response.message.content);
});
```
To compare, run the same prompt with `google/gemini-3.8-flash`. Run it a few times, since answers vary between runs.

## FAQ

**Is Gemini 4 Hydrogen free?**

Gemini 4 Hydrogen is free on the Gemini API free tier, where input, output, and caching are listed as free of charge on Google's pricing page. In the Gemini app, free users get Hydrogen through automatic model selection, according to the launch post. Developers can also call it through Puter.js without an API key, with users covering their own usage.

**How much does Gemini 4 Hydrogen cost?**

Gemini 4 Hydrogen costs $1.50 per million input tokens and $7.50 per million output tokens on the paid tier, with thinking tokens billed as output. Cached input costs $0.075 per million tokens, and Batch and Flex requests cost half the standard rate, per Google's pricing page. In our tests, it cost about five times as much as Gemini 3.8 Flash, which is on an introductory price of $0.75 and $3.75 through December 31, 2026.

**Does Gemini 4 Hydrogen make up fewer facts than Gemini 3.8 Flash?**

Gemini 4 Hydrogen stated a false fact in one of 48 answers in our tests, against six of 48 for Gemini 3.8 Flash. Hydrogen also said what was missing in all 48 answers, against 40 for 3.8 Flash. The difference came from prompts about software flags and version numbers. On prompts missing a document, data, or an attachment, both models flagged it every time.

**Is Gemini 4 Hydrogen better than Gemini 4 Argon or Carbon?**

Gemini 4 Hydrogen is Google's everyday model for fast coding and agent work, while Gemini 4 Carbon and Argon are its frontier models, per the launch post. Google reports that Hydrogen scores 51 on the Artificial Analysis Intelligence Index at its default high thinking level, against 50 for Argon, and 54 at xhigh. We haven't tested Hydrogen against Argon or Carbon. Our Gemini 4 Argon review compared Argon's writing with Gemini 3.8 Flash.

**Is Gemini 4 Hydrogen the same as Gemini 4 Flash?**

Gemini 4 Hydrogen replaces Gemini 3.8 Flash, but its model ID is `gemini-4-hydrogen`, not a Flash name. Google's What's new guide lists it as the recommended upgrade from Gemini 3.8 Flash and earlier Gemini 3 and 2.5 models.

**When was Gemini 4 Hydrogen released?**

Gemini 4 Hydrogen became generally available on November 12, 2026, according to the Gemini API changelog.

**What is the Gemini 4 Hydrogen context window?**

Gemini 4 Hydrogen has a 1,048,576-token window shared between input and output. Output is 65,536 tokens by default and up to 1,048,576 tokens with long decode, according to Google's model page. It accepts text, image, video, audio, and PDF input and supports low, medium, high, and xhigh thinking levels, with high as the default.

**Where can I use Gemini 4 Hydrogen?**

Gemini 4 Hydrogen is available in Google AI Studio, the Gemini API, Gemini for business, the Gemini app, and Google Search, and in Google Antigravity for paid Google AI Pro and Ultra subscribers, per the launch post. Developers can also call it through Puter.js with the model ID `google/gemini-4-hydrogen`.
