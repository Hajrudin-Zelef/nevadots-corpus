---
id: collect-261121-veille-ia/claude-anthropic/claude-fable-5-2-ai-agents-rebuild-fermat-s-last-theorem-proof-in-30-hours-starting-with-47-lemmas
title: "Claude Fable 5.2 AI Agents Rebuild Fermat's Last Theorem Proof in 30 Hours, Starting With 47 Lemmas"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["theorem", "anthropic", "agents", "lemmas", "fable", "agent", "claude", "lean"]
source: https://www.techtimes.com/articles/330361/20261110/claude-fable-52-ai-agents-rebuild-fermats-last-theorem-proof-30-hours-starting-47-lemmas.htm
source_anchor: ""
source_lines: [1, 91]
section: "Claude_Anthropic"
---
Anthropic's November 9 launch page says 1,000 Claude Fable 5.2 agents fully formalized Fermat's Last Theorem in the Lean proof assistant in 30 hours with "minimal human intervention," compared with 11 days for its September run. A separate Anthropic engineering post describes the experiment more narrowly: a 28.7-hour "toy experiment" on an early Fable 5.2 snapshot, in which the agents received 60 target statements, a one-sentence route sketch, the relevant definitions and 47 lemmas already proved in Anthropic's earlier formalization. Anyone comparing the two timelines, pricing a similar agent swarm or deciding how much weight a self-reported milestone deserves needs those inputs, because the 11-day and 30-hour runs did not start from the same place.

This is a verification milestone, not a new proof. Andrew Wiles settled the theorem in 1995, and the Lean code encodes his argument so a computer can check every step. Anthropic's first formalization, published September 4 and produced by an unreleased research model, took 11 days, about 13 million lines of Lean and roughly six billion output tokens.

### Did Fable 5.2 Agents Start From Scratch? 60 Targets, 47 Lemmas and One Route Sketch Came First

The engineering post says the team had one lead agent and 999 helpers running an early version of Fable 5.2. Each agent worked in a sandbox with no network access beyond a gateway to the Claude API, a shell and file editor, Lean and Mathlib, its own Git branch with merge-request rights and a shared Postgres database. The prompt package listed 60 statements to prove (the final theorem plus 59 intermediate milestones), a one-sentence sketch of the classical route, the definitions those statements use and 47 proved lemmas from the earlier formalization. Anthropic says it offered no other guidance on how to prove the theorem or organize the team.

The launch page frames the same run differently. It says the agents started from minimal prompts, built their own coordination methods, worked without mathematical reference material and tracked progress against a list of intermediate theorem statements. Whether Mathlib, the definitions and the 47 lemmas count as references is a definitional call the launch page does not spell out. The engineering post also does not say where the lemmas came from beyond Anthropic's earlier formalization. That September work built on Buzzard's Imperial College London project, and Anthropic's repository lists 106 files with upstream material from that project and a second one, flt-regular.

The September comparison point is different. According to Anthropic's timeline of that run, the human team wrote no mathematics and no Lean beyond the one-line statement of the goal theorem, though it offered encouragement. The new run was seeded with milestones and lemmas but, per the engineering post, no researcher steered it; the harness sent the lead agent 19 automatic time reminders, each encouraging it to keep going.

### How Do 1,000 Agents Coordinate? Five SQL Tables, Per-Agent Virtual Machines and Merge Job

September's run used Prove2Me, a platform that keeps a shared graph linking each theorem statement to its sub-statements. The Fable 5.2 experiment replaced that graph with five SQL tables in a shared Postgres database, named announce, assign, notes, subtasks and landed, and gave every agent its own Git branch. A separate job merged branches into the main line.

The infrastructure choice carries a scaling lesson. Anthropic gave each agent a lightweight virtual machine because a single shared machine hit limits at about 100 agents. The lead agent split the problem and assigned parts, and the team's task tree grew to 2,418 tasks up to 10 levels deep.

Coordination at that scale produced a recognizable failure mode: premature pessimism. Early on, helpers judged 47 of the 60 statements infeasible (a different 47 from the lemma count), yet the team went on to prove most of them. The captain for the Langlands-Tunnell step estimated under a 1% chance of finishing in time, yet the lead still moved about 750 helpers onto it; that step was proved at hour 28.4 and the final theorem at hour 28.7. Activity stopped near hour 18, and the run was restarted from its last save with 12 more hours.

Anthropic's authors say the result does not remove the need for domain-specific harnesses such as Prove2Me, which may still speed up Lean compilation and help agents reuse human results, and that human encouragement could keep agents from stopping too early. The post also reports that only an action-based safety monitor ran during the experiment, that none of more than one million tool calls met the bar for blocking, and that one root-access check came close.

### Single Kernel Check Backs 30-Hour Claim While September Run Added Comparator and Buzzard Review

The engineering post says the lead agent reported the final theorem on the main branch and kernel-checked by the official checker; in the post we reviewed, we found no mention of a second kernel or a comparator tool. The September package was heavier: a comparator confirmed the statement matches Mathlib's own statement of Fermat's Last Theorem, the proof relied only on Lean's three standard axioms, and Kevin Buzzard reviewed the result. In the sources we reviewed on the evening of November 9, we found no independent review of the new proof, and Anthropic says external organizations including the UK AI Security Institute, Frontier Design and METR did not review the launch post or system card.

The gap matters because Lean certifies only what its kernel can check. A purported disproof of the Collatz conjecture was announced as verified in Lean in 2026, then found to rely on a Lean bug and became invalid once the bug was fixed. A comparator guards a different risk: that the theorem proved is not the theorem intended.

The model comparison carries its own limits. With the same 1,000-agent setup, Opus 5.5 completed 26% in 18 hours and barely progressed by hour 30, and a chart caption on the launch page, as described by The Decoder, shows it plateauing at 0.27. The caption says the chart shows a single run per model and that both teams resumed from their last save with 12 more hours at hour 18, so the chart carries no variance estimate. The engineering post says the Fable 5.2 run used one-third as many output tokens as the September run and produced a proof one-seventh as long; neither source we reviewed gives the new proof's line count.

### Output Tokens Alone Would Run Roughly $80,000 at Fable 5.2 List Price

Anthropic has not published the cost of the run. A rough floor comes from two published figures: the September run used about six billion output tokens, and the Fable 5.2 run used one-third as many, or about two billion. Fable 5.2 lists at $8 per million input tokens and $40 per million output tokens, with an optional fast mode at $16 and $80 that generates tokens up to 2.5 times faster. At the standard rate, two billion output tokens come to about $80,000 by TechTimes arithmetic, before input, cache and virtual-machine costs. Because the launch page counts action tokens while the engineering post counts output tokens, treat the figure as an order-of-magnitude estimate, not a bill. The run spanned up to 28,700 agent-hours (1,000 agents over 28.7 hours, if every agent ran throughout), another TechTimes calculation.

Fable 5.2 is available on all platforms, including Amazon Web Services, Google Cloud and Microsoft Azure, as claude-fable-5-2. Mythos 5.2 stays restricted to vetted organizations through Anthropic's Cyber and Life Sciences Verification Programs.

### Physics Preprints Label Themselves Primarily AI-Generated, With Physicist Checking Results

Anthropic says Fable 5.2 solved two open problems in theoretical physics that rely on holographic duality, the idea that certain gravitational universes can be described by a quantum theory with no gravity. Both results were posted November 7 on Hexagon, a preprint server, with no DOI assigned and no peer review indicated. Both pages carry the label "Primarily AI-generated text" and a note that human understanding covers only some parts.

The first paper, "The Hawking-Page transition at weak coupling," runs 42 pages. In 1983, Hawking and Page showed that black holes form abruptly as certain large universes heat up, and Anthropic's page says a test for very small universes was laid out in 2003 but was too laborious to finish. The closest 2003 match we found is a paper by Ofer Aharony, Joseph Marsano, Shiraz Minwalla, Kyriakos Papadodimas and Mark Van Raamsdonk on weakly coupled gauge theories, which found either a single first-order transition below the Hagedorn temperature or two continuous transitions; Anthropic's page does not name its source. The new paper asks whether the transition stays first order in N=4 super Yang-Mills theory at weak coupling, where there is no classical geometry. It reports a quartic coefficient of b = −1.48×10⁻³ λ², with λ the 't Hooft coupling, which makes the transition first order and slightly below the Hagedorn temperature, and it reproduces the known two-loop Hagedorn temperature. The paper presents this as black-hole-like structure appearing in ordinary weak-coupling perturbation theory.

The second paper, "The spinless modular bootstrap at large central charge," runs 131 pages. The modular bootstrap bounds the dimension of the lightest non-vacuum primary in a two-dimensional conformal field theory using only modular S invariance, and at large central charge c the bound takes the form c divided by Λ. Earlier work had pinned the optimal slope only between about 8.503 and 12; the paper reports the exact optimum as 12π² divided by (π² plus 4 arcsinh² 1), about 9.1266. It gives an explicit functional with a c^(1/3) correction and supports a conjectured coefficient for that term with numerics up to c = 2048. Anthropic describes the technique as one physicists have used for more than 15 years to rule out candidate theories of quantum gravity in a simplified universe.

Provenance statements on the pages describe who checked what. For the first paper, an internal Claude research model prompted by a physicist obtained the result, the physicist checked it against several known special cases, and a separate Claude computation using a different method, not included in the paper, reportedly agrees. For the second, an internal Claude research model first obtained the main result autonomously, Claude Opus 5.5 later reached it independently, and Claude wrote the paper after brief guidance from a physicist who checked the main results. Neither page names the internal research model, while the launch page credits Fable 5.2. One physicist's review is not peer review, and the physics community has yet to weigh in.

### Quantum ESPRESSO Gain: 36x Geometric Mean Across 13 Phonon Cases, From 12x to 108x

Akash Ramdas, co-founder of Discovered Materials, supplied the Quantum ESPRESSO claim. His company builds AI agents that discover materials for semiconductor chips. Ramdas says Fable 5.2 made the open-source suite's phonon code about 36 times faster on average than stock on the same GPU, and that the 13 benchmark cases fell from 3.3 hours to under four minutes. He also says the gain beats Fable 5.1's speedup by 3 to 4 times, a comparison that belongs to Ramdas, not to Anthropic.

Discovered Materials' own write-up resolves what the launch page leaves open. The 36x is a geometric mean across 13 cases relative to stock Quantum ESPRESSO on the same GPU, and the per-case speedups range from about 12x to about 108x; against a stock CPU run, the geometric mean is 14x across the 12 cases that have a CPU reference. The write-up does not define the baseline for the 3.3-hour figure. Summing the stock-GPU and optimized columns of its Table 2 benchmark results gives about 11,784 and 222 in the table's unstated units, which would be 3.3 hours and 3.7 minutes if the units are seconds. That is a suite-level reduction near 53x, by TechTimes arithmetic, larger than 36x because the slowest case, a 54-atom silicon calculation that fell from 4,380 to 57.8, accounts for about 37% of the stock total.

Choice of average matters. A geometric mean treats every case as equal, while a total weights the long jobs; the Wikipedia entry on the geometric mean notes that using it to aggregate performance has been questioned and suggests weighting programs and using other means, such as the harmonic mean for speedups. Readers should ask which case type matches their own workload.

On correctness, the write-up says the team used standard benchmark suites plus hidden test sets covering representative systems to avoid overfitting. It describes remaining mismatches as within experimental measurement error but gives no numerical tolerances, and the benchmarks are the authors' own with no independent review. The single-GPU model is not named, and the hybrid-functional results ran on four H100 GPUs. The patch is public as the dm_qe repository on GitHub, so other labs can test it. Ramdas says phonon calculations are costly enough that labs often skip stability checks when screening new materials, which is where a verified speedup would pay off.

### XTX Markets Praise Comes From Longtime AI-for-Math Funder, While IMC Cites Three Best Scores

Anthropic's page carries statements from testers at quantitative-finance firms. XTX Markets founder Alex Gerko said Fable 5.2 delivered strong gains over Fable 5.1, especially in setups with multiple coordinating agents. IMC principal AI engineer Marquis Wong said it set new best scores on three of the four quant research problems the firm has finished. These are customer reports, not independent benchmarks.

Context helps weigh them. XTX Markets launched a $10 million AI Mathematical Olympiad Prize and gave $9.2 million to an AI for Math Fund in December 2024, according to Bloomberg. It has since closed the prize and says it has committed more than $50 million to open-source AI-for-math projects. We found no public profile for Wong beyond his title on Anthropic's page, and, in our searches, no technology-policy record for any of the three named testers.

### RoboDojo Sim Score Hits 64.9% in Anthropic-Run Simulation; Independent Index Shows Cost

On RoboDojo Sim, a benchmark in which a simulated two-arm robot is controlled from camera images, Fable 5.2 at Max effort scored 64.9%. Opus 5.5 at Max scored 39.4%, Gemini 4 Argon at High 34.2%, GPT-6 Astra at Max 29.8% and Fable 5.1 at Max 27.6%, which puts Fable 5.2 at more than twice the last two. Anthropic ran the evaluation internally, the chart lists different effort levels across models, and the results are for simulation, not physical hardware.

On price and speed, independent figures arrived the same day. According to The Decoder's reading of Artificial Analysis, Fable 5.2 at max effort scores 65 on the Artificial Analysis Intelligence Index, first among the models it tracks, at about $26.50 per index task, roughly 2.6 times Opus 5.5's $10; at xhigh effort the model scores 62. Anthropic itself advises starting with Opus 5.5 for most coding and knowledge tasks, and says Fable 5.2 at High effort scored 45.3% on FrontierCode 2.0, about level with Opus 5.5's best score of 45.5%, for roughly half the cost per task.

### Three Showcase Results Carry Three Verification Tiers: Kernel Check, Physicist Review, Customer Benchmark

Anthropic's launch rests on three kinds of evidence with different strengths. The Fermat run has the strongest form of checking, a Lean kernel, but its single-checker package is lighter than September's, no outside reviewer was reported as of November 9 and the starting inputs were substantial. The physics results rest on one physicist's review of AI-written preprints that have not been refereed. The Quantum ESPRESSO figure rests on a customer's own benchmark suite, where the headline average hides a per-case spread from about 12x to about 108x. Nearly every performance figure about Fable 5.2 here comes from Anthropic or its testers; the Artificial Analysis index is the only independent measurement we found.

For readers, the practical steps differ by result. Treat the 30-hour Fermat time as a best case with milestones and lemmas supplied, and wait for outside review of the Lean artifact. Wait for peer review before citing the physics. Test the public Quantum ESPRESSO patch on systems like your own before budgeting around 36x. And price any large agent swarm from token counts, not from headline hours.

## Frequently Asked Questions

### Did Fable 5.2 prove Fermat's Last Theorem?

No. Andrew Wiles proved it in 1995, and Fable 5.2's agents translated that argument into Lean so a computer can check each step; the work is verification, not discovery.

### Was the 30-hour Fermat run started from scratch?

Not entirely. Anthropic's engineering post says the agents received 60 target statements, a one-sentence route sketch, definitions and 47 lemmas from the earlier formalization, with Mathlib in their sandbox, and no researcher steered the run.

### Does a 36x Quantum ESPRESSO speedup apply to every calculation?

No. The 36x is a geometric mean across 13 cases, with individual speedups from about 12x to about 108x, and the benchmarks and validation are the authors' own.

### Who has checked the new black-hole physics preprints?

One physicist checked the results, according to provenance statements on the preprints, which are labeled as primarily AI-generated and show no peer review.
