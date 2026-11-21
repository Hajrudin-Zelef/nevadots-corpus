---
id: collect-261121-veille-ia/claude-anthropic/claude-fable-5-1-review-i-built-an-image-codec-with-it
title: "Claude Fable 5.1 review: I built an image codec with it"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-09-01"]
keywords: ["fable", "claude", "codec", "image", "webp", "about", "good", "real"]
source: https://thomas-wiegold.com/blog/claude-fable-5-1-review/
source_anchor: ""
source_lines: [1, 116]
section: "Claude_Anthropic"
---
For the last two months I barely touched Claude. Not out of protest. Everything else just got good. Grok, ChatGPT, Qwen 3.8 Max, Kimi K3, DeepSeek V4 Flash, GLM. Pick one, they all work now. When I ran Qwen 3.8 Max through my usual tests I remember thinking the gap I'd been paying a premium for had mostly closed. Competition is good. Good for me as a customer, and clearly good for Anthropic, who spent the middle of this year under real pressure.

So when Claude Fable 5.1 landed on September 1, my expectations were low. Fable 5 was already excellent. What's a point-one release supposed to do?

Then I gave it two jobs. Build an image codec that beats WebP. Build a website good enough that an award-winning design studio would put its name on it. Both worked. That's the short version of the review. The longer version is about why, and about whether it's worth what it costs, which is a lot.

## What Claude Fable 5.1 actually is

Claude Fable 5.1 is Anthropic's frontier model, released September 1, 2026, sitting above Opus 5 in a tier they call Mythos-class. It shares its weights with Claude Mythos 5.1. The only real difference between the two is safeguards: Fable is generally available with conservative classifiers on top, Mythos goes out through trusted-access programs with those classifiers loosened.

The specs, from the Bedrock model card:

| Context window | 1M tokens (default and max) | 
| Max output | 128K tokens | 
| Reasoning | Adaptive thinking, always on, five effort levels from low to max, defaults to high | 
| Knowledge cutoff | June 2026 | 
| Modality | Text and image in, text out | 
| API string | `claude-fable-5-1` | 

Two of those matter more than they look. The June 2026 cutoff is the freshest of any Claude model, which you notice immediately with libraries that moved this year. And adaptive thinking can't be turned off, so every request carries reasoning tokens whether you wanted them or not. Remember that for the pricing section.

The other thing worth knowing is routing. Some cybersecurity and biology queries get flagged and answered by Opus instead of Fable, and you aren't billed at Fable rates when that happens. Anthropic says cyber false positives are down roughly 60% per session versus Fable 5. I didn't trip a single one across either test, but if you do authorised security work you still will, and the Claude Code issue tracker has months of complaints about exactly that.

## Test one: a Go image codec that beats WebP

I picked a codec deliberately. It's hard in a way most benchmark tasks aren't. There are a million Express APIs in the training data and not a million novel entropy coders. You have to hold a lot of state, make decisions that only pay off three files later, and be right about the maths, because when you're wrong about the maths you don't get a warning. You get garbage pixels.

I asked for something better than WebP, in Go, and I let it run.

It landed in one shot. One relatively short prompt, no back and forth, no arguing about whether the bug was real. That surprised me more than the result did.

The result: the codec beats WebP on compression by somewhere between 1 and 9 percent depending on the image, and it's much faster on top of that. One to nine percent sounds modest until you remember WebP has had thirteen years of engineers pushing on it. I did not expect to beat it at all, let alone beat it on speed.

Now the caveats, and they're big enough that I want them in the same section rather than buried at the bottom. This is an experiment. You could not ship it. Compression ratio is maybe a quarter of what makes a real codec viable, and the rest is decode performance on hardware you don't control, progressive rendering, colour accuracy, alpha handling, metadata, streaming, plus the small matter of every browser on earth agreeing to support it. Beating WebP on one axis is not the same as replacing it.

The test set was small too. Under 50 images. That's enough to tell me something interesting happened and nowhere near enough to claim general performance. A real evaluation needs thousands, spread across photos, illustrations, screenshots, and all the horrible edge cases that break codecs in production.

Take it as what it was. Not "can I ship this," but "can the model do a genuinely hard thing from a cold start." It could.

## Test two: a website an agency could ship

The second test was the opposite kind of hard. Not correctness, taste.

The brief was a site for an award-winning web design company, which is designed to catch models out. "Professional website" is what every model produces by default. "Award-winning" is what almost none of them can do, and the gap between the two is where all the interesting failure lives. I've run six models through this kind of frontend test before and most land in competent-but-generic territory.

The twist I added: I asked for a portfolio, with a few examples of the studio's best work. That turns one design problem into several at once. A portfolio whose sample work is mediocre fails the brief no matter how nice the wrapper is, and a model with only one good design in it gets caught immediately, because you can watch it recycle the same ideas across every example.

Fable 5.1 didn't get caught. The result is at fable51-webdesign.thomas-wiegold.com. The portfolio pieces are distinct from each other and individually good, which is the bit I didn't expect. The animations and transitions are what I keep going back to look at. They're timed properly and they carry the eye somewhere, and that's usually where generated frontends fall apart.

One prompt. No revisions. Good on the first try, same as the codec.

I'd put it in front of a real client tomorrow. This is the test I'd point at if someone asked what "creative" means for a coding model. Not that it wrote CSS. That it made choices, several times over, without repeating itself.

## What actually changed from Fable 5

Here's where I have to be careful, because I like this model and that's exactly when you start seeing improvements that aren't there.

My honest read: Fable 5.1 feels better to work with than Fable 5. More direct. Less talkative. It goes at the goal instead of narrating its way toward it. It might be slightly faster, though I didn't time anything properly, so treat that as a feeling rather than a measurement.

What I can't tell you is that the output is better. My Fable 5 results were already great. My Fable 5.1 results are also great. If there's a quality difference in the final artifacts I'm not confident enough to claim it.

The interesting part is that "less talkative" turns out to be measurable. Snorkel AI found that on frontier coding tasks both models solved, Fable 5.1 used 58% fewer output tokens than Opus 5 and finished 36% faster. Every reported roughly twice the speed at about half the tokens. So the thing I noticed as a vibe is a real property, and it's the one I'd care about most day to day.

On benchmarks, two numbers are worth your attention and most of the rest aren't. Terminal-Bench-Science 0.1 went from 24.7% on Fable 5 to 52.6% on 5.1, which is a jump you rarely see on a point release. And Artificial Analysis put it at the top of its Intelligence Index with a score of 66 at max effort, ahead of Opus 5 at 63 and GPT-5.6 Sol at 61.

One correction while I'm here, because I keep seeing it repeated. The 95% SWE-bench Verified figure doing the rounds is Fable 5's number, not 5.1's. Anthropic didn't headline a SWE-bench result for 5.1 at all. If a review quotes it as a 5.1 score, the person writing it didn't read the launch post.

## The cost problem, which has not gone away

Fable 5.1 costs $10 per million input tokens and $50 per million output. Unchanged from Fable 5, and exactly double Opus 5 on both sides.

The headline improvement is cache reads, cut 75% to $0.25 per million from $1.00. That's a strange enough number to say out loud: Fable 5.1's cached input is now cheaper than Opus 5's, even though its base rate is double. If your workload is a long-running agent chewing through the same context, the maths changed in your favour.

Anthropic frames this as roughly 25% cheaper for typical workloads and up to 45% for agentic ones. Artificial Analysis found the opposite on its own Intelligence Index runs: about 20% more expensive per task than Fable 5, because 5.1 burns more tokens getting there. Both can be true. Which applies to you depends on your cache hit rate, and you won't know until you measure it.

For those of us on subscriptions rather than the API, the practical version is simpler. Credits go fast. That's the part that stops me reaching for Fable by default, and it's the same complaint I had about Fable 5.

## When it's worth it

Default to Opus 5. Escalate to Fable 5.1 when the task is hard, important, or creative. That's not me being cautious, it's what Anthropic's own docs recommend, and it matches how I've actually been using it.

A concrete threshold: if Opus 5 at high effort fails more than about 15 to 20 percent of your long-horizon tasks, or cache-heavy agent workloads dominate your bill, Fable earns the premium. Otherwise you're paying double for a difference you won't notice on a CRUD endpoint.

Now the part where I disagree with the leaderboards.

The number people point at is Artificial Analysis's Coding Agent Index v1.1, where GPT-5.6 Sol scored 80 against Fable's 77. Two problems with it. The 77 is Fable 5's score, not 5.1's, so the comparison is a generation out of date before you even get to the argument. And I don't think the ranking reflects what happens when you sit down and actually use these models.

I think the benchmarks are broken, and not as a vague complaint. Look at how the numbers get made. Anthropic's cross-vendor comparison columns were run by Anthropic, under Anthropic's conditions. Artificial Analysis revised its own methodology mid-year and Fable 5's launch score of 64.9 became 60, with a note that the two aren't interchangeable. On OSWorld and AutomationBench, runs where a safeguard fired scored zero, which drags the average down for a reason unrelated to capability. And nearly every published chart is a cost-accuracy curve across five effort levels, so a number quoted without its effort tier tells you close to nothing.

None of that is dishonest. It's that a benchmark measures a task, and the task isn't the job. My job is ambiguous briefs, taste, and problems where the hard part is deciding what to build in the first place. No index scores that, and no index is going to.

So I'll say it plainly rather than hedge it into mush. Fable 5.1 is number one right now. It built a working codec and a multi-piece portfolio site in one prompt each, and nothing else I have access to does that. If you want a number to agree with me, it tops the Artificial Analysis Intelligence Index at 66. For the head to head against the alternatives, see my Opus 5 versus GPT-5.6 versus Grok 4.5 comparison.

If you take one thing from this, take the test structure rather than my verdict. Pick two tasks, one genuinely hard and one that needs taste. Run both against whatever you use today and against Fable 5.1. Then compare cost per finished task, not tokens per second and not how impressive the output looks in isolation. I went in expecting a nothing release and came out having changed my default for a whole class of work. Your two tasks might say something different. That's the point of running them.

### Related Articles

#### Claude Sonnet 5.5 Review: I Tested It Against Opus 5.5

Claude Sonnet 5.5 review: I tested it against Opus 5.5 and Sonnet 5 on a web design and a Go chess engine. Real costs, results, and who should use it.

14 min read

AI News
#### Claude Fable 5 Review: Best AI Coding Model Yet

My hands-on Claude Fable 5 review. I ran my usual coding tests and it one-shotted a poker sim no model ever beat. Best coding model yet, with caveats.

11 min read

AI News
#### Claude Opus 4.6: What's Actually Better?

Claude Opus 4.6 dominates benchmarks and coding tasks, but is it really better than 4.5? A developer's honest take on what changed and what matters.

7 min read
