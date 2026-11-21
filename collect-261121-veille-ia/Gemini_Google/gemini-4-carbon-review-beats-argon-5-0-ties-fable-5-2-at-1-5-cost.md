---
id: collect-261121-veille-ia/gemini-google/gemini-4-carbon-review-beats-argon-5-0-ties-fable-5-2-at-1-5-cost
title: "Gemini 4 Carbon review: beats Argon 5-0, ties Fable 5.2 at 1/5 cost"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["carbon", "argon", "answer", "fable", "google", "gemini", "cost", "prompts"]
source: https://llmtest.io/blog/gemini-4-carbon-review-2026
source_anchor: ""
source_lines: [1, 46]
section: "Gemini_Google"
---
Gemini 4 Carbon is the better model than Gemini 4 Argon on hard work, and it is much slower and more expensive per answer. In our test it won 5 of 8 prompts against Argon and lost none. Against Claude Fable 5.2 it split 2-2 with 4 ties, at about a fifth of Fable's cost per answer. The price is the catch: the $2 input and $10 output rates match Argon only until November 30, then double.

|  | Carbon | Argon | Fable 5.2 | 
|---|---|---|---|
| Input / output per 1M tokens | $2 / $10 (then $4 / $20) | $2 / $10 | $8 / $40 | 
| Prompts won by Carbon (of 8) | n/a | 5 | 2 | 
| Prompts won by the rival (of 8) | n/a | 0 | 2 | 
| Avg cost per answer | $0.0329 (vs Argon run), $0.0262 (vs Fable run) | $0.0079 | $0.1393 | 
| Avg latency | 47.6 s, 42.6 s | 17.0 s | 36.3 s | 
| Avg output tokens | 2,255, 2,087 | 765 | 2,945 | 

Carbon figures are listed per run: the first value is from the Argon run, the second from the Fable run. Prices come from OpenRouter's model list, which shows a 1,048,576-token context for Carbon, and from Google's availability post. Everything else is from our runs.

Google made Carbon generally available on 2026-11-10. It is in Google AI Studio and Gemini for business for developers, and in the Gemini app, Google Antigravity and AI Mode in Search for AI Ultra subscribers. A cyber variant stays limited to the Fairwind Program by application. Google positions Argon as the default for most software and knowledge work, and Carbon for harder tasks such as multi-step debugging and open-ended research, noting that Carbon spends more tokens reasoning and verifying.

Google also reports ranks on CursorBench 4.0, Perplexity's WANDR and Zapier's AutomationBench, and a lower per-attempt breach rate in simulated attacks than Argon. Those are vendor numbers and we did not test them. We tested the thing Google's own positioning predicts: whether the extra reasoning pays off on hard prompts, and what it costs.

We wrote 8 prompts at the hard end of everyday engineering work: a Postgres job-queue deadlock, a TypeScript cache race, a Kubernetes restart loop diagnosed from facts, a window-function SQL report, a queue-technology comparison, a contract-cap ambiguity, an interval-merge algorithm with an open/closed trap, and an ask-or-act agent decision. We ran `google/gemini-4-carbon` head-to-head against `google/gemini-4-argon` and `anthropic/claude-fable-5.2` through OpenRouter, with a 4,000-token output cap. The judge was `anthropic/claude-sonnet-5.5`. It judged every prompt twice with the answer positions swapped, and a win counts only when both orders agree; otherwise it is a tie. Total spend was $2.45 of a $5 cap, on 2026-11-10. Eight prompts is a small sample, so treat the results as a direction. One Carbon answer and one Fable answer hit the 4,000-token cap and were cut off. The saved runs are in the repo under `server/blog/runs/gemini-4-carbon-review-2026/`.

Carbon won the deadlock, the SQL report, the queue comparison, the interval algorithm and the ask-or-act prompt. The other three were ties or judge disagreements. Argon never won. On the job-queue prompt the judge said:

Response A is better: it finds more of the real bugs and gives a safer fix, though both fixes would work in practice.


The clearest gap was the SQL report, where Argon's answer contained a broken first draft:

A is correct and runnable, while B's query does not run and its notes are shaky. A wins clearly.


The cost of that quality is large. Carbon wrote about three times as many output tokens (2,255 against 765) and took 47.6 seconds per answer against 17.0, so an average answer cost $0.0329 against $0.0079. Doubling the token price after November 30 would, by simple arithmetic, put that near $0.066 per answer, around eight times Argon's. For routine work Argon is the right default, as Google says. Use Carbon where a wrong answer costs more than the tokens.

This was close. Carbon won the queue comparison and the ask-or-act prompt, Fable won the deadlock and the algorithm prompt, and four ended as ties. On the deadlock, the judge preferred the more complete bug list from Fable:

Overall B is more complete and more accurate about what it can and cannot know, while A's tie-breaker and index tip are a smaller advantage.


On the interval algorithm, Carbon's code was right but ran out of tokens:

Response B is better: A's code is correct but its answer is cut off, while B is correct and complete.


Carbon is a heavy writer, so give it a generous output limit. The economics are the story here. Carbon averaged $0.0262 per answer against $0.1393 for Fable 5.2, about a fifth, with a similar number of wins. Fable also produced more output (2,945 tokens on average), which is part of why it cost more. After the November 30 price change, Carbon would still be roughly $0.052 per answer on the same arithmetic, well under Fable. For how Fable 5.2 compared with its own predecessor, see our Claude Fable 5.2 review.

Google says Carbon is included for AI Ultra subscribers in Antigravity, the Gemini app and AI Mode, and is billed per token for API users. We did not check current plan prices, so we can't give a break-even point. If you use it through the API, the November 30 price change is the number to plan around.

Choose Carbon over Argon for hard debugging, SQL and analysis where correctness matters and a 45-second wait is fine. Keep Argon for everything routine, or try the cheaper, faster Gemini 4 Hydrogen for agent loops. Against Fable 5.2, Carbon is a credible cheaper alternative: equal wins in our run at about a fifth of the cost per answer, with more cut-off risk at tight token limits. Lock in your evaluation before November 30, because the price doubles. For tasks where model choice is more about the prompt, see our posts on the best LLM for SQL generation and the best LLM for code review. We have not tested the Fairwind-only cyber variant.
