---
id: collect-261121-veille-ia/divers-tech/did-ai-solve-the-navier-stokes-problem-what-was-proved-and-why-it-isn-t-over
title: "Did AI Solve The Navier-Stokes Problem? What Was Proved, And Why It Isn't Over"
domain: divers-tech
role: reference
task: article
actors: []
dates: ["2026-09-08"]
keywords: ["equations", "fluid", "coffee", "navier-stokes", "problem", "proved", "outside", "time"]
source: https://dev.scienceabc.com/pure-sciences/navier-stokes-openai-singularity-what-was-proved-what-is-open
source_anchor: ""
source_lines: [1, 97]
section: "Divers_Tech"
---
## Table of Contents (click to expand)

In September 2026, OpenAI said a team of about 10,000 AI agents had proved that the Navier-Stokes equations, which describe how liquids and gases flow, can blow up: a fluid at rest, pushed by a smooth outside force, reaches speeds that grow without limit in a finite time. That matches one of the four statements the $1 million Millennium Prize accepts, and the Clay Mathematics Institute says the problem has “apparently been settled,” pending its own slow check. Some experts call that outside force a loophole, because the question most of them had in mind, whether a fluid left alone can blow up, is still open.

Stir a cup of coffee, then lift the spoon out. The swirl spins, slows and dies, every single time. You have never seen a cup of coffee spin itself faster and faster until it tore a hole in the kitchen.

Physics has a set of equations for that swirl, worked out separately by Claude-Louis Navier and George Stokes in the early 1800s. They run weather forecasts and shape jet wings. Yet nobody has ever proved that the equations agree with your coffee: could the math say a swirl speeds up without limit? There is a $1 million prize for the answer.

On 8 September 2026, OpenAI announced that its AI had found that answer. Three days later, the keepers of the prize said the problem had “apparently been settled.” Yet the question most experts had in mind is still open, so both things are true at once. To see how, we need a little science first.

## What Are The Navier-Stokes Equations, In Simple Terms?

Back in the cup, shrink your view to one small blob of coffee. Why does it move the way it does?

Three things push on that blob. The coffee around it presses on it, which is pressure, and drags on it, which is friction (a fluid’s friction is called viscosity, and honey has far more than water). The third push comes from outside the fluid: gravity, or your spoon.

That is all the equations say. The prize’s official problem statement puts it plainly: the main equation “is just Newton’s law f = ma for a fluid element,” the same law that governs a thrown ball. In words:

**acceleration of the blob = friction − pressure push + outside force**

The picture below shows the same thing in symbols. You need not read them, but notice the little *f* on the far right: the outside force, on which the whole story turns.

The same few lines cover the calm stream from a tap and the churning wake of a jet, or in the prize’s words, “both the breeze and the turbulence.” That is a lot of work for one law you met in high school.

Nobody has solved them in general, though. They are “too difficult to solve analytically,” so engineers cheat, with style. They chop the air around a wing into millions of tiny cells and let a computer work out close-enough answers, one small step at a time. That is how weather forecasting works, and how engineers tame the turbulence that rattles your flight. Nobody can solve these equations, and we fly on them anyway.

The $1 million question is more basic: do the equations always have a smooth answer to give, or can they, in some strange case, give up?

## What Does It Mean For A Fluid To “Blow Up”?

Experts call a flow **smooth** when nothing in it jumps. Speed changes gently from place to place, and it stays finite everywhere. Your coffee is smooth in this sense, even mid-stir.

The opposite is called **blow-up**, and its formal name is a singularity. OpenAI describes it as “speeds in the fluid growing without bound within a finite amount of time.” Picture one point in the cup where the flow gets faster and faster, past every speed you can name, before a set moment. Oddly, the total energy in the cup stays finite the whole time.

Real coffee cannot do this. The equations assume you can zoom in on a fluid forever, but zoom far enough and you hit molecules. So a blow-up is a failure of the model, not of the mug. Your kitchen is safe.

## Why Has No One Solved Navier-Stokes?

Because two effects fight inside the equations, and for about 90 years nobody could call the winner.

On one side is friction, the peacemaker. It smooths out sharp changes, which is why your swirl dies. Take friction away and you get Euler’s wilder equations.

On the other side is the way a swirl can feed itself. In a flat, two-dimensional flow, friction wins, and that has long been known, but the prize text warns that the flat case “gives no hint about the three-dimensional case.”

In three dimensions, a spinning tube of fluid can stretch, thin out and spin faster. Big swirls hand their energy to smaller ones, which is the heart of the turbulence behind a gust of wind. The worry was always a runaway: energy rushing into smaller and smaller swirls, faster than friction can calm it.

The best tool experts have is a cap on the fluid’s total energy, and it is not enough. Tools like that, the mathematician Terence Tao explained years ago, are “much weaker at controlling fine-scale behaviour.” They see the whole cup, but go blind right where the trouble would start.

For most of those years, experts bet on friction. “Ten years ago, nobody believed there was a singularity for Navier-Stokes,” says the mathematician Diego Córdoba.

## What Does The $1 Million Navier-Stokes Problem Ask?

In 2000, the Clay Mathematics Institute, the foundation that keeps the prize, picked seven famous unsolved problems and put $1 million on each. Only one had fallen before this year.

The official Navier-Stokes entry, written by the mathematician Charles Fefferman, offers four statements, and a proof of any one wins.

**Statements A and B** say the flow always stays smooth, whatever smooth start you give it, for a fluid that fills all of space (A) or sits in a box that repeats forever in every direction (B).

**Statements C and D** say the opposite: somewhere there is a smooth start that ends in a blow-up, in the same two settings.

The fine print is in the force. In A and B, the text says “Take f(x, t) to be identically zero,” so there is no outside force and the fluid is left alone. But C and D ask for a smooth start “and a smooth f(x, t),” so an outside force is allowed, as long as it is smooth.

So the two halves of the prize do not mirror each other. One half takes the spoon away, and the other half hands it back.

## Did ChatGPT Solve Navier-Stokes?

This was not the ChatGPT on your phone. OpenAI says it used an unreleased internal model and ran a crowd of AI programs, called agents, that could talk to each other and run code.

The numbers in OpenAI’s post are large. The group that cracked the problem had “on the order of 10,000 concurrent agents.” They reached their proof “about 88 hours after the first agents were launched,” having sent each other millions of messages, which is some group chat.

The proof starts with a fluid at rest and applies a smooth outside force. The fluid’s energy stays finite, and even so, in a finite time, the flow blows up, in a swirl that spirals inward and gets stretched long and thin, “like spaghetti.” OpenAI says this settles statement C, and D as well.

A second AI model then rewrote the proof in a language a machine can check line by line. Someone still has to confirm the machine checked the statement the prize asks for, and other mathematicians have not yet gone through the proof themselves.

OpenAI says it will not claim the prize, after one of its own put the computing bill at “several million dollars.” So it spent several million to not collect one. Nobody said the machines were good with money.

## Why Do Some Mathematicians Call The Outside Force A Loophole?

Most experts, it turns out, write the problem without that little *f*. They assumed the force did not matter, and that any blow-up would look the same with or without it.

Córdoba and his former student Luis Martínez-Zoroa saw an opening: could the force alone make the equations blow up? Their method stacks up endless layers of smooth flows, an “infinite cascade,” and it had already blown up Euler’s friction-free equations, though with a messy force. The prize demands a smooth one, and that was the gap the AI work closed. Córdoba, for the record, likes to joke: “I don’t use AI: I have Luis.”

The proof’s fluid starts at rest, so all of its motion comes from the outside force. In coffee terms, the prize allows a spoon, and this is a brilliant spoon: it stirs in a pattern that stays smooth even as the coffee under it blows up. The blow-up, OpenAI stresses, comes from the fluid’s own motion, not from “putting in an infinite force by hand.”

That is fair, but not the question most experts had in mind. They want to know about coffee with no spoon: can a fluid left alone blow up on its own? The problem as written is solved. The problem as many experts picture it is not, which is why some may say the win has “only been achieved through a loophole.”

Nobody knows whether the proof can be stretched to the no-force case, but there are hints. OpenAI says its agents also blew up Euler’s friction-free equations with no outside force at all, and that claim awaits checking too. Tao wrote on his blog, the day before OpenAI’s news, that “it should even be possible to do without the forcing term.” That is a forecast, not a proof.

Solved or not, then? It depends which problem you mean, and there are three.

**The problem as written** now has a machine-checked proof of statements C and D. The Clay Mathematics Institute says it has “apparently been settled,” and that word is doing a lot of work: its own check is “deliberately unhurried,” and no prize has been awarded.

**The problem as experts meant it** asks whether a fluid with friction, left alone, can blow up. Nobody has shown that it can, and nobody has shown that it cannot. Statements A and B remain unproved.

**The problem in your cup** has not changed at all. Planes fly and forecasts run on the same equations as last month. Even so, the result matters, because friction was meant to be the peacemaker, and the math now says it can lose.

So stir your coffee and watch the swirl die, as it always will. The mystery was never the mug, but whether a law as plain as F = ma could ever run out of answers. For a fluid left alone, nobody knows yet. Hand it the right spoon, though, and a proof that still awaits checking says it can.
