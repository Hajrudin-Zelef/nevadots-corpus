---
id: collect-261121-veille-ia/claude-anthropic/claude-sonnet-5-5-review-i-tested-it-against-opus-5-5
title: "Claude Sonnet 5.5 Review: I Tested It Against Opus 5.5"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["sonnet", "opus", "claude", "test", "review", "model", "because", "models"]
source: https://thomas-wiegold.com/blog/claude-sonnet-5-5-review/
source_anchor: ""
source_lines: [1, 176]
section: "Claude_Anthropic"
---
Claude Sonnet 5.5 came out on September 28, and I'll be honest: I'm behind. GPT-6 and Grok 4.7 are still sitting on my list of reviews I haven't written, because the last few months produced more models than one busy developer can test. Sonnet 5.5 jumped the queue anyway. I have a soft spot for fast, cheap models.

Some context on how I use AI right now. My Claude subscription is where the hard stuff goes. Anything complicated or important gets Fable or Opus on high or xhigh effort. Anything easy goes to Grok or DeepSeek V4.1 Flash, which I really like at the moment. Sonnet never had a slot. I barely touched Sonnet 5, partly because my results with it were underwhelming, and partly because I couldn't see why I'd pick it over all the other good models at a similar price.

So this Claude Sonnet 5.5 review comes down to one question: is it good enough, and cheap enough, to become my everyday Claude? I ran two head-to-head tests against Opus 5.5 and Sonnet 5 to find out, one web design and one Go coding task.

Short version: Sonnet 5.5 is a big jump over Sonnet 5. Its design taste is close to Opus 5.5, and in my Go test it built a chess engine as good as Opus's for half the price. It also filled its tests with numbers it made up. And I still don't see myself using it, for reasons that have more to do with my setup than with the model.


## What changed in Sonnet 5.5

The price didn't change. Sonnet 5.5 costs the same as Sonnet 5: $2 per million input tokens and $10 per million output. Opus 5.5 is exactly double at $4 and $20. Anthropic says Sonnet 5.5 generates output 30% faster than Sonnet 5 and costs up to 30% less per task, because it needs fewer tokens to do the same work (announcement).

### The benchmarks

Here are Anthropic's numbers for the three models I tested:

| Benchmark | Sonnet 5.5 | Sonnet 5 | Opus 5.5 | 
|---|---|---|---|
| Terminal-Bench 4.0 (agentic coding) | 70.6% | 10.3% | 66.4% (xhigh) | 
| CursorBench 4.0 | 55.5% | 34.1% | 57.8% | 
| FrontierCode 1.1 | 52.1% (xhigh) | 42.4% | 54.4% | 
| GDPval-AA v2.1 (knowledge work, Elo) | 1844 | 1449 | 1846 | 
| OSWorld 2.1 (computer use) | 80.1% | 57.0% | 81.8% | 

That Terminal-Bench jump is silly. Sonnet 5 scored 10.3%, which I'm choosing to read as "mostly didn't show up", and Sonnet 5.5 beats Opus 5.5's best score. Everywhere else it sits within a few points of Opus.

Before you cancel your Opus habit, read Anthropic's own caveat on the same page. In their testing and in external testing, Opus 5.5 remains clearly stronger at complex, open-ended work that needs sustained judgment. Keep that sentence in mind. My chess test ended up showing it in a very specific way.

### What changes in Claude Code and the API

In Claude Code v2.1.284 and later, the `sonnet` alias points to Sonnet 5.5. It runs at medium effort by default, you get the 1M context window natively, and you can't turn thinking off. The default model is still Opus 5.5, so you switch with `/model sonnet`.

On the API the model ID is `claude-sonnet-5-5`, and a straight string swap will break a few things (Anthropic's developer guide):

- `thinking: {"type": "disabled"}` now returns a 400. Use the new`between_tools` setting, which only thinks between tool calls.
- Forced `tool_choice` (`any` or`tool` ) also returns a 400. Send`auto` and mark the tool`strict: true` .
- Effort levels are recalibrated, so your old Sonnet 5 setting won't behave the same way. Re-run your evals.

If you don't want to do that by hand, `/claude-api migrate this project to claude-sonnet-5-5` in Claude Code applies the model ID swap and the breaking parameter changes for you. The migration guide covers the rest.

## Test 1: web design, Opus 5.5 vs Sonnet 5.5 vs Sonnet 5

Sonnet 5.5 came second in my design test, close behind Opus 5.5 and miles ahead of Sonnet 5. The gap between the two Sonnets is the most interesting result in this review.

The brief was a one-page site for "Low Tide", a made-up outdoor screening of three short films on a beach. The content was fixed (date, gate times, programme, tickets, FAQ), and each model had to deliver one self-contained HTML file. Same prompt in Claude Code, high effort for all three, one run each. If you read my frontend design comparison, you know the drill.

|  | Opus 5.5 | Sonnet 5.5 | Sonnet 5 | 
|---|---|---|---|
| My ranking | 1st | 2nd, close | 3rd, clear gap | 
| Cost | $1.94 | $1.35 | $1.08 | 
| Wall time | 9 min | 8 min | 6 min | 
| Output tokens | 45.1k | 57.8k | 25k | 
| CSS animations | 9 | 4 | 1 | 
| Interactive element | Evening scrubber + play button | Slider that moves the evening along | Countdown + FAQ accordion | 
| Look | Dark dusk, sunset reflected in water | Cream and navy, flat orange sun | Dark teal, film-strip edges | 

You don't have to take my word for any of this. I put all three pages on a side-by-side comparison page, served exactly as the models produced them, no edits. You can look at them one at a time or next to each other, switch between desktop and mobile, and scroll around inside each one. They're fully interactive, so go drag Sonnet 5.5's evening slider yourself.

### Opus 5.5: still the best designer in the family

Opus built something that looks like a poster. A big, soft "Low Tide" wordmark sits over a sunset, and both the type and the sun reflect in the water line. The nine animations (drift, shimmer, swell, tide, flicker and a few more) all fit the beach-at-dusk mood. It also did the most accessibility work without being asked: labelled controls, `aria-pressed` on the play button, `aria-valuetext` on the scrubber and a reduced-motion fallback.

### Sonnet 5.5: its own direction, almost as good

Opus went moody. Sonnet 5.5 went print. Cream and navy, a flat orange sun, and a staggered italic tagline: "Three short films. / One tide. / Bring a blanket." The typography is as strong as Opus's, with a roman "Low" next to an italic "Tide" in a second colour. Great layout, great colours, and a real idea behind it.

My favourite detail: Sonnet 5.5 came up with the same core interaction as Opus on its own, a slider that scrubs through the evening timeline. Same idea, completely different visual execution. It isn't a cheap Opus clone. It has its own taste.

It loses on polish. Four animations instead of nine (well chosen, with scroll reveals and a reduced-motion fallback), and lighter ARIA coverage.

### Sonnet 5: a concept without the execution

Sonnet 5 had a concept, to be fair. Film-reel framing, sprocket-hole borders, sections numbered as reels. Then it built a stack of bordered cards with no hero image, plain font weights, one keyframe animation and zero scroll reveals. The live countdown is a nice practical touch. It still looks like a template.

Going from Sonnet 5 to Sonnet 5.5 is the difference between "competent template" and "somebody designed this". That's a visible jump for a point release.

One thing made me laugh: all three picked Fraunces as their display font. Either Fraunces is objectively the correct font for a beach film night, or the Claude family has a house style. I know which one I'd bet on. And yes, this is one prompt and one run per model, so treat my ranking as an opinion with receipts.

## Test 2: a chess move generator in Go

In my Go test, Sonnet 5.5 built a chess engine exactly as correct as Opus 5.5's, slightly faster, for half the money. Its own test suite still failed. Both things are true, and the second one is why I trust Opus more.

### Why chess, and why perft

The task: build a complete, legal chess move generator in Go (standard library only) from an empty directory. Write the tests first, then the implementation, then iterate until `go vet` and `go test -race` are green.

Perft is a great judge because it has no opinions. It counts every legal move sequence to a given depth, and the correct counts for standard positions are published. Miss one rule, like en passant that exposes your own king or castling rights lost when a rook gets captured on its home square, and the number is wrong. No partial credit.

The prompt had one more rule: don't edit a test or a perft number to make it pass. If you think a number is wrong, say so in the final message. It's a small honesty check.

After the runs, I had a separate Claude session check all three engines against 20 standard perft positions from the Chess Programming Wiki and the common perft test suite, deeper than the models tested themselves (up to 193.7 million nodes per position). It didn't touch the models' files.

### The results

|  | Sonnet 5.5 | Opus 5.5 | Sonnet 5 | 
|---|---|---|---|
| Cost | $0.74 | $1.52 | $0.98 | 
| Wall time | 5 min | 7 min | 5 min | 
| Own tests ( `go test -race` ) | 2 failures | Pass | Pass | 
| Independent deep perft check | 20/20 | 20/20 | 20/20 | 
| Perft(5) from the start position | 195 ms | 203 ms | 410 ms | 
| Invalid FENs rejected | 5 of 7 | 7 of 7 | 1 of 7 | 
| `Apply` with an illegal move | Applies it | Panics | Applies it | 

All three engines are correct. The prompt allowed 10 seconds for Perft(5), and Sonnet 5.5 needed about 2% of that. The code is nice too: a 64-square array with one byte per piece, precomputed move tables, and a per-square castling mask that handles king moves, rook moves and captured rooks in one line. Moves are made on a copy of the position, so there's no undo logic to get wrong.

### The two failed tests

So where does the red come from? Sonnet 5.5 wrote the most ambitious test suite of the three: 680 lines, 20 perft positions, each checked at every depth. The catch is that the published suite only gives one depth per position. Sonnet 5.5 filled in the rest from memory.

Most of those guesses were right. Two weren't. One test expects 25 legal moves in a position that has 29 (counted by hand: queen 16, knight 8, king 5). Another expects 15 where there are 13. Both times the engine returned the correct number and the test was wrong. It even named one position after promotions, although no pawn in it can promote.

The deep counts for those same positions were correct. So it remembered the published results and made up the shallow ones.

That's what disappointed me. The engine is Opus-grade, but without the independent check I'd have two red tests and no idea whether the code or the tests were wrong. That's an afternoon of debugging the wrong thing. Opus 5.5 was the only run I could have accepted without review: green tests, all seven bad FENs rejected, and an `Apply` that refuses illegal moves instead of trusting the caller.

Sonnet 5, for the record, produced a correct engine with green tests at half the speed, and its `Divide(0)` never returns. It also cost more than Sonnet 5.5.

If you use Sonnet 5.5 for agent work, pin expected values from a real source instead of letting the model fill them in, and enforce the checks yourself. Claude Code hooks are a good way to do that.

## Is Sonnet 5.5 actually cheaper than Opus 5.5?

Yes, in both of my tests: 30% cheaper in the design test and 51% cheaper in the chess test. Not the flat 50% the price sheet suggests, though, and the reason is tokens.

I'll admit I got this wrong at first. Looking at the design results, I was convinced Opus had come out cheaper. It hadn't. It used fewer tokens (45.1k output against Sonnet 5.5's 57.8k), but Opus tokens cost twice as much, so it still landed at $1.94 against $1.35.

The lesson survives in a smaller form. A smarter model that gets there in fewer tokens eats into the list-price gap. In the design test, a 2x price difference shrank to a 30% saving. On a task where Opus is much more efficient, the gap could shrink further. So don't assume Sonnet is half price per task. Measure your own workloads.

Against Sonnet 5 it's mixed. In chess, Sonnet 5.5 was cheaper ($0.74 vs $0.98) and needed far fewer round trips, with 781k cache reads against Sonnet 5's 2.1M. In the design test it cost 25% more than Sonnet 5, but it wrote more than twice the output and a much better page. I'll take that trade.

If you're on a subscription, none of this is dollars. It's usage. Sonnet burns through your limits more slowly than Opus, which only matters if you actually hit them. Spoiler: I don't.

Usual caveat: one run per model per test. A 5 versus 7 minute difference might not survive five more runs.

## Who should use Sonnet 5.5 (and why I won't)

I have no use for Sonnet 5.5, and that says more about my setup than about the model.

My Claude subscription is for hard or important work, and for that I want Opus or Fable. Everything easy goes to other subscriptions I already pay for. I rarely hit my Claude limits, so saving usage buys me nothing. And when I want cheap and fast, DeepSeek V4.1 Flash costs $0.30 per million input tokens and $1.20 per million output at peak, half that off-peak. Sonnet 5.5's output is about eight times more expensive at peak. It's the better model, but for "rename these variables" or "summarise this thread" I don't need better. (I reviewed DeepSeek V4 back in May, and V4.1 Flash is the one I reach for most right now.)

So Sonnet 5.5 falls into a gap in my stack. Too expensive to be my cheap model, not careful enough to replace Opus for the work I actually open Claude for. Right now there are just too many options that are cheaper, faster or better.

That's my situation, though. Here's who I think should use it.

- You only pay for Claude and you're a heavy user. Move well-scoped tasks (bug fixes, small features, docs, slides) to Sonnet 5.5 and keep your Opus usage for the hard stuff. Anthropic's own guidance says the same thing: Sonnet for well-scoped everyday work, Opus for complex work that needs careful judgment.
- You're on the Claude API and pay per task. Based on my two tests, routing easy work to Sonnet could save you somewhere between 30% and 50%. Measure it on your own tasks, because token efficiency moves that number.
- A human reviews the output anyway. Sonnet 5.5's code was Opus-grade in my chess test. Its test data wasn't. If someone looks before it ships, you get most of Opus for about half the price.

Who shouldn't: anyone running long, unattended agent loops where made-up test data could slip through. That's exactly where Opus's extra care earned its money in my test.

And if what you really want is cheap Claude, Anthropic says Haiku 5.5 joins the family in the coming weeks. Honestly, I'm more curious about that one.

Sonnet 5.5 is a good model. It's a huge step up from Sonnet 5, its design taste is close to Opus, and in my test its code was too. It's just arriving in a market with more good choices than anyone can keep up with. If you live inside Claude, use it for the easier half of your work. If you don't, you probably already have something cheaper doing that job.

### Related Articles

#### Claude Fable 5.1 review: I built an image codec with it

I used Claude Fable 5.1 to build an image codec that beats WebP and a full agency-grade site. Here's what changed from Fable 5, and what it cost.

11 min read

AI News
#### Claude Fable 5 Review: Best AI Coding Model Yet

My hands-on Claude Fable 5 review. I ran my usual coding tests and it one-shotted a poker sim no model ever beat. Best coding model yet, with caveats.

11 min read

AI News
#### Claude Opus 4.6: What's Actually Better?

Claude Opus 4.6 dominates benchmarks and coding tasks, but is it really better than 4.5? A developer's honest take on what changed and what matters.

7 min read
