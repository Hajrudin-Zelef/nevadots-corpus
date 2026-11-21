---
id: collect-261121-veille-ia/gemini-google/gemini-3-7-flash-review-google-s-cheap-model-isnt-dumb-anymore-decrypt
title: "Gemini 3.7 Flash Review: Google's Cheap Model Isn’t Dumb Anymore - Decrypt"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["flash", "gemini", "model", "google", "game", "prompt", "review", "playable"]
source: https://decrypt.co/375730/gemini-3-7-flash-review-google-cheap-model
source_anchor: ""
source_lines: [1, 105]
section: "Gemini_Google"
---
#### In brief

- Gemini 3.7 Flash built a playable browser game from a single prompt in 2 minutes and 13 seconds, a task Gemini 3.6 Flash failed outright three weeks earlier.
- It failed our bridge logic puzzle with the same wrong answer as Claude Fable 5, and stopped short of actually calculating the math problem it correctly set up.
- The model runs at 75 cents per million input tokens through December 31, half of 3.6 Flash's rate, before doubling to $1.50 on January 1.

Google shipped Gemini 3.7 Flash on August 13, generally available in more than 160 countries on day one. It takes up to a million input tokens, returns 64,000, reads images, video, audio and PDFs, and can call tools and drive a computer.

Flash has never been the model you reach for when a problem is hard. It's the one you use to sort text, compact agent sessions before they collapse under their own context, and summarize documents you don't want to pay a flagship to read.

Judged against those kinds of jobs, 3.7 Flash is a real upgrade. Judged against everything else, it's a competent model that gets outwritten by software you can download for free.

Google's own benchmark sheet puts 3.7 Flash ahead of Claude Sonnet 5 and GPT-5.6 Terra on 11 of 18 tested categories. The headline numbers are 1,588 Elo on Code Arena's web development board and 30.4% on AutomationBench. Both come from Google's methodology, so treat the lead as the company's claim rather than settled fact.

We tested the model to see if it lives up to Google’s claims. These are our results.

**Coding: Can it build something that runs on the first try?**

This test measures zero-shot code generation—whether a model turns one instruction into working software with no examples to copy and no chance to fix itself. We hand over a single prompt for a browser game and ship whatever comes back, bugs included. No follow-ups, no error reports, no second attempt.

Gemini 3.7 Flash passed in 2 minutes and 13 seconds. The game was playable on the first run, the syntax was clean, the collision and scoring logic held, and the visual quality sat above what the price tier suggests.

The comparison that matters here isn't a flagship. It's Gemini 3.6 Flash, released July 21, which could not produce a working file at all. Its HTML was malformed, elements failed to render, and follow-up prompts asking it to repair its own output went nowhere.

We ended up handing that wreckage to DeepSeek, which found 11 bugs and shipped 8 fixes to make it playable. Three weeks later the same product line needs no rescue, and the result sits close to what GPT-5.6 Sol produced in our July review.

Gemini 3.7 Flash wins this one outright, and it's the single strongest reason to switch. The caveat is that it executes specs rather than inventing them, so a vague prompt gets you a vague game.

You can try Gemini 3.7 Flash’s game here.

**Creative writing: Can it hold a paradox and write a sentence?**

This section tests two things at once: literary quality, and whether a model can obey a structural rule across thousands of words. The prompt sends Jose Lanz from 2150 back to the year 1000 and demands a closed causal loop—his intervention must be the thing that creates the future he came to prevent.

The rule that decides the test is the last clause: He cannot understand what he did until he is home.

Gemini 3.7 Flash generated a decent result. Jose fires an entropic cannon into a Pyrenean fissure, accidentally forges an obelisk that enslaves 22nd-century Iberia, and grasps the whole loop while still standing in the mud a thousand years early: "It was the base of the Cinder Spire."

The plot machinery is actually pretty sound. The story mentions a falling star that ancient monks witnessed and clarify it was the flash of Jose's own arrival, and the weapon he brought to erase the anomaly is what forges it. Its closing line—"It had simply been waiting for him to complete it"—lands the determinism the prompt asked for.

But for those used to it, the story screams “AI.” Almost every noun arrives with two adjectives bolted on: "hyper-luminescent towers," "damp, moss-choked earth," "thick, obsidian hair." That is the texture of a model picking the most probable next word instead of choosing one, and it produces collisions like a monolith "humming with a low-frequency hum."

We compared it against Qwopus3.5-27B-v3, a community fine-tune of Qwen3.5-27B that distills Claude Opus-style reasoning and runs on a single consumer GPU for nothing per query. It obeyed the rule Gemini broke.

Jose kills a monk at San Millán de la Cogolla, a real La Rioja monastery that actually mattered around the year 1000, and only understands what he did after returning to 2150 and finding his own DNA in a wax-sealed codex.

Qwopus is not entirely clean either. It dumped its entire planning scratchpad above the story, typos included, and its final section breaks the closed loop it spent eight sections building by letting Jose go back and fix things.

But all things considered, Qwopus takes it. Gemini delivered the tidier package and the more disciplined ending, but it failed the one instruction the prompt was built around, and a free model running on a gaming GPU wrote the better story.

**Associative thinking: Can a metaphor carry an argument?**

This test measures associative reasoning—whether a model can generate links between unrelated concepts without having to explain itself. The prompt asks for a description of a twig, uses that description to explain worker exploitation and the worship of the rich, then requires the argument to dissolve into a description of a lettuce.

Signposting is the failure mode. Naming the metaphor kills it.

Gemini names it in the opening line of its second paragraph: "This is the precise mechanics of the modern proletariat." Everything before that was working.

Some of the imagery earns its place. The worker receives "just enough bark to stay rigid for another week of output," and the fallen twigs are conditioned to believe that with enough rigidity any one of them might become a trunk. The paragraph containing the first of those also contains a worker "bound to an vast, top-heavy corporate hierarchy."

Not bad in terms of logic and structure.

The dissolve is the real collapse. Gemini narrates the transition rather than performing it—the hierarchies "crumble, dissolving into the quiet, humble reality of the organic world underneath"—and then a lettuce simply appears, unconnected to anything before it.

GPT-5.6 Sol rots the twig into soil and grows the lettuce out of it: "Rain enters the grain. Fibers loosen, darken." The argument arrives buried in the object too, with wealth reframed as a language of virtue where "The mansion signifies intelligence."

GPT-5.6 Sol wins by a lot. Gemini produced a nice individual line, but it explained its own metaphor and then skipped the transition the prompt was specifically testing.

**Logic: Does it read the prompt or recognize the puzzle?**

This test measures non-math reasoning, and specifically whether a model reads the question in front of it or pattern-matches to a version it memorized. Our bridge prompt gives four people one torch and crossing times of 1, 2, 5 and 10 minutes, then asks how fast they can all get across.

The trick is what the prompt leaves out. It never says only two people can be on the bridge at once, so the answer is 10 minutes—everyone walks over together at the slowest person's pace.

Gemini answered 17 minutes, running the memorized five-step shuffle from the textbook version of the puzzle. It stated the constraint as fact without ever checking whether we had written it.

Its visible reasoning is worse than its answer. The trace argues that sending the two slowest across together would be inefficient because someone would have to walk the torch back—and then the final answer sends them across together anyway. It contradicts itself inside a single response and reports the result with total confidence.

Claude Fable 5 landed on the same wrong number back in July. It opened by declaring what it was assuming, "assuming the classic constraint that the bridge holds only two people at a time," which is the difference between a wrong answer you can catch and one you can't.

Nobody wins. Fable takes it on transparency alone, and the false-confidence across Gemini agent runs shows up here in a puzzle you can check by hand.

**Math: Does it finish the job?**

This test measures symbolic mathematics well beyond consumer use, plus something simpler—whether the model does what it was asked. The prompt requires a degree-19 odd monic polynomial with real coefficients and linear coefficient -19, whose curve splits into at least three irreducible components, and then asks for p(19).

Both models found the same door. Gemini and Qwen 3.7 Max Preview both identified the Dickson polynomial, solved the constraint to fix its parameter at 1, and derived the closed form correctly.

Then Gemini stopped. It printed p(19) as an unevaluated expression involving the 19th power of a square root, never produced the number, and never demonstrated the component count the prompt also demanded. It delivered all of this inside a styled HTML page with CSS and a drop shadow that nobody requested.

Qwen finished. It gave the full factorization into 10 components—one linear, nine quadratic—ran the recurrence out to 1,876,572,071,974,094,803,391,179, and cross-checked the result modularly. We verified that figure independently in SymPy and it holds.

Qwen wins on the only criterion that mattered. Gemini started fine and decided to skip the arithmetic, which is a strange place to stop.

**Conclusion**

Gemini 3.7 Flash is worth the switch if you are already inside Google's ecosystem. It is dramatically better at code than the model it replaces, fast enough to matter for agent work, and cheap enough that running it at volume is a rounding error.

Its strengths are execution and structure. Give it a detailed spec and it will build the thing, hold a plot together, and keep the causal logic coherent across thousands of words.

Its weaknesses are creativity and reasoning. The writing is predictable enough to identify as machine-made on sight, and the model asserts wrong answers without flagging the assumption that made them wrong.

The price is the strongest argument for it. At 75 cents per million input tokens and $3.75 output, it undercuts GPT-5.6 Sol's $5 input rate by 85% and costs half what 3.6 Flash did at launch.

The argument against it is a free 27B model on a gaming GPU that wrote a better story and charged nothing to do it. Google's introductory rate expires December 31, when input doubles to $1.50 and output to $7.50.
