---
id: collect-261121-veille-ia/gpt-openai/gpt-6-1-terra-pricing-benchmarks-and-terra-vs-sol-vs-luna
title: "GPT-6.1 Terra: Pricing, Benchmarks, and Terra vs Sol vs Luna"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: ["2026-10-26"]
keywords: ["terra", "gpt-6", "luna", "openai", "price", "writing", "tokens", "index"]
source: https://www.lindy.ai/blog/gpt-6-1-terra
source_anchor: ""
source_lines: [1, 260]
section: "GPT_OpenAI"
---
OpenAI just released GPT-6.1 Terra, a cheaper sibling to GPT-6.1 Sol built for everyday work like reading screens, filling in forms, and writing clear drafts. It costs half as much as Sol, though how much you save depends on how hard you push it.

I went through OpenAI's docs and Artificial Analysis's independent tests to see where it holds up. Here's what Terra costs, how it stacks up against Sol and Luna, and where you can use it today.

**TL;DR:**

- **What it is:** OpenAI's fast, lower-cost GPT-6.1 model for computer use, visual reasoning, and writing.
- **Released:** October 26, 2026.
- **API price:** $1 per million input tokens, $0.05 cached, and $5 output, which is half of GPT-6.1 Sol's rates.
- **Where to use it:** ChatGPT Work and Codex on Plus, Pro, Business, Enterprise, and Edu, plus the API (gpt-6.1-terra), GitHub Copilot, and Amazon Bedrock. It isn't in regular ChatGPT chat.
- **Biggest caveat:** It's best at max effort, but that's also where it uses the most tokens, so your bill ends up closer to Sol's than you'd expect.

## What is GPT-6.1 Terra?

**GPT-6.1 Terra is OpenAI's fast, lower-cost GPT-6.1 model, built for computer use, visual reasoning, and clear writing at half the price of GPT-6.1 Sol.** OpenAI's launch post calls it "a significant step up" from GPT-5.6 Terra, the version it released in July.

It sits between Luna and Sol in the GPT-6 lineup. As OpenAI's Ted Sanders put it on Hacker News, the order is "Luna < Terra < Sol < Astra," and if you're "very price sensitive," pick Terra or Luna.

**The specs that matter,** from OpenAI's model page:

- **Inputs and outputs:** It reads text and images and writes text. Audio and video aren't supported.
- **Context:** The window holds 1.05 million tokens, with up to 922,000 for input and 128,000 for output.
- **Knowledge cutoff:** June 1, 2026, about a month newer than Astra and Sol (April 30, 2026).
- **Reasoning effort:** You can pick none, low, medium (the default), high, xhigh, or max.

## GPT-6.1 Terra vs Sol vs Luna: which one to use

**Pick Sol for your hardest coding and reasoning work, Terra for high-volume screen, image, and writing tasks, and Luna when price matters more than quality.** Prices come from OpenAI's pricing page, and scores come from Artificial Analysis's model pages (Intelligence Index v5.0, max effort, checked October 28):

| 🧩 | 🌍 GPT-6.1 Terra | ☀️ GPT-6.1 Sol | 🌙 GPT-6 Luna | 
|---|---|---|---|
| Built for | Computer use, images, writing | Complex coding and professional work | Focused, high-volume tasks | 
| API price (input / output per 1M) | $1 / $5 | $2 / $10 | $0.10 / $0.50 | 
| Cached input per 1M | $0.05 | $0.10 | $0.01 | 
| Intelligence Index score | 51 | 55 | 28 | 
| Cost per index task | $0.96 | $1.27 | $0.15 | 
| Output speed | 91 tokens/sec | 81 tokens/sec | 124 tokens/sec | 

### Terra vs Sol

**Sol is the smarter model, but Terra is the better deal for a lot of everyday work.** OpenAI admits as much in the launch post: "For the most complex coding tasks, GPT-6.1 Sol and GPT-6 Astra remain the better choice." Sol leads Terra by four points on the Artificial Analysis Intelligence Index and by six on its Coding Agent Index (54 to 48).

Terra wins when the job is mostly screens, images, or writing. OpenAI says it finishes computer-use tasks on its Agents' Last Exam test more than three times faster than Sol, and Sanders thinks it's "probably better than Astra and Sol at technical communication." It also makes a ChatGPT plan go further. For more on where Sol earns its higher price, see our GPT-6.1 Sol breakdown.

### Terra vs Luna

**Terra is much smarter, and at medium effort it costs about the same per task as Luna does at max.** On Artificial Analysis's index, medium Terra scores 38 for $0.17 a task, and max Luna scores 28 for $0.15. Even Terra on low (29 points, $0.07 a task) edges out Luna on max, though that's Terra's cheapest setting against Luna's priciest.

Luna still wins on speed and per-token price. At $0.10 per million input tokens, it's ten times cheaper than Terra, so it's still the pick for short, repetitive jobs like sorting tickets or tagging documents, where Terra's extra brainpower would go to waste. Luna also powers OpenAI's Decisions API, and our Gemini Decision 1 vs GPT-6 Luna comparison pits it against Google's decision model; both score options rather than writing text.

## GPT-6.1 Terra pricing

**GPT-6.1 Terra costs $1 per million input tokens and $5 per million output tokens on the API.** That's half of GPT-6.1 Sol's rates and well under the old GPT-5.6 Terra's $2 and $12. Here's the full rate card from OpenAI's pricing page:

| ⚙️ Mode | 📥 Input | 🗂️ Cached input | ✍️ Cache writes | 📤 Output | 
|---|---|---|---|---|
| Standard | $1.00 | $0.05 | $1.25 | $5.00 | 
| Batch or Flex | $0.50 | $0.025 | $0.625 | $2.50 | 
| Fast | $2.00 | $0.10 | $2.50 | $10.00 | 
| Standard, prompts over 272K tokens | $2.00 | $0.10 | $2.50 | $7.50 | 

All prices are per million tokens. Cache writes cost $1.25 to keep the cache for 30 minutes, or $2.50 for 12 hours. Regional processing adds 10%. Agent builders that run on these models add their own charges; our Voiceflow pricing breakdown shows how seats and LLM tokens stack up.

### Caching cuts the input bill by 95%

**Cached input costs $0.05 per million tokens, or 5% of the normal input rate.** That's a quarter of what GPT-5.6 Terra charged, according to Artificial Analysis.

This helps most with agents that send the same instructions and files on every step. If an agent carries 100,000 tokens of context into each step, that's $0.10 a step without caching and half a cent with it. Writing to the cache costs $1.25 per million tokens, though, so you only save when the agent reuses that context. DeepSeek charges less for cache hits, which our DeepSeek V4 Flash pricing breakdown covers.

### What a task costs at max effort

At max effort, Terra writes a lot more, which eats into the half-price discount. **Artificial Analysis measured $0.96 per test task for Terra, only about 24% less than Sol's $1.27,** because Terra wrote 99 million output tokens across the test suite against Sol's 64 million.

Here's the cost per task across OpenAI's lineup and Claude Opus 5.5:

| 🤖 Model (max effort) | 🧠 Index score | 💵 Cost per task | 
|---|---|---|
| GPT-6.1 Terra | 51 | $0.96 | 
| GPT-6.1 Sol | 55 | $1.27 | 
| GPT-6 Astra | 57 | $6.24 | 
| Claude Opus 5.5 | 58 | $10.04 | 
| GPT-6 Luna | 28 | $0.15 | 

**Dropping one notch to xhigh changes the math.** Terra scores 45 there for $0.40 a task, less than half its max-effort cost. (On an older version of its index, Artificial Analysis's launch-day write-up also found max-effort Terra cost less than a third as much per task as GPT-5.6 Terra.)

### What Terra costs in ChatGPT

**In ChatGPT, you pay for the plan, and Terra makes your usage last longer.** OpenAI's help center estimates a Business Standard seat gets 35 to 285 local messages every five hours with Terra, compared with 15 to 150 on GPT-6.1 Sol and 5 to 45 on GPT-6 Astra. Premium seats get five times that, with no five-hour limit.

That's about double Sol's range, which matches what OpenAI's Thibault Sottiaux said in a launch-day post: "2X more usage than 6.1 Sol across API and subscriptions." For plan prices, see our full ChatGPT pricing guide.

## GPT-6.1 Terra benchmarks

OpenAI mostly compares Terra with the older GPT-5.6 Terra, so it helps to read its numbers next to independent tests.

### What OpenAI claims

- **Agents' Last Exam (computer use):** At medium effort, Terra scores 15 points higher than GPT-5.6 Terra for about a third of the cost, and matches GPT-6 Astra at a tenth of Astra's per-token price. At max effort, OpenAI says it uses less than a third as many tokens as Claude Opus 5.5 at a quarter of the per-token price, so each task costs about 9x less.
- **Terminal-Bench 4.0 (coding):** At max effort, Terra solves 50.3% of tasks, more than double GPT-5.6 Terra's 21.5%.
- **Terminal-Bench-Science 0.1:** At max effort, it beats GPT-6.1 Sol for less money and lands within five points of Opus 5.5 for under a fifth of the estimated cost.
- **MMMU-Pro (visual reasoning):** It beats GPT-5.6 Terra at every reasoning setting and nearly matches Astra and Sol.
- **OSWorld 2.1 (desktop apps):** At medium effort, it tops GPT-5.6 Terra's best score.
- **Factual errors:** On OpenAI's internal test of tricky prompts, Terra's error rate at low effort is 16.7%, down from 53.6% for GPT-5.6 Terra. At max effort, it's 4.3%, nearly as low as Astra and Sol.

### What independent testing shows

**Artificial Analysis gives GPT-6.1 Terra a 51 on its Intelligence Index (v5.0) at max effort, 11th out of 51 comparable models.** That's four points behind GPT-6.1 Sol, six behind GPT-6 Astra, and seven behind the top model in that group, Claude Opus 5.5. (We compared Opus 5.5 and Astra head to head in our Opus 5.5 vs Astra guide.)

**The score falls fast as you lower the effort setting:**

| 🎚️ Effort | 🧠 Index score | 💵 Cost per task | 📝 Output tokens on the test suite | 
|---|---|---|---|
| Max | 51 | $0.96 | 99M | 
| Xhigh | 45 | $0.40 | 35M | 
| High | 42 | $0.26 | 21M | 
| Medium (API default) | 38 | $0.17 | 14M | 
| Low | 29 | $0.07 | 7.2M | 

Medium is the API's default, so that 13-point drop means out-of-the-box Terra is closer to Luna than to Sol. Pick your effort level on purpose.

**Two independent results look good for Terra:**

- **Fewer made-up answers:** On Artificial Analysis's AA-Omniscience test, Terra guesses 38% of the time when it doesn't know the answer, down from 88% for GPT-5.6 Terra, and it gets slightly more questions right.
- **Coding:** It scores 48 on the Coding Agent Index v2.0, six points under Sol and 17 above Luna, and Artificial Analysis says it now rivals Sol on Terminal-Bench 4.0 at max effort.

### How fast is GPT-6.1 Terra?

**Artificial Analysis clocks GPT-6.1 Terra at about 91 output tokens per second, faster than Sol (81) and Astra (71) and slower than Luna (124).** That's slower than the old GPT-5.6 Terra, which ran at about 132.

**How long you wait for the first word depends on effort, and Terra starts faster than Sol at every setting.** Here's Artificial Analysis's time to first token on OpenAI's API, checked October 28:

| 🎚️ Effort | 🌍 GPT-6.1 Terra | ☀️ GPT-6.1 Sol | 
|---|---|---|
| Low | 1.3 sec | 2.4 sec | 
| Medium | 1.7 sec | 3.6 sec | 
| High | 5.7 sec | 40.8 sec | 
| Xhigh | 17.5 sec | 106.4 sec | 
| Max | 78 sec | 278 sec | 

On low or medium, Terra starts answering in under two seconds, which is fast enough for chat or a live agent. On max, you wait over a minute, so save that for background jobs.

OpenAI's "three times faster" claim is about something else. According to OpenAI and AWS, Terra *finishes* Agents' Last Exam tasks more than three times faster than Astra and GPT-6.1 Sol. That counts the whole job, including how many steps it takes, so expect the biggest speedup on screen-based work and less on long reasoning tasks at max effort, where Terra gets wordy.

## What GPT-6.1 Terra is built for: computer use

**Computer use means the model looks at a screenshot, decides what to click or type, and does it, one step at a time.** OpenAI built Terra for this, along with reading images and writing. Its model guide suggests jobs like matching product photos to catalog entries and working inside app screens.

OpenAI's own example is invoice entry. Picture a finance assistant at a 40-person company who spends every Monday typing supplier invoices into accounting software. Hand that to Terra, and the job looks like this:

1. **Read the invoice:** Terra opens the PDF and pulls the supplier name, invoice number, line items, tax, and due date.
2. **Find the right record:** It opens the accounting app, searches for the supplier, and starts a new bill.
3. **Fill the form:** It types each field, matches line items to the right expense accounts, and attaches the PDF.
4. **Stop at the edge cases:** Anything that doesn't match, like a supplier that isn't on file or totals that don't add up, goes to a person for review.

The low price is what makes this work at volume. Say each invoice takes 20,000 input tokens (screenshots and steps) and 2,000 output tokens. A thousand invoices would then cost about $30 on Terra, $60 on Sol, and $3 on Luna, if Luna could do the job as well. Your real numbers depend on the app, the number of steps, and the effort level.

Writing is the other big selling point. OpenAI says Terra writes "clearer, more natural" text for jobs like drafting proposals, and Sanders thinks it beats even Astra at technical communication.



## How to access GPT-6.1 Terra

You can use GPT-6.1 Terra in ChatGPT Work, Codex, the OpenAI API, GitHub Copilot, and Amazon Bedrock. **It isn't available in regular ChatGPT conversations,** per OpenAI's help center.

### In ChatGPT Work and Codex

Terra is rolling out in ChatGPT Work and Codex on Plus, Pro, Business, Enterprise, and Edu plans, per OpenAI's release notes. On Enterprise, access also depends on your workspace admin's settings. OpenAI doesn't list Free or Go plans for Terra, and those plans get GPT-6 Luna in the Codex desktop app instead, according to OpenAI's Codex pricing page.

**To switch to it:**

- **ChatGPT desktop app and Work on the web:** Open**Advanced** in the Power setting and choose GPT-6.1 Terra from the model list.
- **Codex CLI:** Update to the latest version (Terra needs 0.159.0 or later), then run codex -m gpt-6.1-terra, or use /model inside a session, per OpenAI's Codex model docs.

### In the API

Developers call it as gpt-6.1-terra through the Responses API or Chat Completions, plus Batch. It supports computer use, web search, file search, code interpreter, MCP, skills, and image inputs. Fine-tuning isn't supported.

**If you're moving from GPT-5.6 Terra, OpenAI's migration notes boil down to three steps:**

1. **Start at medium effort, then test xhigh:** Medium is the default, and OpenAI suggests raising it when the quality gain is worth the extra time and tokens. On Artificial Analysis's index, xhigh lifts the score from 38 to 45.
2. **Move tool calls to the Responses API:** Tool calling doesn't work through Chat Completions.
3. **Test on your own images:** Compare both models on real screenshots and documents at the same effort level before you switch.

### In GitHub Copilot

**GPT-6.1 Terra is generally available in GitHub Copilot for Pro, Pro+, Max, Business, and Enterprise users,** according to GitHub's changelog. You can pick it in the model picker in VS Code, Visual Studio, JetBrains IDEs, Xcode, Eclipse, the Copilot CLI, the cloud agent, the GitHub Copilot app, github.com, and GitHub Mobile.

It's billed at provider list pricing under usage-based billing, and it's rolling out gradually. It's on by default, so Business and Enterprise admins who don't want it can switch it off in Copilot's model policy settings. Comparing coding tools more broadly? See our roundup of AI coding agents.

### On Amazon Bedrock

AWS made GPT-6.1 Terra generally available on Bedrock on launch day. You can call it through Bedrock's APIs or set ChatGPT Work and Codex to use it through Bedrock.

## Where GPT-6.1 Terra falls short

Terra is a good deal for the right jobs, but **here's what to know before you move work onto it:**

1. **The default effort setting is weak.** At medium, it scores 38 on Artificial Analysis's index, closer to Luna's 28 than to Sol's 55.
2. **Max effort eats most of the discount.** It writes about 55% more output tokens than Sol on the same test suite, which leaves the cost per task only about a quarter lower.
3. **Sol and Astra still win on hard coding.** OpenAI says so in its own launch post, and Sol leads Terra on Artificial Analysis's Coding Agent Index.
4. **It isn't in regular ChatGPT chat.** If you mostly chat, you'll need to switch to Work or Codex to use it.
5. **It's a modest upgrade for subscribers.** Sanders put it plainly: "For subscribers, it's not a bombshell release." Its main perk on a plan is stretching your usage limits, which helps most if you keep hitting them.
6. **It's slower per token than the old Terra.** About 91 tokens per second, against roughly 132 for GPT-5.6 Terra.
7. **Its safety profile is mixed.** It drew the fewest serious misalignment flags of the models OpenAI compared, but more low-severity flags than Sol and Astra on a few risky behaviors.

## Is GPT-6.1 Terra safe? The Critical cybersecurity rating

**OpenAI rates GPT-6.1 Terra Critical for cybersecurity under its Preparedness Framework,** the same as GPT-6 Astra and GPT-6.1 Sol. The system card addendum rates it High for biological and chemical capability and below High for AI self-improvement.

That means Terra runs behind the same safety checks as Sol and Astra, which OpenAI says can sometimes slow, pause, or stop legitimate work, including defensive security work.

The behavior tests cut both ways.

In OpenAI's misalignment testing, Terra drew the fewest serious flags (severity 3 or higher) of the five models: 22, against 24 for Sol and 26 for Astra.

At lower severity, it had the lowest rates for ignoring instructions, deception, and reward hacking. But it was flagged more often than Sol and Astra for hiding uncertainty, getting around restrictions, exfiltrating data, and destructive actions. It held up against prompt injection about as well as Sol and Astra.

Keep a person in the loop for anything it can't undo.

## Should you use GPT-6.1 Terra?

**If you build agents that read screens, documents, or images at volume, yes, and it'll probably become your default.** You get most of Sol's ability on that kind of work at half the token price. The sweet spot is xhigh: 45 on Artificial Analysis's index for $0.40 a task.

If you write a lot of code or run hard reasoning jobs, stay on Sol. It leads by six points on Artificial Analysis's Coding Agent Index, and at max effort Terra only saves you about a quarter. On a ChatGPT plan, switch to Terra in Work or Codex when you keep hitting your limits. And if you only use regular chat, you can skip this one for now.

**One last tip:** if your team uses an AI teammate in Slack like Lindy, which lets you pick the model for each task, check which OpenAI models show up in your workspace before you plan around Terra.



## Frequently asked questions about GPT-6.1 Terra

### What is GPT-6.1 Terra?

**GPT-6.1 Terra is OpenAI's fast, lower-cost GPT-6.1 model for computer use, visual reasoning, and writing,** released on October 26, 2026. It sits between GPT-6 Luna and GPT-6.1 Sol in OpenAI's lineup and costs $1 per million input tokens and $5 per million output tokens on the API.

### Is GPT-6.1 Terra good?

**Yes, GPT-6.1 Terra is a strong mid-priced model, especially for computer use and writing.** It scores 51 on Artificial Analysis's Intelligence Index at max effort, four points behind GPT-6.1 Sol. OpenAI says it matches GPT-6 Astra on its Agents' Last Exam computer-use test at medium effort. The catch is that its scores drop fast at lower effort settings.

### Is GPT-6.1 Terra better than Sol?

**No, GPT-6.1 Sol is the more capable model,** leading Terra by four points on Artificial Analysis's Intelligence Index and six on its Coding Agent Index. Terra is the better value for computer use, images, and writing. It costs half as much per token, and OpenAI says it finishes computer-use tasks more than three times faster.

### Is GPT Terra better than Luna?

**Yes, GPT-6.1 Terra is far more capable than GPT-6 Luna,** scoring 51 to Luna's 28 on Artificial Analysis's Intelligence Index at max effort. Terra at medium effort costs about the same per task as Luna at max and scores 10 points higher. Luna is still faster and ten times cheaper per input and output token for simple, repetitive jobs.

### How much does GPT-6.1 Terra cost?

**GPT-6.1 Terra costs $1 per million input tokens, $0.05 per million cached input tokens, and $5 per million output tokens** on the OpenAI API. Batch and Flex cost 50% less, Fast mode costs twice as much, and prompts over 272,000 tokens cost 2x for input and caching and 1.5x for output on the whole request. In ChatGPT, it's included in Plus, Pro, Business, Enterprise, and Edu plans through Work and Codex.

### Is GPT-6.1 Terra in regular ChatGPT?

**No, GPT-6.1 Terra isn't available in regular ChatGPT conversations.** You can use it in ChatGPT Work and Codex on Plus, Pro, Business, Enterprise, and Edu plans, through the API as gpt-6.1-terra, in GitHub Copilot, and on Amazon Bedrock. OpenAI hasn't listed Free or Go plans for it.

## About the editorial team

**Lindy Drope**Founding GTM at Lindy

Lindy leads GTM at Lindy and is the team’s most prolific automation builder. She publishes weekly educational videos and articles on building AI assistants – And yes, she’s a real person!
