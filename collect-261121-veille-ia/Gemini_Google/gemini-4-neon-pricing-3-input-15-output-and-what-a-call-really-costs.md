---
id: collect-261121-veille-ia/gemini-google/gemini-4-neon-pricing-3-input-15-output-and-what-a-call-really-costs
title: "Gemini 4 Neon Pricing: $3 Input, $15 Output and What a Call Really Costs"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["output", "tokens", "neon", "gemini", "input", "standard", "thinking", "costs"]
source: https://dev.to/hassann/gemini-4-neon-pricing-3-input-15-output-and-what-a-call-really-costs-5g09
source_anchor: ""
source_lines: [1, 154]
section: "Gemini_Google"
---
Gemini 4 Neon pricing is $3 per 1M input tokens and $15 per 1M output tokens on the Gemini API's Standard paid tier, flat across its 1M-token context window. Thinking tokens bill at the output rate, cached input costs $0.15 per 1M, Batch and Flex halve every rate, and Priority costs $5.40 and $27. There's no free API tier.

Google released Neon on November 18, 2026, as the middle model of the Gemini 4 family, between the fast Hydrogen and the heavyweight Carbon. Below: the rate card, what real calls cost, how thinking levels move your bill, and how Neon compares with its siblings before and after December 1.

Gemini 4 Neon (model ID `gemini-4-neon`) has four paid tiers on Google's pricing page. All prices are in USD per 1M tokens:

| Tier | Input | Output incl. thinking | Cached input | 
|---|---|---|---|
| Standard | $3.00 | $15.00 | $0.15 | 
| Batch | $1.50 | $7.50 | $0.075 | 
| Flex | $1.50 | $7.50 | $0.075 | 
| Priority | $5.40 | $27.00 | $0.27 | 
| Free tier | Not available | Not available | Not available | 

Three details matter most:

1. 
**Thinking is output.** The output column reads "including thinking tokens", so every token Neon spends reasoning costs $15 per 1M on Standard, even though you never see it in the answer.
2. 
**Caching is implicit only.** You get the $0.15 rate when Google's cache matches a repeated prompt prefix; you can't create a cache object yourself.
3. 
**No free tier.** Google's pricing page lists Neon as not available on the Free tier. Hydrogen is the Gemini 4 model you can call for $0 (see our Gemini 4 Hydrogen free guide).

The four tiers sell the same model at different speed guarantees. Batch returns results asynchronously at half price, Flex is half price for traffic that tolerates slower processing, and Priority costs 80% more than Standard to stay fast when capacity is tight. Pick by deadline:

| Your workload | Tier | Why | 
|---|---|---|
| Interactive coding assistant, chat | Standard | A user is waiting; $3/$15 is the baseline | 
| Nightly code review, eval runs, document backfills | Batch | Nobody waits; you pay $1.50/$7.50 | 
| Background agents that can retry later | Flex | Same half price, without packaging a batch file | 
| Customer-facing agent with an uptime promise | Priority | $5.40/$27 buys you capacity under load | 

Grounding has its own meter. On Standard and Priority, Google Search grounding includes 5,000 free search requests per month, shared across Gemini 3 and newer models, then costs $14 per 1,000. Google Maps grounding follows the same 5,000-free, then $14 per 1,000 pattern. The pricing page lists no grounding price for Batch or Flex.

Neon supports four thinking levels: `low`, `medium`, `high` and `xhigh`. The level doesn't change the price per token; it changes how many output tokens Neon burns before it answers.

Artificial Analysis ran its Intelligence Index at every level and published the output tokens each run used. Multiply by the $15 Standard output rate and you get the output cost of the full run:

| Thinking level | Intelligence Index | Output tokens | Output cost at $15/1M | Time per task | 
|---|---|---|---|---|
| low | 44 | 28.3M | $424.50 | 3.7 min | 
| medium | 55 | 62.9M | $943.50 | 8.3 min | 
| high | 60 | 123M | $1,845 | 14.8 min | 
| xhigh | 61 | 283M | $4,245 | 21.5 min | 

Two things jump out. Going from `high` to `xhigh` buys one index point for 2.3x the output tokens. And `medium` delivers most of `high`'s score for about half the tokens. Artificial Analysis puts Neon's headline cost at $6.44 per index task at `xhigh`.

The practical rule: start your tests at `medium`, move a task to `high` only when you can show it fails at `medium`, and save `xhigh` for the rare problem where one extra point of quality is worth more than double the cost.

Here are three common request shapes, priced on Standard. Output figures include thinking tokens.

**1. A code review call.** 20,000 input tokens (the diff plus context), 3,000 output tokens:

- Input: 20,000 × $3 / 1M = $0.060
- Output: 3,000 × $15 / 1M = $0.045
- 
**Total: $0.105 per call** , or $0.0525 on Batch

Run 1,000 of those a day for 30 days and you spend $3,150 a month on Standard, or $1,575 if the reviews can wait for Batch.

**2. An agent loop with a cached prefix.** Ten turns, each sending the same 50,000-token system prompt and tool list, plus 5,000 new input tokens and 2,000 output tokens:

- With a cache hit on the prefix: 50,000 × $0.15 / 1M + 5,000 × $3 / 1M + 2,000 × $15 / 1M = $0.0525 per turn, **$0.53 for the loop**
- With no cache hit: 55,000 × $3 / 1M + 2,000 × $15 / 1M = $0.195 per turn, **$1.95 for the loop**

That's a 73% difference from caching alone. Because Neon's caching is implicit, you can't force the hit; you raise its odds by keeping the stable part of the prompt at the start and byte-for-byte identical across turns.

**3. A long-document question.** 800,000 input tokens (a large codebase or contract set) and 4,000 output tokens:

- Input: 800,000 × $3 / 1M = $2.40
- Output: 4,000 × $15 / 1M = $0.06
- 
**Total: $2.46** , or $1.23 on Batch

Neon has no long-context surcharge on the pricing page, so token 900,000 costs the same as token 1. The context window is 1,048,576 tokens shared between input and output, and a single synchronous request returns up to 65,536 output tokens by default.

Neon costs exactly twice Hydrogen per token. The Carbon comparison flips on December 1, when Carbon's introductory rate ends:

| Model | Input / 1M | Output / 1M | Code review call (20K in, 3K out) | Free tier | 
|---|---|---|---|---|
| Gemini 4 Hydrogen | $1.50 | $7.50 | $0.0525 | Yes | 
| Gemini 4 Neon | $3.00 | $15.00 | $0.105 | No | 
| Gemini 4 Carbon, through Nov 30 | $2.00 | $10.00 | $0.070 | No | 
| Gemini 4 Carbon, from Dec 1 | $4.00 | $20.00 | $0.140 | No | 

For the next eleven days, Carbon is cheaper per token than Neon. If you build on Carbon's $2/$10 today, budget for $4/$20 in December. Our Gemini 4 Carbon pricing breakdown covers that change in detail.

From December 1, Neon sits in the middle: 25% under Carbon, 2x Hydrogen per token. Per task, the gap depends on thinking tokens, so measure cost per request on your own prompts instead of comparing rate cards.

Route by difficulty: Hydrogen for high-volume everyday calls, Neon for complex coding and analysis you run often, and Carbon for the hardest single problems.

Five levers, in the order they usually pay off:

1. 
**Lower the thinking level.**`medium` uses about half the output tokens of`high` on the Artificial Analysis runs.
2. 
**Send waiting work to Batch or Flex.** Anything nobody reads in real time costs half.
3. 
**Shape prompts for implicit caching.** Keep system prompts and tool definitions at the top and unchanged, so repeated prefixes bill at $0.15 instead of $3.
4. 
**Cap output.** Set a maximum output token count that fits the task.
5. 
**Route easy calls to Hydrogen.** Keep Neon for work that needs it.

Check the Gemini API rate limits page for your tier before you plan throughput.

Every response from Google's Interactions API carries a `usage` object with input, cached, thought and output token counts. That block, times the rate card above, is your real cost per request. Running the same prompts at each thinking level and comparing them is a repetitive job, so hand it to your coding agent.

Apidog CLI gives Claude Code, Codex, Cursor or OpenCode a command-line way to create and run API tests. Set it up once:


```
npm install -g apidog-cli@latest
apidog login --with-token <YOUR_TOKEN>
npx -y skills add https://apidog.com   # pick your agent when prompted
```
Then give your agent one prompt:


```
In Apidog project <id>, create an environment for the Gemini API with
GEMINI_API_KEY and THINKING_LEVEL variables. Add a test case for
POST /v1beta/interactions with model gemini-4-neon and
generation_config.thinking_level set from THINKING_LEVEL. Send our code review
prompt, assert HTTP 200 and non-empty output, and save usage.total_input_tokens,
total_cached_tokens, total_thought_tokens and total_output_tokens. Run it at
low, medium and high, then compute the Standard cost of each run at $3 input,
$0.15 cached and $15 per 1M for thought and output tokens.
```
The agent creates the environment, checks its test case payload against the schema, then runs it once per level:


```
apidog environment create gemini-api --project <id> --base-url https://generativelanguage.googleapis.com
apidog cli-schema get test-case-create
apidog cli-schema validate test-case-create --file case.json
apidog test-case create --project <id> --file case.json
apidog run --test-case <caseId> --project <id> -e <envId> --env-var "THINKING_LEVEL=low" -r cli,json
apidog run --test-case <caseId> --project <id> -e <envId> --env-var "THINKING_LEVEL=medium" -r cli,json
apidog run --test-case <caseId> --project <id> -e <envId> --env-var "THINKING_LEVEL=high" -r cli,json
```
The JSON reports give your agent the token counts to price, so you get a cost-per-level table for your own prompts. Add `apidog run ... -r junit` to CI with the Apidog CLI CI/CD guide to catch a prompt change that doubles your thinking tokens, or start from Apidog CLI in Claude Code. Apidog's Free plan covers up to 4 users.

**Is Gemini 4 Neon free?** Not through the API. Google's pricing page lists Neon as not available on the Free tier, so you need a paid Gemini API project. Hydrogen is the Gemini 4 model with a free tier.

**Are thinking tokens billed separately?** No. They bill as output tokens, at $15 per 1M on Standard. The Interactions API's `usage` object reports them as `total_thought_tokens`, so you can see how much of each bill is reasoning.

**Does Gemini 4 Neon cost more past 200K tokens?** No. The pricing page shows one input rate and one output rate for Neon, with no long-context tier, across the full 1,048,576-token window.

**How much cheaper is Batch?** Exactly half: $1.50 per 1M input and $7.50 per 1M output, with cached input at $0.075. Flex has the same rates.

**Is Neon cheaper than Carbon?** Not until December 1. Carbon's introductory $2/$10 runs through November 30, then rises to $4/$20, which puts Neon's $3/$15 about 25% below Carbon per token.

## Top comments (0)
