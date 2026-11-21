---
id: collect-261121-veille-ia/divers-tech/seven-million-dollar-questions
title: "Seven Million-Dollar Questions"
domain: divers-tech
role: reference
task: article
actors: []
dates: ["2026-10-28"]
keywords: ["seven", "open", "about", "clay", "water", "each", "money", "problem"]
source: https://enlight.blog/millennium-prize-problems/
source_anchor: ""
source_lines: [1, 222]
section: "Divers_Tech"
---
We hear within us the perpetual call: There is the problem. Seek its solution. You can find it by pure reason, for in mathematics there is no *ignorabimus*.

For 88 hours at the start of September, about ten thousand copies of one AI system worked on a single question about water. By their makerâs count they passed 2.7 million messages between them, and on Saturday 5 September they stopped. Three days later OpenAI announced what they had found: a smooth push which, applied to a fluid at rest, makes it blow up. Somewhere, after a finite time, its speed becomes infinite. The proof runs to about 166 pages and comes with a version written for Lean, a program that checks proofs step by step.

If it holds up, it settles one of the seven Millennium Prize Problems, each worth a million dollars. OpenAI says it wonât claim the money. That makes it the second solver to say no. The first was a Russian mathematician who, in 2010, turned down the only Millennium Prize ever awarded.

Twenty-six years in, the scoreboard reads: one problem solved and the money refused, one âapparently settledâ (the prize-giverâs careful phrase) by a machine, and five open, three of which have moved since August. This post covers all seven: what each asks in plain words, how we got here, and the question I care about most. If they all fell tomorrow, what would actually change for the rest of us?

## Seven Doors, One Price

In August 1900 David Hilbert stood up at the International Congress of Mathematicians in Paris and set out the problems he thought the new century should solve. The printed list had 23, with no money attached, and most have since been settled. The eighth included a guess that Bernhard Riemann had made in 1859 about prime numbers. That one is still open.

A century later, on 24 May 2000, the Clay Mathematics Institute did the same thing with a budget. Clay is a private foundation set up two years earlier by the businessman Landon Clay and his wife, Lavinia. At a meeting in the CollÃ¨ge de France it named seven problems and put $1 million on each. Riemannâs guess was the only one to make both lists.

The rules are the interesting part. Nobody can send Clay a proof. A solution has to appear in a refereed outlet, survive at least two years, and then win âgeneral acceptance in the global mathematics communityâ. For P vs NP and NavierâStokes a proof either way counts; for the rest, a counterexample wins only if it effectively settles the question. The money can be split, and Clay says it will pay special attention to earlier work that made a solution possible. Keep that clause in mind.

| Problem | First asked | Status, 28 Oct 2026 | 
|---|---|---|
| NavierâStokesCan a smooth flow of fluid blow up? | 1822 Â· 1845 | claimed, forced case | 
| Riemann hypothesisDo the zeta zeros all sit on one line? | 1859 | open | 
| PoincarÃ© conjectureIs every loop-shrinking 3D space a sphere? | 1904 | proved, 2003 | 
| Hodge conjectureAre special shapes built from algebraic pieces? | 1950 | open | 
| YangâMills mass gapWhy does a massless theory make mass? | 1954 | open | 
| P vs NPIs everything easy to check also easy to find? | 1956 Â· 1971 | open | 
| Birch and Swinnerton-DyerCan point counts predict a curveâs solutions? | 1960s | open | 

Iâll take them in the order they were first asked, which happens to start with the one in the news.

## Will the River Tear?

Every time you stir your tea, it obeys equations written down in 1822 by Claude-Louis Navier, a French engineer, and put on a firmer footing in 1845 by George Gabriel Stokes in Cambridge. Theyâre Newtonâs second law for a fluid. Each little parcel of water speeds up or slows down because of pressure from its neighbours, the friction of its own stickiness, and any outside force.

Pull the plug in a bath and watch the whirl over the plughole: the closer the water gets to the centre, the faster it spins. Now imagine a whirl that keeps tightening by itself until, at one point and at one moment, the water is spinning infinitely fast. Mathematicians call that a blowup. Nobody has proved that the equations rule it out, and until this year nobody had found a way to make it happen.

Engineers solve these equations on computers every day. What nobody has managed is to prove that they always behave: that a smooth flow, started with a finite amount of energy, stays smooth forever instead of blowing up.

The equations are what mathematicians call supercritical. Energy is the main thing we can control, and it gets weaker exactly at the small scales where a blowup would happen. Itâs like trying to rule out a fire in one room by watching the average temperature of the whole building.

Charles Fefferman, who wrote Clayâs official statement, offered four ways to win. âAll of spaceâ means water stretching out forever in every direction. A âwrap-around boxâ works like the screen in the old *Asteroids* arcade game: whatever flows out of one side comes straight back in through the opposite side. C and D also allow a push from outside, like a spoon that keeps stirring, as long as the push is perfectly smooth.

Humans found that door. In 2023 Diego CÃ³rdoba and Luis MartÃnez-Zoroa forced a blowup in a stickiness-free cousin of the equations with a cascade of swirls, though their push was too rough for Clayâs terms. This August, Tristan Buckmaster and Levent AlpÃ¶ge got blowups from a smooth push in three related fluid models and had the proofs checked in Lean. AlpÃ¶ge works at Anthropic, the company behind the Claude models, but did this work in a personal capacity. OpenAIâs run started on 1 September.

What happened next is disputed. Buckmaster alleges that OpenAI learned of his work early and proposed an authorship deal that left AlpÃ¶ge out; OpenAIâs SÃ©bastien Bubeck denies using their prompts or proofs and calls the account âfalse and inflammatoryâ.

- 15 Aug 2026Buckmaster and AlpÃ¶ge get forced blowups in three fluid models, Lean-checked a week later.
- 1â5 SeptOpenAIâs 88-hour run: about 10,000 agents, by the companyâs figures.
- 7 SeptBuckmaster posts three papers and a statement late in the day.
- 8 SeptOpenAI announces forced blowup for NavierâStokes, statements C and D, and says it wonât claim the prize.
- 11 SeptClay: the problem âhas apparently been settledâ.
- 17 SeptConstantin, Ignatova and Vicol show the method canât get rid of the force.
- 6 OctA Kingâs College London and Cambridge preprint finds that the Lean proof and the written proof donât state the same thing.

Fefferman told *Quanta*, âI was thrilled that the problem was solved,â and named CÃ³rdoba and MartÃnez-Zoroa as the heroes of the story. Luis Silvestre of the University of Chicago put the other side in one line to *Scientific American*: âThe most important problem is unsolved.â He means A and B, water left to itself, which is what a physicist wants to know. Peter Constantin, Mihaela Ignatova and Vlad Vicol showed that OpenAIâs method canât answer that, because its spoon canât be taken out.

Clay calls its process âdeliberately unhurriedâ, and its page for the problem reads âActiveâ, where the pages for the five open problems read âUnsolvedâ. Under the rules no prize can be paid until two years after a refereed publication, so late 2028 at the earliest, and the clause about earlier work will matter.

None of this will change your weather forecast. Forecasts fail after a week or two because the atmosphere is chaotic: tiny errors in todayâs measurements grow until they swamp the prediction, the effect PoincarÃ© first glimpsed in the three-body problem. A 2019 study put the hard limit for midlatitude weather at about two weeks, whatever the maths says. What a proof of A or B would tell us is whether the equations can be trusted all the way down, or whether at tiny scales they predict something impossible and real water must do something else.

## A Very Likely Guess

In 1859 the Berlin Academy made Bernhard Riemann, then 32, a corresponding member, and he said thank you the way mathematicians do, with a paper. Itâs six handwritten pages long and the only thing he ever published on prime numbers.

Shy and often ill, but wildly original. His 1854 lecture on curved space became the geometry of Einsteinâs relativity, and of time travel. He died of tuberculosis in Italy at 39.

Primes are the numbers you canât get by multiplying smaller whole numbers together: 2, 3, 5, 7, 11, 13 and on forever. They thin out as you go up (25 below 100, 168 below 1,000), but in no obvious pattern. Riemann showed that the gap between how many primes we expect and how many we find is made of waves, the way a chord is made of notes. The Riemann hypothesis says none of the notes is out of tune: they all grow at the same, slowest possible rate, so no single wave can drown out the rest and the primes never stray far from the expected count.

Gauss had spotted a rule of thumb: around any large number *x*, roughly one number in every ln *x* is prime. (ln is the natural logarithm; near 1,000 that means about one in 7, near a million about one in 14.) Adding up those chances gives his estimate, li(*x*), and itâs remarkably good. There are 78,498 primes below a million, and li predicts about 78,628. Riemann found an exact formula for the difference, using a function Euler had studied a century earlier: Î¶(*s*), or zeta. Euler had shown, for instance, that 1 + 1/4 + 1/9 + 1/16 + â¯ adds up to exactly ÏÂ²/6, one of those places where Ï turns up with no circle in sight.

Riemannâs leap was to feed zeta complex numbers. Picture those as points on a map, each with an eastâwest position (its âreal partâ) and a northâsouth one (its âheightâ). At certain points on the map zeta comes out as exactly zero, and each of those zeros is one note in the chord.

<sup>s</sup>+ 1/3

<sup>s</sup>+ 1/4

<sup>s</sup>+ â¯

Î¶(s) = 0 and 0 < Re(s) < 1 â Re(s) = 1/2

A zeroâs eastâwest position sets how fast its wave grows. RH says every important zero sits on the same northâsouth line, at position 1/2, which keeps all the waves as quiet as they can be. Riemann worked out the first few zeros by hand, found them on that line, and wrote the most famous understatement in mathematics:

â¦und es ist sehr wahrscheinlich, dass alle Wurzeln reell sind. Hiervon wÃ¤re allerdings ein strenger Beweis zu wÃ¼nschen; ich habe indess die Aufsuchung desselben nach einigen flÃ¼chtigen vergeblichen Versuchen vorlÃ¤ufig bei Seite gelassenâ¦â¦and it is very likely that all the roots are real. A rigorous proof of this would certainly be desirable; but after a few fleeting, vain attempts I have put the search for one aside for the time beingâ¦Bernhard Riemann, 1859 Â· Enlight translation Â· in his notation, âreal rootsâ are zeros on the line

In that chart li(*x*) always overcounts, and computers have confirmed it does for every *x* up to 10<sup>19</sup>. Yet J. E. Littlewood proved in 1914 that somewhere further on it must undercount, and switch back and forth infinitely often. The first switch is known to come before about 1.4 Ã 10<sup>316</sup>, a number with 317 digits. In number theory, âtrue as far as weâve lookedâ can be a very long way from true.

Riemann died seven years later. His housekeeper is said to have burned some of his notes, but in what survived Carl Ludwig Siegel found, in 1932, a method for computing zeros that computers still use. David Platt and Tim Trudgian have now checked 12,363,153,437,138 of them, every one on the line. Proofs moved more slowly: by 2020 the share of zeros proved to be on the line had crawled up to 41.7%.

This year it jumped. In August Anthropic announced that a research version of its Claude model had proved that at least two-thirds of the zeros lie on the line and are simple (no zero counted twice). Levent AlpÃ¶ge and Ralph Furman checked it and posted it with a Lean proof; Youness Lamzouri soon found a simpler route. Then on 6 October OpenAI released a proof of a âquasi-Riemann hypothesisâ: no zeros anywhere east of 7/8 on the map (and so, by a symmetry, none west of 1/8), for Î¶ and for a whole family of its cousins. Experts largely think itâs right, and Maksym RadziwiÅÅ of NYU told *Quanta* that âthe vast majority of consequences of RH already follow from quasi-RH.â Neither result has been refereed, and neither is RH. A disclosure, since it matters here: Anthropic makes Claude, and AlpÃ¶ge works there.

If RH were proved tomorrow, thousands of published theorems that begin âassuming RHâ would become plain theorems, and weâd know for certain that the primes are spread as evenly as they can be. Your bank wouldnât notice. RSA encryption depends on factoring big numbers being hard, and RH gives no way to factor anything; people already behave as if itâs true. Even fast primality testing no longer needs it: since 2002 there has been a test that relies on nothing.

In *Numb3rs* (âPrime Suspectâ, 2005), a mathematician who says heâs close to proving RH has his daughter kidnapped by people who think his work will crack encryption. In *Gifted* (2017), a child prodigyâs late mother turns out to have solved NavierâStokes and left orders not to publish until her own mother died.

## The Man Who Said No

Henri PoincarÃ©, who more or less invented modern topology, asked his question at the end of a long paper in 1904 and then walked away from it. The paperâs last sentence reads: âMais cette question nous entraÃ®nerait trop loin.â But this question would take us too far.

The surface of the Earth is finite, yet it has no edge: you can walk forever without falling off. Lay a loop of rope anywhere on it and you can always pull the loop tight to a single point. On the surface of a doughnut you canât, if the rope goes round the hole. PoincarÃ© asked about the next dimension up. If a finite, edgeless three-dimensional space lets every loop shrink like that, must it be a â3-sphereâ, the three-dimensional version of a ballâs surface? The loop on the sphere at the top of this page is that rope, shrinking away.

<sub>1</sub>(M) = 1 â M â SÂ³

<sub>1</sub>(M) = 1: every loop shrinks to a point Â· â SÂ³: the same shape as a 3-sphere

Oddly, the same question in higher dimensions fell first, in 1961 and 1982. Three held out. That same year Richard Hamilton suggested letting a shape smooth itself out the way heat spreads through a metal bar, evening out its bumps until itâs round, an idea called Ricci flow. The trouble was that a shape like a dumbbell could pinch at its thin waist, and nobody knew how to carry on past a pinch.

Between November 2002 and July 2003, a researcher in St Petersburg posted three papers to arXiv, the free preprint server, and never sent them to a journal. In them he cut each pinch out surgically, capped the cut ends, restarted the flow, and showed that it ends in pieces whose shapes are all known. That proved PoincarÃ©âs conjecture and a much bigger one of William Thurstonâs, which amounts to a complete catalogue of three-dimensional spaces.

A perfect score at the 1982 International Mathematical Olympiad, a celebrated proof in 1994, then years of quiet before the three papers. He left the Steklov Institute in 2005 and has kept away from public life since.

Three teams spent the next three years checking the details, and in 2006 *Science* named the proof its Breakthrough of the Year, the first time it had picked a piece of mathematics. That August, Perelman declined the Fields Medal. âI donât want to be on display like an animal in a zoo,â he reportedly said.

On 18 March 2010 Clay awarded him the first Millennium Prize. On 1 July he turned it down, and the Interfax news agency reported his reason: he considered Hamiltonâs contribution no less than his own. Clay put the money towards the PoincarÃ© Chair at the Institut Henri PoincarÃ© in Paris, which for five years paid for young mathematicians to spend time there. I find that the most graceful outcome of the whole prize scheme so far.

The proof made Ricci flow a standard tool. It didnât tell us the shape of the universe, a myth that still does the rounds; thatâs a question for telescopes.

## Shapes Made of Equations

The Hodge conjecture is the hardest of the seven to put into plain words, so treat this as an honest approximation. William Hodge put it forward at the 1950 International Congress, held at Harvard.

Some shapes can be described exactly by an equation, the way *x*Â² + *y*Â² + *z*Â² = 1 describes the surface of a ball. Such shapes can have holes, tunnels and loops in many dimensions, and mathematicians keep a catalogue of them, a bit like an inventory of every hole and tunnel in a sculpture. Hodge singled out a special kind of entry in that catalogue and guessed that each one can be built, LEGO-style, from smaller pieces that are themselves described by equations.

Solomon Lefschetz had already proved the simplest case around 1924, which settles every shape of three dimensions or fewer. A stricter version, allowing only whole-number amounts of each building block, turned out to be false in 1962, which is why Clay allows fractions. In 2025 Eyal Markman proved the conjecture for a family of four-dimensional shapes called abelian fourfolds. On 6 October OpenAI claimed it for another large family of shapes, with no machine-checked proof, and the next day withdrew three related papers after a sign error broke one argument.

A proof would pay off almost entirely inside mathematics, as a firm bridge between geometry, topology and algebra. Thereâs no gadget at the end of this one. String theorists use related numbers to describe curled-up extra dimensions, but they can already compute those.

## Why Is a Proton Heavy?

In February 1954 a young physicist, Chen Ning Yang, gave a seminar in Princeton on a new kind of force field he had worked out with Robert Mills. Wolfgang Pauli, in the audience, pressed him again and again: what is the mass of the particle that carries this field? By Yangâs own account he had no answer. The equations described massless particles, yet the strong nuclear force they were meant to explain reaches only across the width of an atomic nucleus, which points to a mass.

Step on a scale. Nearly all of what it reads is in the protons and neutrons of your atoms, and the masses of the quarks inside those account for only about 1% of it. The rest is energy, the energy of the force that glues the quarks together, turned into mass by E = mcÂ². YangâMills theory describes that glue. The prize asks for a proof that the theory exists as solid mathematics and has a âmass gapâ: a lightest possible particle, with nothing between zero and that mass, like a guitar string that canât play any note below a certain pitch.

Physics found its way through in 1973 (a Nobel Prize in 2004), and the resulting theory of quarks and gluons works superbly, but nobody has turned it into rigorous mathematics. Arthur Jaffe and Edward Witten, who set Clayâs problem, ask for that proof, and the gap is the hard part: the equations contain no masses at all, yet the lightest particle they produce must weigh something.

<sup>4</sup>exists, and Î > 0

<sup>4</sup>= space and time Â· Î = the mass of the lightest particle Â· for any compact simple gauge group

That bottom bar is the bit I find hard to get over: the Higgs field, famous for âgiving particles massâ, accounts for about 1% of a proton. A proof wouldnât change a single measurement, because calculations like the top half of the chart already get these masses right. It would explain, in mathematics, why the world isnât massless. It has nothing to do with the Higgs, a common mix-up, and it wonât unlock new energy sources.

## Easy to Check, Hard to Find

On 20 March 1956 Kurt GÃ¶del wrote to John von Neumann, who was by then seriously ill with the cancer that killed him the following year. Tucked into the letter was a question decades ahead of its time. Could a machine find the proof of a theorem in a time that grows only modestly, say in proportion to the proofâs length or its square? If so, GÃ¶del noted, much of a mathematicianâs work could be handed over to machines.

Checking a finished sudoku takes a minute; solving a blank one can take an evening. Opening a combination lock is instant once you know the code; finding the code means trying combinations one by one. Plenty of real jobs have this shape: planning a delivery route, drawing up a school timetable, packing a lorry. P vs NP asks whether that gap is real, or whether every problem whose answer is quick to check also has a quick way to find the answer that nobody has spotted yet.

The question got its modern form in 1971, when Stephen Cook showed that a huge family of these problems stand or fall together; Leonid Levin found the same thing independently in the Soviet Union. P is the set of problems a computer can solve quickly. NP is the set whose answers it can check quickly. The question is whether they are the same set.

âQuicklyâ has a precise meaning: the work grows gently as the problem gets bigger. Say youâre packing for a trip and want the best choice from *n* items. Trying every combination means 2<sup>*n*</sup> checks, so each extra item doubles the work. A clever method needing something like *n*Â² steps would only quadruple its work when the list doubles. The gap is enormous:

Almost everyone thinks the answer is no. In William Gasarchâs 2018 poll, 88% of 124 respondents said P â NP, as did 99% of those he judged to be experts. Proving it is another matter: several of the most natural lines of attack have themselves been proved hopeless.

If P â NP, as expected, very little changes, because the world already runs as if it were true. If P = NP with a fast, practical algorithm, the world changes overnight. The encryption behind online banking and shopping breaks, and GÃ¶delâs proof-finding machines arrive. Almost nobody expects it. Even then it wouldnât cure cancer, as is sometimes claimed; biology is held back by experiments and data, not by search.

## Counting Points on a Curve

In the early 1960s Bryan Birch and Peter Swinnerton-Dyer did something new: they used a computer to discover a conjecture.

Curves like these already protect the messages on your phone, though in a different form (more on that below). The question is how many points on such a curve have coordinates that are simple fractions. Some curves have only a handful; others have infinitely many. Birch and Swinnerton-Dyer guessed that you can tell which, and how rich the supply is, by doing âclock arithmeticâ on the curve: counting its solutions on a clock with 5 hours, then 7, then 11, and so on through the primes. (On a 7-hour clock, 5 + 4 = 2.) They ran those counts on Cambridgeâs EDSAC 2.

An elliptic curve is the set of points satisfying an equation like *y*Â² = *x*Â³ â *x* + 1. Draw a line through two of its rational points (points whose coordinates are fractions) and it meets the curve at a third. Flip that over the *x*-axis and you have a new point, which counts as their sum. From a few starting points you can keep generating new ones, and the number of independent starting points you need is the curveâs rank.

Birch and Swinnerton-Dyer noticed that curves with plenty of solutions on small clocks tended to have high rank, and turned the hunch into a precise guess: combine all those clock counts into one function, and the rank can be read off from how it behaves at a single point.

<sub>s=1</sub>L(E, s)

<sub>s=1</sub>= how many times it vanishes at s = 1

Itâs proved whenever that function vanishes at most once at the key point (Gross and Zagier, 1986; Kolyvagin, 1988â89), and more than 66% of curves are now known to obey the rank part. The highest rank anyone has found is at least 29 (Elkies and Klagsbrun, 2024). OpenAIâs October release claims the full formula for a restricted class of curves. It isnât machine-checked, and I couldnât find an independent expert verdict on it.

A proof would settle a puzzle about a thousand years old, the congruent number problem: which whole numbers are the area of a right-angled triangle with rational sides? Six is, thanks to the 3-4-5 triangle. So is 157, but its simplest triangle has sides whose fractions run to dozens of digits. Jerrold Tunnellâs 1983 test for this works in one direction and needs BSD for the other. And no, it wouldnât break Bitcoin. Bitcoinâs signatures do use an elliptic curve, *y*Â² = *x*Â³ + 7, but entirely in clock arithmetic on a single, enormous clock, where rank means nothing.

## What Would Actually Change?

Press releases tend to skip the honest version, so here it is in one place: what a proof would give us, and what already works fine without one.

| Problem | A proof wouldâ¦ | Already works without it | 
|---|---|---|
| NavierâStokes | Tell us whether the fluid equations can be trusted at the very smallest scales | Weather models, aircraft design, blood-flow simulations | 
| Riemann | Turn thousands of âassuming RHâ theorems into theorems, and pin down how evenly the primes are spread | Encryption, primality tests, prime counts | 
| PoincarÃ© | Done: a complete catalogue of 3D spaces, and Ricci flow as a tool | Not the shape of the universe, which is for telescopes | 
| Hodge | Join geometry, topology and algebra; a payoff mostly inside maths | The numbers string theorists use, which are already computable | 
| YangâMills | Give physics its first rigorous interacting quantum field theory in four dimensions | Lattice calculations of particle masses | 
| P vs NP | If P â NP, confirm what we assume. If P = NP and fast, break most public-key encryption overnight | Solvers that crack huge real-world scheduling and chip-design problems | 
| Birch and Swinnerton-Dyer | Settle the congruent number problem, and make rank calculations certain to finish | Elliptic-curve cryptography, which never uses rank | 

Itâs a deflating table, and I think itâs the right one. Each problem asks whether a tool we already rely on stands on solid ground, and people have built on it anyway, the way they crossed bridges long before anyone could calculate why bridges stand.

## Checked, but Understood?

It has been a strange autumn. On 11 September, the same day as Clayâs statement, 25 Fields medallists published a declaration titled âA Severe Misalignment of AI in Mathematicsâ. Their argument is that solving famous problems is a tool and a proxy for what mathematics is really for, which is understanding. On 6 October OpenAI released 722 machine-written manuscripts at once, grouped into 372 families of results. *Nature* reported that none of them solves any of the five open Millennium problems. The next day three were withdrawn, because a sign error had broken one argument and the two papers that built on it.

Some of these proofs come with a version in Lean, a program that checks every logical step. Think of it as a perfect proofreader: it guarantees that each line follows from the last, but not that the essay answers the question that was set. A person still has to confirm that what was typed into Lean says what the paper says, and the Kingâs College preprint shows how that can slip. Terence Tao made a related point in a public lecture at Caltech on 9 October: proofs are going from scarce to abundant, and optimising blindly for solved problems now risks harming the field.

A proof that a computer has verified but no human yet understands reminds me of a signal we could hear but never read. Mathematicians are already doing the reading: Tao has set up a community project to rewrite OpenAIâs papers, and Lamzouriâs shorter two-thirds proof is exactly how a verified result becomes an understood one.

## Very Likely

Riemann turned 200 on 17 September, and mathematicians marked it with meetings in Hannover, Berlin, GÃ¶ttingen, MÃ¼nster and London. His âvery likelyâ is now 167 years old. Between August and October one machine pushed the share of zeros known to sit on his line from under 42% to two-thirds, and another narrowed the strip they can live in. Neither is his hypothesis, and I doubt either will be the last word.

Clayâs page on NavierâStokes has a line I keep coming back to: âa proof gives not only certitude, but also understanding.â For most of history those two arrived together. Whoever proved something hard understood why it was true. This year they started to come apart, and we may soon have a proof of a famous problem that is checked, correct, and understood by nobody, at least for a while.

In the last post, Jonathan Swift was right about the moons of Mars for the wrong reasons. This is the opposite puzzle: being right for reasons nobody can yet follow. I think mathematicians are right to treat that as half a victory. Perelman showed that the money was never the point, and OpenAI seems to agree. What Clay was really paying for is understanding, and that part of the prize is still on the table.

1. Clay Mathematics Institute, The Millennium Prize Problems and the problem pages for NavierâStokes (status âActiveâ), Riemann, PoincarÃ©, Hodge, YangâMills, P vs NP and BSD Â· Rules for the Millennium Prizes (adopted 26 September 2018)
2. Clay Mathematics Institute (11 September 2026). Statement on the NavierâStokes announcement Â· C. Fefferman, Existence and smoothness of the NavierâStokes equation (official problem description)
3. Hilbert, D. (1900). âMathematical Problemsâ, trans. M. W. Newson, *Bulletin of the AMS* 8 (1902), 437â479. Online text
4. OpenAI (8 September 2026), NavierâStokes announcement, and the openai/math repository (company figures for the run) Â· *Quanta Magazine* (8 September 2026), âAI has solved one of mathâs $1 million Millennium Prize problemsâ, with Feffermanâs comments
5. *Scientific American* (September 2026), âDid OpenAI solve the wrong NavierâStokes problem?â (Silvestre) Â· On the credit dispute: NYU Shanghai RITS, summary of statements by Buckmaster and OpenAI
6. CÃ³rdoba, D. & MartÃnez-Zoroa, L. (2023), arXiv:2309.08495 Â· Constantin, P., Ignatova, M. & Vicol, V. (2026), arXiv:2609.20803 Â· Bastounis, A., Circelli, M. & Hansen, A. (2026), arXiv:2610.08144, and Kingâs College London, press release (27 October 2026)
7. Tao, T. (2016). âFinite time blowup for an averaged three-dimensional NavierâStokes equation.â *Journal of the AMS* 29 Â· Zhang, F. et al. (2019). âWhat is the predictability limit of midlatitude weather?â*Journal of the Atmospheric Sciences* 76
8. Riemann, B. (1859). âUeber die Anzahl der Primzahlen unter einer gegebenen GrÃ¶sseâ, in *Gesammelte mathematische Werke* , 2nd ed. (1892), p. 145. archive.org Â· Clay, Riemannâs 1859 manuscript
9. Platt, D. & Trudgian, T. (2021). âThe Riemann hypothesis is true up to 3Â·10<sup>12</sup> .â*Bulletin of the LMS* . doi.org/10.1112/blms.12460 Â· Schoenfeld, L. (1976). âSharper bounds for the Chebyshev functions Î¸(*x* ) and Ï(*x* ). II.â*Mathematics of Computation* 30, 337â360 Â· On Littlewoodâs sign changes: arXiv:2501.04488
10. AlpÃ¶ge, L. & Furman, R. (2026). âMore than two thirds of the zeta zeros are simple and on the critical line.â arXiv:2608.13637 Â· Lamzouri, Y. (2026), arXiv:2609.02882 Â· C. Turnage-Butterbaugh, guest post on Taoâs blog (history of the proportion records) Â· *Quanta Magazine* (19 October 2026), on the quasi-Riemann hypothesis
11. Perelman, G. (2002â03). math/0211159, math/0303109, math/0307245 Â· Clay, The PoincarÃ© Chair Â· Ã. Ghys, âGÃ©omÃ©triser lâespace : de Gauss Ã Perelmanâ (quotes PoincarÃ©âs last sentence) Â· Wikipedia, âGrigori Perelmanâ
12. Markman, E. (2025), arXiv:2502.03415 Â· B. Totaro and C. Voisin, guest posts on the status of the Hodge conjecture Â· Retraction Watch, on OpenAIâs withdrawn papers
13. Yang, C. N. & Mills, R. L. (1954). âConservation of isotopic spin and isotopic gauge invariance.â *Physical Review* 96, 191. doi.org/10.1103/PhysRev.96.191 Â· Morningstar, C. & Peardon, M. (1999). âThe glueball spectrum from an anisotropic lattice study.â*Physical Review D* 60, 034509. doi.org/10.1103/PhysRevD.60.034509 Â· Quark masses: Particle Data Group
14. Gasarch, W. (2019). âThe third P =? NP poll.â *SIGACT News* . cs.umd.edu Â· GÃ¶delâs 1956 letter, trans. in M. Sipser, âThe history and status of the P versus NP questionâ (1992)
15. Birch, B. & Swinnerton-Dyer, P. (1963, 1965). âNotes on elliptic curves I, II.â *J. reine angew. Math.* Â· Wikipedia, âBirch and Swinnerton-Dyer conjectureâ Â· Bhargava, M., Skinner, C. & Zhang, W. (2014), arXiv:1407.1826 Â· Certicom, SEC 2 (the secp256k1 curve)
16. Tao, T. (2026). Posts on the Fields medallistsâ declaration and âMath 2.0â
17. Riemann 200: Hannover Â· Berlin Â· GÃ¶ttingen Â· MÃ¼nster Â· London
18. On screen: L. Fortnow, âFactoring Numb3rsâ (19 February 2005) Â· Wikipedia, âGifted (2017 film)â

Translations of Riemann are Enlightâs own; Hilbert is quoted in Newsonâs 1902 translation. Every chart is drawn from numbers computed for this post (prime counts, li(*x*), zeta zeros, quark masses) or taken from the papers cited. The 2026 results in this post were unrefereed when it was published; check the sources above for anything that has changed since.
