---
id: collect-261121-veille-ia/linux-kernel/mold-3-0-0-released
title: "Mold 3.0.0 Released"
domain: linux-kernel
role: reference
task: article
actors: []
dates: []
keywords: ["genai", "should", "mold", "quality", "rust", "vibecoding", "slop", "people"]
source: https://lobste.rs/s/9srb4r/mold_3_0_0_released
source_anchor: ""
source_lines: [1, 93]
section: "Linux_Kernel"
---
We are rewriting mold from C++ to Rust, and 2.42.1 is likely to be the last C++ version of the linker unless we need to make another patch release. The Rust version of mold will be released as mold 3.0. A tool like mold needs to be developed with the expectation that it may remain in use for decades. Given that expected lifespan, mold is still relatively early in its life, and we believe that rewriting it in Rust at this point is the right decision.

Um. Why is even this tagged vibecoding? It's fairly off topic, as a software release with no technical content, but nothing LLMish seems to be mentioned at the link

And it's frustrating that lobste.rs lumps this into "vibecoding". Using genAI to help port an existing codebase, that you wrote yourself, carefully reviewing and testing each step, is not the same things as generating a project from the ground up using only prompts and no code review.

There are many things wrong with genAI, I wish we could reverse the usage trend. But we have to stop labeling anything it touches "slop", there's high quality stuff out there (FWIW, mold 3.0 looks high quality to me). Using AI assistance is becoming normal, to write browsers, compilers, desktop envs, etc. Anything coming from Google, Red Hat, and many others. If lobste.rs tags it all as "vibecoding", the tag loses it usefulness.

I disagree, I feel we should use the term slop for anything that GenAI has touched, be it code, visual art, writing or whatever else. GenAI is very unethical and I don't think we should accept its existence. GenAI as a whole, for any use, should be objected to.

GenAI is very unethical and I don't think we should accept its existence. GenAI as a whole, for any use, should be objected to.

I largely agree with that.

But dogmatically putting everything in the same category makes your message inaudible. The term "slop" predates GenAI, for most people it describes the quality, and they may find that some GenAI output is slop while some other is good. Similar dichotomy with "vibecoding". If you ignore nuance that people care about, they won't listen to you.

Also, pushing against GenAI by pointing out bad quality output is quickly becoming ineffective. The tools are improving, people are learning to use them well, and top quality is not always necessary. What we need to point out are the deeper issues: ethical, societal, environmental, etc.

GenAI exists, and will keep growing. If we want to limit the bad consequences, we need to talk to people, not just bluntly reject the tech.

In my views, "slop" is something soulless, and is not quite a reflection of quality.

I agree that we shouldn't fight against it by saying its output is bad quality; we should fight against it by saying it's soulless, and because of all of the negative externalities. It is, in its essence, a machine which devalues humanity and erodes our bonds with one-another.

Though, I do think we should bluntly reject the tech. We should definitely talk to people and have empathy, we should definitely be kind. The whole reason I am anti-GenAI is because I am pro-people, so of course we should be! But I do not think we should accept GenAI in any way.

That it will keep growing is a decision we as a society have to make, and I feel we should push that decision towards "no, we don't want it, and it should go away".

The existence and naming of the tag is largely orthogonal to the acceptance or rejection of the technology.

Is a user filtering on "rust" rejecting Rust? Not really, they're just not interesting in reading about Rust on lobste.rs. Similarly, you might not be interested in reading about GenAI assisted software development here.

The name of the tag, "vibecoding", is clearly opinionated against the tech : it relates to a way of using GenAI that is decried even amongst GenAI users. It's not a neutral term like GenAI is.

I don't mind the existence of the "vibecoding" tag, I would probably use it if could be applied judiciously, for example on stories about the Bun rewrite. But as it is, it's applied both too broadly (there's no indication that the mold rewrite was vibecoded, just AI-assisted) and too haphazardly (missing for example from many Ladybird posts).

Using genAI to help port an existing codebase, that you wrote yourself, carefully reviewing and testing each step, is not the same things as generating a project from the ground up using only prompts and no code review.

In order to understand how this is possible, unfortunately, you have to actually have used AI tools. And I'm convinced that the people applying the vibecoding tag en masse have clearly no direct experience with them. (Or maybe they experienced what these tools did a year ago and still believe that they are the crap they used to be.)

OK well I found the rewrite to Rust part interesting:

https://github.com/rui314/mold/releases/tag/v2.42.1

https://news.ycombinator.com/item?id=49963385

Um. Why is even this tagged vibecoding? It's fairly off topic, as a software release with no technical content, but nothing LLMish seems to be mentioned at the link

Every recent commit message stinks of LLM banter and Claude is the second top contributor in the repo.

The author mentions using AI for doing the port.

The author used AI assistance for the port

And it's frustrating that lobste.rs lumps this into "vibecoding". Using genAI to help port an existing codebase, that you wrote yourself, carefully reviewing and testing each step, is not the same things as generating a project from the ground up using only prompts and no code review.

There are many things wrong with genAI, I wish we could reverse the usage trend. But we have to stop labeling anything it touches "slop", there's high quality stuff out there (FWIW, mold 3.0 looks high quality to me). Using AI assistance is becoming normal, to write browsers, compilers, desktop envs, etc. Anything coming from Google, Red Hat, and many others. If lobste.rs tags it all as "vibecoding", the tag loses it usefulness.

I disagree, I feel we should use the term slop for anything that GenAI has touched, be it code, visual art, writing or whatever else. GenAI is very unethical and I don't think we should accept its existence. GenAI as a whole, for any use, should be objected to.

I largely agree with that.

But dogmatically putting everything in the same category makes your message inaudible. The term "slop" predates GenAI, for most people it describes the quality, and they may find that some GenAI output is slop while some other is good. Similar dichotomy with "vibecoding". If you ignore nuance that people care about, they won't listen to you.

Also, pushing against GenAI by pointing out bad quality output is quickly becoming ineffective. The tools are improving, people are learning to use them well, and top quality is not always necessary. What we need to point out are the deeper issues: ethical, societal, environmental, etc.

GenAI exists, and will keep growing. If we want to limit the bad consequences, we need to talk to people, not just bluntly reject the tech.

In my views, "slop" is something soulless, and is not quite a reflection of quality.

I agree that we shouldn't fight against it by saying its output is bad quality; we should fight against it by saying it's soulless, and because of all of the negative externalities. It is, in its essence, a machine which devalues humanity and erodes our bonds with one-another.

Though, I do think we should bluntly reject the tech. We should definitely talk to people and have empathy, we should definitely be kind. The whole reason I am anti-GenAI is because I am pro-people, so of course we should be! But I do not think we should accept GenAI in any way.

That it will keep growing is a decision we as a society have to make, and I feel we should push that decision towards "no, we don't want it, and it should go away".

The existence and naming of the tag is largely orthogonal to the acceptance or rejection of the technology.

Is a user filtering on "rust" rejecting Rust? Not really, they're just not interesting in reading about Rust on lobste.rs. Similarly, you might not be interested in reading about GenAI assisted software development here.

The name of the tag, "vibecoding", is clearly opinionated against the tech : it relates to a way of using GenAI that is decried even amongst GenAI users. It's not a neutral term like GenAI is.

I don't mind the existence of the "vibecoding" tag, I would probably use it if could be applied judiciously, for example on stories about the Bun rewrite. But as it is, it's applied both too broadly (there's no indication that the mold rewrite was vibecoded, just AI-assisted) and too haphazardly (missing for example from many Ladybird posts).

IMHO the tag needs to be split.

In order to understand how this is possible, unfortunately, you have to actually have used AI tools. And I'm convinced that the people applying the vibecoding tag en masse have clearly no direct experience with them. (Or maybe they experienced what these tools did a year ago and still believe that they are the crap they used to be.)

One month ago it was a c++ project, now it's rust. It can't have been done manually.

Everything is vibecoding these days except if it's explicitly "not vibecoding".

[Comment removed by its author]
