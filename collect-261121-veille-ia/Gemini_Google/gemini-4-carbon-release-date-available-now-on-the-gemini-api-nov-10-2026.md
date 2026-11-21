---
id: collect-261121-veille-ia/gemini-google/gemini-4-carbon-release-date-available-now-on-the-gemini-api-nov-10-2026
title: "Gemini 4 Carbon Release Date: Available Now on the Gemini API (Nov 10, 2026)"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-11-11"]
keywords: ["gemini", "google", "carbon", "november", "available", "post", "argon", "release"]
source: https://apidog.com/blog/gemini-4-carbon-release-date/
source_anchor: ""
source_lines: [1, 156]
section: "Gemini_Google"
---
As of November 11, 2026, Google’s Gemini 4 Carbon is available now: Google made it generally available on November 10, 2026 as `gemini-4-carbon` on the Gemini API paid tier, in Google AI Studio and in Android Studio, 13 days after announcing it on October 28 for a subset of cyber defenders. It costs $2 per million input tokens and $10 per million output tokens through November 30, then $4 and $20.

Below: the full timeline next to Argon’s, who gets Carbon where, what Google published for the API at GA, and how to test `gemini-4-carbon` against `gemini-4-argon` on your own prompts.

## What is Gemini 4 Carbon?

Gemini 4 Carbon is Google DeepMind’s newest frontier model, announced October 28, 2026 by Koray Kavukcuoglu as “our strongest model with state-of-the-art cybersecurity and long horizon capabilities.” It follows Gemini 4 Argon, which launched two weeks earlier, ships at the same $2 input / $10 output introductory price per million tokens, and went GA on November 10.

The full benchmark table and price math live in What is Gemini 4 Carbon?. This post sticks to access and timing; your first request is in the Gemini 4 Carbon API guide.

## Gemini 4 Carbon release date: what Google actually said

Every access and timing fact Google has published, with its source:

| Item | What’s known (Nov 11, 2026) | Source | 
|---|---|---|
| Announced | October 28, 2026 | Google launch post | 
| Generally available | November 10, 2026, 13 days after the announcement | Google GA post | 
| Developers | Gemini API paid tier, Google AI Studio, Android Studio | Google GA post | 
| Businesses and consumers | Gemini for business, Google Antigravity, CodeMender; Google AI Ultra in the Gemini app and AI Mode in Search | Google GA post | 
| Price at launch | $2 per 1M input, $10 per 1M output, cached input 95% off | Google launch post | 
| Price after intro | $4 per 1M input, $20 per 1M output from December 1, 2026; the intro period ends November 30 | Google GA post, Gemini API pricing page | 
| Model ID | `gemini-4-carbon` | Gemini API models page | 
| API release | Generally available November 10, 2026 | Gemini API changelog | 

Google’s launch post promised a staged rollout: “We’re continuing to gather feedback and iterate on guardrails before making Carbon available to developers, businesses, and consumers via our product surfaces as soon as possible.”

“As soon as possible” turned out to be 13 days. Google’s GA post opens: “Today, we’re making Gemini 4 Carbon available to developers, enterprises, scientists, and consumers.”

## Carbon matched the Argon precedent: 13 days from Fairwind to GA

The best evidence for Carbon’s timing was what Google did with Argon a month earlier: same program, same price, same staged language. Carbon followed it to the day.

1. **September 30, 2026:** Google announced Gemini 4 Argon and rolled it out only to a set of Fairwind Program partners. No model ID, no public API.
2. **October 13, 2026:** The Gemini API changelog lists`gemini-4-argon` as generally available. Paid API customers got it 13 days after the announcement.
3. **Through November 30, 2026:** Argon bills at $2 / $10 per million tokens on the paid tier, per the Gemini API pricing page. From December 1 it’s $4 / $20.

At Argon’s pace, a public API was due around mid-November, with the US government’s voluntary pre-release testing process and Google’s guardrail work as the two risks. Neither stretched it: Carbon reached the paid Gemini API on November 10, day 13. Our Gemini 4 Argon release date post shows how that rollout played out in detail.

## Fairwind Program: who qualifies and how to apply

The Fairwind Program is Google DeepMind’s early-access track for cyber defenders. Before GA it was the only route to Carbon; since GA, Google says defenders can apply there for Carbon for cyber on a rolling basis. One caveat: as of November 11, the Fairwind Program page still describes exclusive access to Gemini 4 Argon with cybersecurity defense capabilities and doesn’t mention Carbon.

**Who Google prioritizes**, per the Fairwind FAQ:

- Governments and national cyber authorities
- Critical infrastructure operators: healthcare, telecommunications, energy and financial networks
- Core technology platforms that secure widely used software
- Academic labs focused on defensive benchmarking (students are pointed to CodeMender on Google Cloud instead)

Google says it works with over 650 partners globally and vets every applicant’s security history and record of ethical operations.

**How to apply:**

1. Contact your Google Cloud account team: the Fairwind FAQ names that as the way to request access.
2. Not a Google Cloud customer yet? The FAQ points you to Google Cloud’s getting-started docs first.
3. Wait. Google says it will “review and respond to eligible partners who meet our criteria as soon as we can.” No processing time is published.

**Terms you’d sign up for** (from the Fairwind page, written for Argon):

- Only defensive dual-use tasks: authorized threat simulation, reverse engineering and malware analysis for defensive and academic research. Creating malware isn’t allowed.
- User-level authentication, phishing-resistant MFA and access controls.
- Access only for internal cybersecurity, incident response or penetration testing teams, with employee access tracked.
- No sharing, reselling or redistributing access.

Building a coding tool at a startup? You almost certainly don’t qualify. Your route is the paid Gemini API, open since November 10.

## Gemini 4 Carbon API: what Google published at GA

Google now serves Carbon from its own endpoint, so start there. Third-party “Gemini 4 Carbon API” pages that went up before November 10 were placeholders. Here is what Google’s docs list:

| Item | Published by Google (Nov 11, 2026) | 
|---|---|
| Model ID | `gemini-4-carbon` | 
| Context window | 1,048,576 tokens, shared by input, thinking and output | 
| Max output tokens | 65,536 per standard response; more with background execution | 
| Where | Gemini API paid tier, Google AI Studio, Android Studio | 
| Gemini app plan | Google AI Ultra | 
| Free tier | Not available | 
| Thinking levels | `low` ,`medium` (default),`high` | 

Carbon follows Argon’s GA rules, per Google’s Carbon developer docs: explicit caching isn’t supported (implicit caching only), `temperature`, `top_p` and `top_k` are ignored, and HTTP timeouts should be at least 15 minutes unless you use background execution. Moving from Argon is a model ID change. Our Gemini 4 Argon API guide covers what those changes meant for Argon.

## Gemini Carbon vs Argon: what you can use today

Since November 10, price and access are a tie. Google’s numbers, three rows:

|  | Gemini 4 Argon | Gemini 4 Carbon | 
|---|---|---|
| Who can use it | Paid Gemini API, GA since Oct 13 | Paid Gemini API, GA since Nov 10 | 
| Price (intro) | $2 / $10 per 1M through Nov 30 | $2 / $10 per 1M through Nov 30 | 
| Model ID | `gemini-4-argon` | `gemini-4-carbon` | 

The difference is the job: Google says Argon “remains the ideal choice for the majority of software engineering and knowledge work,” while Carbon spends more tokens on harder tasks. The full side-by-side is in Gemini 4 Carbon vs Argon, and if you’re hunting for a $0 route, read Is Gemini 4 Carbon free?.

## Gemini 4 Carbon benchmark claims and why they matter for timing

The cyber numbers explained the gate. Google reports 88.2% on SEC-bench Pro, a state-of-the-art 54.8% on ExploitGym (exploitation) and 71.0% on Collinear’s CWE-bench (patching). A model that’s better at exploitation needed more guardrail work before it went public. At GA, Google added red-team results: an adaptive attack agent breached Carbon on 0.8% of attempts, down from 3.1% on Argon. The Gemini 4 Argon benchmarks post has Argon’s baseline for comparison.

Early testers quoted by TestingCatalog reportedly found Carbon better than Argon at 3D work, weaker at frontend UI, and slower but “less lazy.” Treat that as reported, not measured.

## Send your first request to gemini-4-carbon

Keep the model name in a variable, so the same request runs on `gemini-4-carbon` and `gemini-4-argon`. This request follows Google’s documented Interactions API format. We haven’t run it for this post; it needs a paid Gemini API key, since neither model is on the free tier.

```
export GEMINI_MODEL="gemini-4-carbon"   # or gemini-4-argon for everyday work
curl -s "https://generativelanguage.googleapis.com/v1beta/interactions" \
  -H "x-goog-api-key: $GEMINI_API_KEY" \
  -H "Content-Type: application/json" \
  -X POST \
  -d "{\"model\": \"${GEMINI_MODEL}\", \"input\": \"Review this diff for injection bugs.\", \"generation_config\": {\"thinking_level\": \"medium\"}}"
```
Read the model from config, log token usage per request (Carbon spends more thinking tokens per turn than Argon, so the same task can cost more at the same price), and raise client timeouts to at least 15 minutes, as Google’s Carbon docs recommend.

## Let your coding agent test Carbon against Argon with Apidog CLI

Don’t guess which model your workload needs. Hand the check to your coding agent. Apidog CLI lets Claude Code, Codex, Cursor or OpenCode create and run API tests from the terminal, so the agent can build one test that hits the Gemini models list endpoint to confirm your key can see Carbon, and one that runs your real prompt on both models.

Set it up once:

```
npm install -g apidog-cli@latest
apidog login --with-token <YOUR_TOKEN>
npx -y skills add https://apidog.com   # pick your agent when prompted
```
Then give your agent one prompt:

```
In Apidog project <id>, create a Gemini API environment with GEMINI_MODEL=gemini-4-carbon.
Add two test cases: (1) GET /v1beta/models that asserts a model name contains
"gemini-4-carbon"; (2) POST /v1beta/interactions using {{GEMINI_MODEL}} that
asserts HTTP 200 and non-empty output text. Run both and summarize the results.
```
The agent checks each payload against its schema before writing anything, then runs the cases:

```
apidog environment create gemini-api --project <id> --base-url https://generativelanguage.googleapis.com
apidog cli-schema get test-case-create
apidog cli-schema validate test-case-create --file models-watch.json
apidog test-case create --project <id> --file models-watch.json
apidog run --test-case <caseId> --project <id> -e <envId> -r cli,json
```
When the models case passes, your key can reach Carbon. Rerun your prompt case with `--env-var "GEMINI_MODEL=gemini-4-argon"` and compare the output and token counts against Carbon. To rerun the comparison on a schedule, have the agent read `apidog cli-schema get scheduled-task-create` and create a scheduled task, or put the same `apidog run ... -r junit` in CI with the Apidog CLI CI/CD guide. New to the CLI? Start with Apidog CLI in Claude Code. Apidog’s Free plan covers up to 4 users.

## FAQ

**What is the Gemini 4 Carbon release date?** November 10, 2026. Google announced it October 28 for a subset of cyber defenders and made it generally available 13 days later as `gemini-4-carbon`.

**Can I use the Gemini 4 Carbon API today?** Yes, with a paid key. Since November 10, 2026, `gemini-4-carbon` is on the Gemini API paid tier, Google AI Studio and Android Studio. It isn’t on the free tier.

**How long did Argon take to go public?** 13 days: announced September 30, 2026 for Fairwind partners, GA on the paid Gemini API October 13. Carbon matched it: October 28 to November 10.

**How much does Gemini 4 Carbon cost?** $2 per million input tokens and $10 per million output tokens through November 30, 2026, with cached input 95% off. From December 1 it’s $4 and $20.

**Should I use Carbon or Argon?** Google says Argon remains the ideal choice for most software engineering and knowledge work. Use `gemini-4-carbon` for harder tasks where its extra tokens pay off, and keep the model in config so switching is a test run.
