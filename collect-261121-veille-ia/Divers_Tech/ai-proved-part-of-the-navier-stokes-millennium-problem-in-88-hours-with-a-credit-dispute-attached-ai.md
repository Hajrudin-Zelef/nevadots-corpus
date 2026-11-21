---
id: collect-261121-veille-ia/divers-tech/ai-proved-part-of-the-navier-stokes-millennium-problem-in-88-hours-with-a-credit-dispute-attached-ai
title: "AI Proved Part of the Navier-Stokes Millennium Problem in 88 Hours — With a Credit Dispute Attached - AI Signal"
domain: divers-tech
role: reference
task: article
actors: []
dates: []
keywords: ["problem", "openai", "hours", "navier-stokes", "equations", "part", "page", "proved"]
source: https://aisignal.day/en/navier-stokes-ai-proof-2026/
source_anchor: ""
source_lines: [1, 77]
section: "Divers_Tech"
---
7 min read

On the morning of September 8, OpenAI announced that it had cracked a math problem that had stood open for the better part of a century. Twelve hours earlier, a mathematician at New York University had posted a statement on his university page saying OpenAI had taken his work.

## The problem

The Navier-Stokes equations describe how fluids move. Air over a wing, water swirling in a river, blood pushing through an artery. Every weather forecast is, underneath, a supercomputer grinding through these equations. Engineers have used them for roughly two centuries. Nobody had ever proved they always make sense.

In 2000 the Clay Mathematics Institute named that gap one of its seven Millennium Prize Problems and attached a million dollars to it. Only one of the seven has been claimed so far, the Poincaré conjecture.<sup>[1]</sup>

OpenAI’s answer was that sometimes the equations do not make sense. Start with a fluid sitting still, apply a smooth force to it, and within a finite stretch of time the velocity at one point runs off to infinity. The core of a vortex shrinks and speeds up at the same time, while the total energy stays finite throughout.<sup>[2]</sup> Real water does not do this. The equations stop tracking reality.

## How it was done

OpenAI says it used an internal model it began training on August 28. The company described it only as far more capable than the publicly available GPT-6 and did not release it.

Roughly 10,000 AI agents ran on that model at once. An agent is an AI program that decides its own next step instead of waiting for a human to assign one. The agents traded millions of messages among themselves and arrived at a proof in 88 hours, about three and a half days. Along the way they produced something like 130 billion tokens of text.<sup>[3]</sup> A token is the smallest chunk an AI model reads and writes, a little less than a word in English. 130 billion tokens is roughly a million novels’ worth of writing, generated, critiqued, thrown out and rewritten inside three and a half days.

The estimated bill runs past $40 million.<sup>[3]</sup> That is forty times the prize. The output was a 166-page paper.

The verification method is what makes this different from previous AI math claims. OpenAI formalized the proof in Lean, a programming language that lets a computer check a mathematical argument line by line. Instead of human referees deciding an argument looks airtight, a machine confirms every step. The formalization took another 17 hours.<sup>[2]</sup> Whatever else is in dispute, the logic holds.

## “Solved” needs an asterisk

The Clay problem comes in four versions. OpenAI settled C and D, the ones where an external force is applied. A and B, which ask whether solutions exist and are unique when the fluid evolves on its own, are untouched.<sup>[4]</sup> The Clay Institute’s page still lists Navier-Stokes as open. Claiming the money requires a peer-reviewed publication followed by a two-year waiting period, and OpenAI has said it does not plan to claim it.<sup>[4]</sup>

So the headline is about half right. One large piece of the problem has fallen. The rest is standing.

## Who got there first

Tristan Buckmaster at NYU and Levent Alpöge at Anthropic had spent close to a year working the same angle. By August 22 they had a result on the Euler equations, the frictionless cousin of Navier-Stokes, verified in Lean, and were writing it up.<sup>[4]</sup> According to Buckmaster’s statement, he first learned that word of their progress had reached OpenAI.

Three days later, on a call, OpenAI’s Sébastien Bubeck told him an internal model had already produced a 100-page proof using the exact approach almost nobody else was pursuing. Buckmaster wrote: “The route to the Clay problem through a smooth force, options c and d in Fefferman’s statement of the problem, is the route Luis and Diego opened and the one Levent and I had quietly chosen to attack. Almost nobody else I know of was working on it.”<sup>[5]</sup> That is not, he argued, a direction you land on in a few days by handing a model the problem statement.

By Buckmaster’s account, Bubeck laid out two options. Buckmaster’s team publishes first and OpenAI publishes the next day. Or Buckmaster writes it up alone, with Alpöge left off because he works at a rival lab (a lab being a company like OpenAI or Anthropic that builds frontier AI models). When Buckmaster said he would go public instead, he says Bubeck answered, “Why would you ruin your career?” and then, “If you don’t want me to be nice, then I don’t have to be nice.”<sup>[5]</sup>

OpenAI denied the core allegation. Its September 8 statement reads: “We (the researchers and the agents) did not see any of their work through any means until they released it publicly, in particular, no specific user data was accessed.”<sup>[5]</sup> The company says its own effort started September 1, prompted by rumors that a Millennium Prize problem had fallen.

Bubeck did concede that the Navier-Stokes solution “did follow a similar method” to Buckmaster and Alpöge’s, developed “over the weekend” after their announcement.<sup>[6]</sup> OpenAI has also said it cannot rule out that de-identified data from the two mathematicians’ use of its products helped train the model.<sup>[4]</sup> That is not strictly a contradiction of the no-user-data line, but the distance between the two statements is what the fight is about.

## What mathematicians said

Charles Fefferman at Princeton was glad the problem fell, and pointed elsewhere for credit, calling Diego Córdoba and Luis Martínez-Zoroa, who built the approach this work rests on, “the heroes of the story.”<sup>[2]</sup> Martínez-Zoroa himself was gracious about it: “I’m very happy for Tristan. It would have been nice to do this ourselves, but I’m very happy for him.”<sup>[2]</sup>

Buckmaster’s review of the AI’s proof: “the most horrendous I have ever read.”<sup>[2]</sup> Correct, and worthless to read.

Terence Tao, not a party to the dispute, has separately warned that labs treating open problems as marketing opportunities risks strip-mining the field. A model that hands over a finished proof without its failed attempts takes away the intermediate insight other mathematicians would normally have gotten from the attempt.<sup>[7]</sup>

## What to do with this

**Think twice before feeding unfinished ideas to a chatbot.** The spine of this story is not fluid dynamics. It is whether work in progress leaked through a product. Whether OpenAI actually saw anything is contested. But pasting an unannounced roadmap, a draft contract, or a product design into a chatbot happens in offices every day. Knowing whether you are on a company account and whether your inputs are excluded from training is worth five minutes.

**Read “AI solved it” headlines for scope.** This one covered two of four versions. Finding the single line that says what was settled and what was not separates the claim from the coverage. That habit generalizes well beyond math.

**Work a machine can grade is what changes first.** This result was possible because Lean checks proofs automatically. Anything where correctness can be judged instantly by a machine can be attacked by throwing ten thousand agents at it and letting them fail cheaply. Code, reconciliation, compliance checks. Work where “good” depends on human judgment moves much more slowly. Figuring out which kind yours is tells you a lot.

## A closing thought

Forty million dollars spent to win a million-dollar prize looks like bad arithmetic, but the prize was never the product. What OpenAI bought was evidence that enough compute (the raw processing power it takes to run AI) collapses a century-old problem in three and a half days. That sells for far more than a million dollars.

What the evidence left mathematicians is a 166-page correct proof, a reader who found nothing in it worth learning, and an argument about who was first. The answer arrived and the understanding did not.

The part that sits worst is the menu Buckmaster describes being handed. Publish first and get buried the next day, or publish alone and drop your collaborator’s name. When the ability to solve hard problems concentrates in one company, that company does not just decide which proofs exist. It decides whose name survives. The answer turned out to be less scarce than the order of arrival.

When a team that can spend a million dollars competes against one that can spend forty, is the winner’s result still a measure of ability?

## Sources

#### Notes

1. Clay Mathematics Institute Millennium Prize Problems and prize rules Quanta Magazine ↩
2. Proof content, 17-hour Lean formalization, remarks from Fefferman, Martínez-Zoroa and Buckmaster Quanta Magazine ↩
3. Roughly 10,000 agents, 88 hours, about 130 billion output tokens, estimated cost above $40 million Latent Space AINews ↩
4. Only versions C and D resolved, no prize claim, August 22 Lean verification, OpenAI’s de-identified data caveat Unite.AI · Wikipedia: Navier–Stokes priority controversy ↩
5. Buckmaster’s statement, quotes attributed to Bubeck, OpenAI’s official response TechCrunch ↩
6. Bubeck acknowledging a “similar method” Scientific American ↩
7. Terence Tao’s warning Fortune ↩
