---
id: collect-261121-veille-ia/gpt-openai/gpt-6-1-sol-api-tutorial-build-a-node-js-code-reviewer
title: "GPT-6.1 Sol API tutorial: build a Node.js code reviewer"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: ["2026-09-29"]
keywords: ["gpt-6", "tokens", "model", "astra", "input", "code", "million", "output"]
source: https://flaviocopes.com/gpt-6-1-sol/
source_anchor: ""
source_lines: [1, 549]
section: "GPT_OpenAI"
---
Let’s build a Node.js script that reviews code with GPT-6.1 Sol. We’ll call the OpenAI Responses API with `model: 'gpt-6.1-sol'`, choose a reasoning effort and use prompt caching to reduce the cost of repeated reviews.

The API costs $2 per million input tokens and $10 per million output tokens, the same as GPT-6 Sol, and cached input drops to $0.10 per million. You can also pick the model in Codex or ChatGPT Work if you have a paid ChatGPT plan.

That’s a single model call. If you want OpenAI to run the whole agent loop for your app, with tools and a sandbox, that’s the Agents API.

OpenAI released it on September 29, 2026. It’s the new middle model of the GPT-6 family, and OpenAI says it nearly matches GPT-6 Astra, its top model, on agentic coding, computer use and professional work, at a fifth of Astra’s price. In Artificial Analysis’s tests it also scores higher than GPT-5.6 Sol, for about a third of the cost per task.

After the prices and the Codex setup, we’ll build a small code review script in Node.js that talks to GPT-6.1 Sol through the API.

## The quick answer

| What | How | 
|---|---|
| Model ID | `gpt-6.1-sol` | 
| ChatGPT plans | Plus, Pro, Business, Enterprise, Edu (Codex and ChatGPT Work, not Chat) | 
| Codex CLI | `codex --model gpt-6.1-sol` , or`/model` inside a session | 
| API price | $2 input, $0.10 cached input, $10 output, per million tokens | 
| Reasoning effort | `low` ,`medium` (default),`high` ,`xhigh` ,`max` | 
| Tool calling | Responses API only | 
| Context window | 1,050,000 tokens, up to 128,000 output tokens | 

For the newer models, see how to use GPT-6.2 Astra and Sol.

## What is GPT-6.1 Sol?

The GPT-6 family has three sizes. Astra is the most capable and the most expensive. Luna is the most efficient and the cheapest. Sol sits in the middle, and GPT-6.1 Sol is an upgrade to GPT-6 Sol.

Input and output prices didn’t change. The model got better, and much closer to Astra. A few numbers from OpenAI’s announcement:

- On DeepSWE v1.1, a coding benchmark, it matches Astra at about a fifth of the cost.
- On OSWorld 2.0, which tests a model using a desktop computer, it lands within 2.1 points of Astra at about a seventh of the cost.
- At low reasoning effort, the share of answers with a factual error dropped from 11.4% to 7.7% compared to GPT-6 Sol.

Astra still leads on the hardest problems. On Terminal-Bench Science, GPT-6.1 Sol more than doubled GPT-6 Sol’s score, but Astra keeps the top spot at 68.1%, and OpenAI still recommends Astra for the most difficult scientific work.

Keep in mind that the vendor picks the benchmarks it publishes. I explained how to read these charts in How AI models are measured and compared. The test that tells you the most is one on your own code, like the one I ran in my hands-on comparison of coding models.

A few other changes matter when you write code against it:

- Cached input costs half as much as on GPT-6 Sol: $0.10 per million tokens instead of $0.20. Agents send the same long prompt on every step, so this adds up fast. We’ll use it in the last step of the tutorial.
- The `none` and`minimal` reasoning efforts are gone. The lowest level is`low` .
- The knowledge cutoff moved to April 30, 2026.
- It reads text and images and writes text. The context window is 1,050,000 tokens, with up to 922,000 input tokens and 128,000 output tokens.

If tokens and context windows are new to you, start with the tokens and context window sections of my AI fundamentals guide.

## How much does GPT-6.1 Sol cost?

These are the API prices per million tokens, checked on September 30, 2026:

| Model | Input | Cached input | Output | 
|---|---|---|---|
| GPT-6 Astra | $10 | $1 | $50 | 
| GPT-6.1 Sol | $2 | $0.10 | $10 | 
| GPT-6 Sol | $2 | $0.20 | $10 | 
| GPT-6 Luna | $0.10 | $0.01 | $0.50 | 
| GPT-5.6 Sol | $4 | $0.40 | $20 | 

If you’re still on GPT-5.6 Sol, every token costs half as much on GPT-6.1 Sol.

The fine print:

- Writing tokens into the cache costs $2.50 per million, a bit more than normal input. Reading them back costs $0.10.
- A request with more than 272,000 input tokens costs 2x for input and cache and 1.5x for output, for the whole request.
- Batch and Flex processing cost half. Fast mode costs double.

In ChatGPT you don’t pay per token. Codex and ChatGPT Work draw from your plan’s usage limits instead. If you’re not sure which of the two fits you, I compared them in subscription or API. There’s a longer API pricing vs subscription walkthrough, where I priced a month of my own Codex usage at API rates.

## How efficient is GPT-6.1 Sol?

The price per token doesn’t tell you what a task costs. That also depends on how many tokens the model needs to finish it, and on how much of its input comes from the cache.

Artificial Analysis runs the same evaluations on every model and records what each run costs. At max effort, a task on its Intelligence Index costs $0.72 with GPT-6.1 Sol, $1.05 with GPT-6 Sol, $1.99 with GPT-5.6 Sol and $3.26 with GPT-6 Astra.

Compared to GPT-5.6 Sol, that’s 64% less per task, and GPT-6.1 Sol scores 5 points higher on the index. Astra scores 1 point more than GPT-6.1 Sol, for four and a half times the cost.

On the Artificial Analysis Coding Agent Index, GPT-6.1 Sol at `xhigh` effort scores 1 point above Astra, for less than 15% of Astra’s cost per task. It scored 3 points lower at `max` than at `xhigh`, so on coding tasks try `xhigh` first.

In OpenAI’s announcement, a Terminal-Bench Science task at max effort costs $5.47 on average with GPT-6.1 Sol, $23.21 with Opus 5.5 and $23.80 with Astra.

Efficient doesn’t mean it writes less, though. Artificial Analysis counted 10% to 30% more output tokens than GPT-6 Sol, depending on the effort, and GPT-6.1 Sol still costs less per task.

For the Plus plan, OpenAI’s pricing page estimates 15 to 160 messages every five hours with GPT-6.1 Sol when Codex runs on your computer, and 5 to 45 with Astra. If you run out and buy credits, GPT-6.1 Sol costs half as many credits per token as GPT-5.6 Sol, and a quarter as many for cached input.

## Where can you use GPT-6.1 Sol?

In ChatGPT, GPT-6.1 Sol is available in Codex (the desktop app and the CLI) and in ChatGPT Work on the web and on mobile. It’s not in regular Chat.

The launch rollout covers the Plus, Pro, Business, Enterprise and Edu plans. Free and Go are not included. On Enterprise and Edu the model stays off until an admin turns it on.

In the API the model ID is `gpt-6.1-sol`. The free API tier isn’t supported, so you need a paid tier. Tier 1 starts at 500 requests per minute.

A rollout reaches accounts over time. Mine didn’t have it the day after the launch: GPT-6.1 Sol wasn’t in my Codex model list, and forcing it from the command line failed:

`codex exec -m gpt-6.1-sol "Say hi in one word"````
warning: Model metadata for `gpt-6.1-sol` not found. Defaulting to fallback metadata; this can degrade performance and cause issues.
ERROR: {"type":"error","status":400,"error":{"type":"invalid_request_error","message":"The 'gpt-6.1-sol' model is not supported when using Codex with a ChatGPT account."}}
```
I recorded this rollout error in my coding-model comparison too. If you see “The ‘gpt-6.1-sol’ model is not supported when using Codex with a ChatGPT account”, that comparison explains which models were available for my test. Wait until GPT-6.1 Sol shows up in your model picker before selecting it in Codex.

## How do I use GPT-6.1 Sol in Codex?

In the ChatGPT desktop app, the model and reasoning control sits below the composer. It starts on a Power setting: move it toward **Smarter** for deeper reasoning, or toward **Faster** for quicker and cheaper work. Open **Advanced** to pick GPT-6.1 Sol, a reasoning effort and a speed yourself.

In the CLI, pass the model when you start Codex:

`codex --model gpt-6.1-sol`
Inside a session, `/model` switches the model and the reasoning effort. Max and Ultra are under “More reasoning…” in that menu.

The same flag works for one-off tasks with `codex exec`:

`codex exec -m gpt-6.1-sol "Review the current changes"`
To make it your default, set it in `~/.codex/config.toml`:

```
model = "gpt-6.1-sol"
model_reasoning_effort = "medium"
```
`model_reasoning_effort` accepts `low`, `medium`, `high`, `xhigh`, `max` and `ultra`. The app shows the same scale with friendlier names, from Light to Ultra. Ultra goes beyond a single agent: it splits the work across subagents, which helps on big tasks that divide well.

You can raise the effort for one session without touching the config file:

`codex -m gpt-6.1-sol -c model_reasoning_effort=high`
Which effort should you pick? OpenAI suggests GPT-6.1 Sol at Medium for complex technical work you expect to revise, and at Extra high for polished deliverables and decisions built from conflicting evidence. Higher effort takes longer and uses more of your limits, so the docs recommend starting from the default and going up when a task needs deeper planning.

Fast mode works with GPT-6.1 Sol too. Type `/fast` in the CLI to toggle it, or keep it on in `config.toml`:

```
service_tier = "fast"
[features]
fast_mode = true
```
Fast mode uses your included limits at 2.5x the Standard rate. Ultrafast is a separate tier, and the speed page lists it for GPT-6.1 Sol and GPT-6 Astra. GPT-6 Astra Ultrafast generates tokens up to 8x faster than GPT-6 Astra Standard in Codex. Included Ultrafast usage counts at 8x the Standard rate. Fast mode puts those Codex rates next to Claude Code and Cursor.

If your config still points at `gpt-5.5`, change it now: GPT-5.5 retires from ChatGPT and Codex on October 14, 2026. The API isn’t affected.

Everything else about Codex, from approvals to skills and MCP servers, is in The complete guide to Codex.

## How do I use GPT-6.1 Sol with the OpenAI API?

Let’s use the API to build a script that reviews the changes you haven’t committed yet. It reads `git diff`, sends it to GPT-6.1 Sol and prints a list of problems.

We’ll start with the simplest possible call, then add one feature per step: reasoning effort, structured output, a tool, and prompt caching.

You need Node.js, Git and an OpenAI API key on a paid tier.

### Step 1: create the project

Create a folder, turn it into a Git repository and install the two packages we need, the official `openai` SDK and `zod`:

```
mkdir review-demo
cd review-demo
git init
npm init -y
npm pkg set type=module
npm install openai zod
```
`type=module` lets us use `import` and top-level `await` in plain `.js` files.

Put your API key in a `.env` file:

`OPENAI_API_KEY=sk-proj-...`
And create a `.gitignore`, so the key never ends up in a commit:

```
node_modules
.env
```
Now we need some code to review. Create `slugify.js`, a function that turns a blog post title into a URL slug:

```
export function slugify(title) {
  return title
    .toLowerCase()
    .replace(/[^a-z0-9]+/g, '-')
    .replace(/^-|-$/g, '')
}
```
Commit it:

```
git add .
git commit -m "Add slugify"
```
Our next change adds support for accented letters, so “Perché” becomes “perche” instead of “perch”. Edit `slugify.js`:

```
export function slugify(title) {
  return title
    .normalize('NFD')
    .replace(/[\u0300-\u036f]/g, '')
    .toLowerCase()
    .replace(/[^a-z0-9]+/, '-')
    .replace(/^-|-$/g, '')
}
```
`normalize('NFD')` splits “é” into “e” plus a separate accent mark, and the next line removes the accent marks.

The change also has a bug. The `g` flag disappeared from the third `replace()`, so only the first run of spaces or symbols becomes a dash. Try it:

`node -e "import('./slugify.js').then(m => console.log(m.slugify('Perché usare Astro?')))"``perche-usare astro?`
Leave this change uncommitted, because our script reviews what `git diff` shows.

### Step 2: send the diff to GPT-6.1 Sol

Create `review.js`:

```
import OpenAI from 'openai'
import { execSync } from 'node:child_process'
const client = new OpenAI()
const diff = execSync('git diff', { encoding: 'utf8' })
if (!diff) {
  console.log('No changes to review')
  process.exit()
}
const response = await client.responses.create({
  model: 'gpt-6.1-sol',
  instructions: 'You review code changes. List bugs and risky changes, one per line.',
  input: diff,
})
console.log(response.output_text)
```
`new OpenAI()` reads the key from the `OPENAI_API_KEY` environment variable. `execSync('git diff')` returns your uncommitted changes as text.

In the request, `instructions` tells the model its job, and `input` carries the content to work on. `response.output_text` joins all the text the model wrote into one string.

Run it:

`node --env-file=.env review.js`
The `--env-file` flag loads `.env` into `process.env` before the script starts. If you forget it, the SDK stops right away:

``OpenAIError: Missing credentials. Please pass an `apiKey`, `workloadIdentity`, `adminAPIKey`, or set the `OPENAI_API_KEY` or `OPENAI_ADMIN_KEY` environment variable.``
We didn’t set a reasoning effort, so the model used `medium`, the default.

### Step 3: choose a reasoning effort

GPT-6.1 Sol always thinks before it answers. The `reasoning.effort` parameter sets how much. It accepts `low`, `medium`, `high`, `xhigh` and `max`.

A missing `g` flag is the kind of detail a quick read skips, so for a review we want the model to think harder. Add one line to the request:

```
const response = await client.responses.create({
  model: 'gpt-6.1-sol',
  reasoning: { effort: 'high' },
  instructions: 'You review code changes. List bugs and risky changes, one per line.',
  input: diff,
})
```
You don’t see the reasoning, but you pay for it: reasoning tokens are billed as output tokens, at $10 per million. You can check how many the model used:

`console.log(response.usage.output_tokens_details.reasoning_tokens)`
If you’re moving older code over, look for `effort: 'none'` or `effort: 'minimal'`. GPT-6 Sol accepts `none`, but GPT-6.1 Sol accepts neither value, and the request fails. Use `low` instead.

### Step 4: get structured output

Text is fine to read, but our script can’t do much with it. We want each finding as data: the file, the line, a severity and a message. Then we could fail a Git hook on bugs, or post the findings as comments. For a small routing or classification question, the Decisions API is faster than a full structured Responses call.

Structured output makes the model answer with JSON that matches a schema we define. We write the schema with Zod, and the SDK converts it to JSON Schema for the request and parses the answer back into an object.

Replace `review.js` with this version:

```
import OpenAI from 'openai'
import { zodTextFormat } from 'openai/helpers/zod'
import { z } from 'zod'
import { execSync } from 'node:child_process'
const Review = z.object({
  findings: z.array(
    z.object({
      file: z.string(),
      line: z.number(),
      severity: z.enum(['bug', 'risk', 'nitpick']),
      message: z.string(),
    }),
  ),
})
const client = new OpenAI()
const diff = execSync('git diff', { encoding: 'utf8' })
if (!diff) {
  console.log('No changes to review')
  process.exit()
}
const response = await client.responses.parse({
  model: 'gpt-6.1-sol',
  reasoning: { effort: 'high' },
  instructions: 'You review code changes. Report bugs and risky changes. Use line numbers from the new version of the file.',
  input: diff,
  text: { format: zodTextFormat(Review, 'review') },
})
for (const finding of response.output_parsed.findings) {
  console.log(`${finding.severity} ${finding.file}:${finding.line} ${finding.message}`)
}
```
Three things changed. We call `responses.parse()` instead of `responses.create()`. The `text.format` option passes our schema, and `'review'` is just a name for it. And we read `response.output_parsed`, an object that already matches the `Review` type.

`z.enum()` limits the severity to three values, so the rest of your code never has to handle “Critical!!” or “minor-ish”.

We also told the model to use line numbers from the new version of the file. The hunk headers in a diff carry line numbers for both the old and the new file, and without that sentence you could get a mix of the two.

Each finding now prints on one line: the severity, `slugify.js` with a line number, and the message.

### Step 5: let the model read files

A diff only shows a few lines around each change. Sometimes that’s not enough to judge it: the model might want to see where a function is used, or what a helper does.

We can give the model a tool. A tool is a function in our script that we describe to the model. When the model wants to use it, it doesn’t answer yet. It sends back a `function_call` with the arguments it wants. We run the function, send the result back, and ask again. We repeat until the model stops asking for tools and gives us the review.

With GPT-6.1 Sol, tool calling needs the Responses API, which is the API we’re already using. The older Chat Completions API only works without tools on this model. If you want the bigger picture on tools and function calling, I compare MCP, plain APIs and function calling in what is MCP.

Here’s the complete `review.js` with a `read_file` tool:

```
import OpenAI from 'openai'
import { zodTextFormat } from 'openai/helpers/zod'
import { z } from 'zod'
import { execSync } from 'node:child_process'
import { readFileSync } from 'node:fs'
import path from 'node:path'
const Review = z.object({
  findings: z.array(
    z.object({
      file: z.string(),
      line: z.number(),
      severity: z.enum(['bug', 'risk', 'nitpick']),
      message: z.string(),
    }),
  ),
})
const tools = [
  {
    type: 'function',
    name: 'read_file',
    description: 'Read a file from the repository to see the code around a change.',
    parameters: {
      type: 'object',
      properties: {
        path: { type: 'string', description: 'Path from the repository root, like slugify.js' },
      },
      required: ['path'],
      additionalProperties: false,
    },
    strict: true,
  },
]
function readFile(filePath) {
  const fullPath = path.resolve(filePath)
  if (!fullPath.startsWith(process.cwd() + path.sep) || path.basename(fullPath).startsWith('.env')) {
    return 'Error: you can only read project files'
  }
  try {
    return readFileSync(fullPath, 'utf8')
  } catch {
    return `Error: ${filePath} does not exist`
  }
}
const client = new OpenAI()
const diff = execSync('git diff', { encoding: 'utf8' })
if (!diff) {
  console.log('No changes to review')
  process.exit()
}
const options = {
  model: 'gpt-6.1-sol',
  reasoning: { effort: 'high' },
  instructions: 'You review code changes. Report bugs and risky changes. Use line numbers from the new version of the file. Read a file only when the diff is not enough.',
  tools,
  text: { format: zodTextFormat(Review, 'review') },
}
let response = await client.responses.parse({
  ...options,
  input: diff,
})
while (true) {
  const calls = response.output.filter((item) => item.type === 'function_call')
  if (calls.length === 0) break
  response = await client.responses.parse({
    ...options,
    previous_response_id: response.id,
    input: calls.map((call) => ({
      type: 'function_call_output',
      call_id: call.call_id,
      output: readFile(JSON.parse(call.arguments).path),
    })),
  })
}
for (const finding of response.output_parsed.findings) {
  console.log(`${finding.severity} ${finding.file}:${finding.line} ${finding.message}`)
}
```
Let’s go through the new parts.

The `tools` array describes `read_file` with a JSON Schema: one required string called `path`. `strict: true` makes the model stick to that schema, so `arguments` always parses and always has a `path`.

`readFile()` is the function that actually runs. The model decides which path to ask for, so we check it before reading anything. `path.resolve()` turns the path into an absolute one, and we refuse anything outside the project folder and any `.env` file. Without that check, the model could read any file your user can read, SSH keys included. When something goes wrong, we return the error as text instead of throwing, so the model reads it and can try another file.

The `while` loop is the tool loop. After each response we look for `function_call` items in `response.output`. There can be more than one, because the model can ask for several files at once. For each call we send back a `function_call_output` with the same `call_id`, so the model knows which result belongs to which request. When a response has no calls left, it contains the final review.

`previous_response_id` tells the API to continue the previous response. The API keeps the conversation on its side, so we only send the new items. It doesn’t carry over `instructions`, though, so we send the same settings every time. That’s why they live in one `options` object that both calls spread.

### Step 6: cache the part that doesn’t change

When a request starts with the same tokens as a recent one, OpenAI can reuse the work it already did on that prefix. Those cached tokens cost $0.10 per million on GPT-6.1 Sol instead of $2.

Let’s add the project’s rules to the review, so the model checks the change against them. Many projects keep those rules in an AGENTS.md file. Create one:

```
# Rules for this project
- Plain JavaScript with ES modules, no TypeScript.
- Every exported function has a test in a `.test.js` file next to it.
- Slugs only contain lowercase letters, digits and dashes.
```
The rules stay the same on every run, and the diff changes every time. So we put the rules first and the diff after them. The order alone doesn’t do it, though.

By default the API caches implicitly: it puts the cache boundary at the end of the latest message. In our request that’s the diff. On the next run the diff is different, so the cached prefix never matches, and the rules are never reused. Every run also pays to write its diff into the cache, at $2.50 per million tokens.

The fix is to place the boundary ourselves. We set `prompt_cache_options` to explicit mode, and we add a `prompt_cache_breakpoint` right after the rules. Here’s the finished `review.js`:

```
import OpenAI from 'openai'
import { zodTextFormat } from 'openai/helpers/zod'
import { z } from 'zod'
import { execSync } from 'node:child_process'
import { readFileSync } from 'node:fs'
import path from 'node:path'
const Review = z.object({
  findings: z.array(
    z.object({
      file: z.string(),
      line: z.number(),
      severity: z.enum(['bug', 'risk', 'nitpick']),
      message: z.string(),
    }),
  ),
})
const tools = [
  {
    type: 'function',
    name: 'read_file',
    description: 'Read a file from the repository to see the code around a change.',
    parameters: {
      type: 'object',
      properties: {
        path: { type: 'string', description: 'Path from the repository root, like slugify.js' },
      },
      required: ['path'],
      additionalProperties: false,
    },
    strict: true,
  },
]
function readFile(filePath) {
  const fullPath = path.resolve(filePath)
  if (!fullPath.startsWith(process.cwd() + path.sep) || path.basename(fullPath).startsWith('.env')) {
    return 'Error: you can only read project files'
  }
  try {
    return readFileSync(fullPath, 'utf8')
  } catch {
    return `Error: ${filePath} does not exist`
  }
}
const client = new OpenAI()
const diff = execSync('git diff', { encoding: 'utf8' })
if (!diff) {
  console.log('No changes to review')
  process.exit()
}
const rules = readFileSync('AGENTS.md', 'utf8')
const options = {
  model: 'gpt-6.1-sol',
  reasoning: { effort: 'high' },
  instructions: 'You review code changes. Report bugs and risky changes. Use line numbers from the new version of the file. Read a file only when the diff is not enough.',
  tools,
  text: { format: zodTextFormat(Review, 'review') },
  prompt_cache_options: { mode: 'explicit' },
}
let response = await client.responses.parse({
  ...options,
  input: [
    {
      role: 'developer',
      content: [
        {
          type: 'input_text',
          text: `The project rules:\n\n${rules}`,
          prompt_cache_breakpoint: { mode: 'explicit' },
        },
      ],
    },
    { role: 'user', content: diff },
  ],
})
while (true) {
  const calls = response.output.filter((item) => item.type === 'function_call')
  if (calls.length === 0) break
  response = await client.responses.parse({
    ...options,
    previous_response_id: response.id,
    input: calls.map((call) => ({
      type: 'function_call_output',
      call_id: call.call_id,
      output: readFile(JSON.parse(call.arguments).path),
    })),
  })
}
for (const finding of response.output_parsed.findings) {
  console.log(`${finding.severity} ${finding.file}:${finding.line} ${finding.message}`)
}
const { input_tokens, input_tokens_details } = response.usage
console.log(`input: ${input_tokens}, cached: ${input_tokens_details.cached_tokens}, written: ${input_tokens_details.cache_write_tokens}`)
```
The input is now a list of two messages. First comes a `developer` message with the rules, and its text block carries the breakpoint. Then comes the `user` message with the diff, which sits after the breakpoint and never gets cached. The rules have to go in a message, because the top-level `instructions` field can’t hold a breakpoint.

The last line prints what happened with the cache. `cached_tokens` is how many input tokens were read from the cache, and `cache_write_tokens` is how many were written to it on this run.

Run the script twice and you’ll probably see zeros both times. That’s expected. A prefix needs at least 1,024 tokens to be cached, and ours is a few hundred: the instructions, the tool, the schema and a three-line AGENTS.md. In a real project with a long AGENTS.md, the first run writes the prefix and the runs in the next 30 minutes read it. Every run that reuses it resets the 30 minutes.

One more thing can break the cache: changing `reasoning.effort` between requests changes the prefix. If your app needs more effort for one turn and less for the next, OpenAI’s docs describe a `configuration_update` input item that changes the effort mid-conversation and keeps the cache.

For one review the savings are tiny. They matter for agents, which resend the whole prefix on every step. Take an agent with a 100,000-token prefix that runs 30 steps. That’s 3 million prefix tokens, which cost $6 at the normal input price and $0.30 from the cache on GPT-6.1 Sol. On GPT-6 Sol the cached version would cost $0.60.

## How do I migrate from GPT-6 Sol or GPT-5.6 Sol?

Changing the model ID is most of the work, but check these first:

- Replace `none` and`minimal` reasoning efforts with`low` , and compare the results on a few real tasks.
- Move tool calling to the Responses API. GPT-6.1 Sol still answers on Chat Completions, but only without tools.
- Remove `temperature` ,`top_p` and`top_logprobs` . The API only accepts them when the effort is`none` , which GPT-6.1 Sol doesn’t have.
- If you’re coming from GPT-5.5 or earlier, replace `prompt_cache_retention` with`prompt_cache_options: { ttl: '30m' }` .

If you use Codex, OpenAI’s docs skill can make these changes for you. Run this in your project:

`$openai-docs migrate this project to the GPT-6 model family`
## When should you pick GPT-6 Astra or GPT-6 Luna instead?

GPT-6.1 Sol is meant for complex work where cost matters: agentic coding, computer use, and bigger deliverables like a website built from a product brief.

GPT-6 Luna costs a twentieth of Sol’s price. OpenAI suggests it for focused, repeatable tasks: small edits, well-scoped problems, simple data extraction. If a job runs thousands of times a day and works on Luna, keep it on Luna.

For fast everyday work, visual reasoning, or screenshots through the Responses API, GPT-6.1 Terra sits between Luna and Sol on price and speed.

GPT-6 Astra costs five times as much as Sol. It’s the pick when the result matters more than the bill, like the hardest scientific problems or demanding analysis with strict requirements.

OpenAI’s own advice is to run GPT-6.1 Sol and Astra on the same task and compare the results. That’s also how I’d decide. In my coding model comparison I picked GPT-5.6 Sol for coding, and GPT-6.1 Sol costs half as much per token and about a third as much per task, so it’s the first model I’ll run through the same test once it shows up in my account.

Want me to talk about your product? You can sponsor this site.
