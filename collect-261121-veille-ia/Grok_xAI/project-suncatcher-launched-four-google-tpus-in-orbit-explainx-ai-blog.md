---
id: collect-261121-veille-ia/grok-xai/project-suncatcher-launched-four-google-tpus-in-orbit-explainx-ai-blog
title: "Project Suncatcher Launched: Four Google TPUs in Orbit | explainx.ai Blog"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: []
keywords: ["google", "tpus", "orbit", "four", "suncatcher", "october", "project", "launched"]
source: https://www.explainx.ai/blog/google-project-suncatcher-tpu-satellite-transporter-18-2026
source_anchor: ""
source_lines: [1, 112]
section: "Grok_xAI"
---
Google put TPUs into orbit. On October 1, 2026, Travis Beals wrote that the Planet-built Project Suncatcher prototype launched on SpaceX's Transporter-18 rideshare, that the team has contact, and that the satellite is "operating as expected." Google's own launch post added the payload that the September explainer left unnamed: **four TPUs**. Over the coming weeks the team will collect radiation, vibration and thermal data. The chips are not serving Gemini to the public.

**Update — October 2, 2026:** Launch is no longer a schedule. Beals' in-orbit note confirms contact. The research paper behind the mission is now peer-reviewed in *Joule* (preprint: arXiv:2511.19468). Nothing about token prices, Vertex, or Gemini APIs changed. For the mechanism — solar, heat pipes, radiators, and why the chips pause — see how orbital AI compute works.

## Update (October 30, 2026): the prototype ran its first prompt, a Gemma 4 haiku

After about three weeks of commissioning, engineers sent the first prompt to the satellite this week, according to TechCrunch's Tim Fernholz. The model pre-loaded on board is Google DeepMind's **Gemma 4**, not Gemini, and the request was to write a haiku about being in space. The reply read "Blue marble below / Data streams in silent grace / Starlight feeds my core." Google also asked the spacecraft to report on orbital conditions around it, and a spokesperson told TechCrunch the satellite is operating nominally.

Three details sharpen what this earlier post said:

- **Power:** the prototype generates up to 1 kW. TechCrunch's correction notes it carries four TPUs but can run only one at a time, for about twenty minutes at a stretch, which complicates space-to-ground communication.
- **What the test campaign will look at:** thermal stress and the real space radiation environment. Radiation can cause bit flips that corrupt data without tripping normal monitoring. Ground tests suggested the effect would be minimal for inference and a bigger problem for training, and this is the first chance to check that in orbit.
- **Next step:** two more satellites with more advanced chips are planned for 2027, with higher power generation and inter-satellite optical links, as the section on milestones above describes.

TechCrunch also reports that Suncatcher researchers estimated, in a peer-reviewed analysis published this month, that SpaceX would need to fly Starship about 1,800 times to reach the learning-driven cost reductions of its earlier vehicles. That is TechCrunch's summary of the analysis, which we have not independently read. Nothing here changes pricing or availability of any Google model. A haiku is a demonstration that the stack boots and answers, not a benchmark of performance or reliability in orbit.

It is easy to file Project Suncatcher under moonshot theater. The better read is narrower: this is a test of one question, whether commercial AI accelerators survive the space environment. The answer determines whether orbital compute is an engineering roadmap or a slide. On Earth, data centers are still fighting for grid capacity and hyperscalers are still signing nuclear deals. Four chips on a rideshare do not move that line.

## TL;DR: what is flying and what it tests

| Question | Answer | 
|---|---|
| What launched? | A Planet-built prototype with **four** Google TPUs | 
| On what? | SpaceX Transporter-18 rideshare (Falcon 9), Vandenberg | 
| When? | October 1, 2026 (Google's dated post; morning of October 2 in India) | 
| Status | Contact confirmed; satellite operating as expected | 
| What is tested? | Launch vibration, radiation, thermal behavior in low Earth orbit | 
| Is it a data center? | No. Hardware survival test. A few months of experiments, not continuous training | 
| Workloads | Press preview said Gemini models will run in short bursts; Google did not publish a public endpoint | 
| Radiation result so far | Trillium TPUs survived a dose above a five-year mission in proton-beam tests, with bit-flip effects monitored | 
| Cooling reality | Chips run about 15 minutes before shutting down to cool (NYT, quoting Google) | 
| Paper | *Joule* peer-reviewed version of "Towards a future space-based, highly scalable AI infrastructure system design" | 
| Next milestone | 2027: two satellites testing high-bandwidth laser links | 
| Author | Travis Beals, Senior Director, Paradigms of Intelligence | 

## Why does anyone want AI compute in space?

Google's pitch is power. In low Earth orbit, satellites "can access near-constant sunlight, generating up to eight times more solar power than on Earth." Long term, linked constellations could handle larger AI workloads in orbit. Google frames it as a research moonshot with "measured, deliberate steps," comparing it to autonomous driving and quantum computing, which needed years of experimentation before practical systems.

The eight-times figure drew questions in the Hacker News discussion. Commenters explained it mostly as continuous sunlight: no night, no seasons, no weather, no atmosphere, with panels always facing the sun. Others noted that solar panels are cheap on Earth and asked why not just build eight times more of them. That is the whole debate in miniature, and it recurs below.

On Earth, data centers are fighting for grid capacity, water and permits, and hyperscalers are signing nuclear deals to keep training runs fed. Suncatcher is Google's way of measuring whether space can relieve any of that.

## What does the prototype actually test?

Google's post breaks it into four problems.

**Launch stress.** A rocket ride to low Earth orbit lasts about 10 minutes, with sustained loads up to 10 g and individual components such as TPU chips seeing 50 to 100 g. The team shook the satellite on all three axes to mimic launch. Google says it was "pleasantly surprised that the hardware held up."

**Radiation.** Solar events and cosmic rays can wreak havoc on electronics. The team ran TPUs in a proton beam at UC Davis's Crocker Nuclear Laboratory while running AI workloads, and monitored "how errors, like a bitflip, would affect our workloads." Initial results show Trillium TPUs "hold up remarkably well" and "can survive a radiation total ionizing dose greater than what they would receive during a five-year space mission." That covers both total dose and error behavior, though "some things can only be tested in space."

**Cooling.** With no airflow, "in a vacuum, you can only diffuse heat via radiators." Google is working on "a combination of heat pipes and radiators" and has tested it in a thermal vacuum chamber. The New York Times described the stack: chips on a motherboard, a thermal interface material "that comes in sheets like Fruit Roll-Ups," layers of aluminum and copper, then a radiator panel. Beals told the Times the chips can operate about 15 minutes before shutting down to cool off.

**Connectivity.** Future satellites will each carry dozens of TPUs in clusters, linked by lasers. Google says the lasers must run at very high bandwidth over extremely short distances, "similar to hitting a coin-size target from miles away while both points are in motion." That test is the 2027 mission with two satellites. The November 2025 research post sketched the longer design: dawn-dusk sun-synchronous LEO, free-space optical inter-satellite links, and an ML flight-control model for close formation. The preprint illustrates an 81-satellite cluster with a 1 km radius. Transporter-18 is one bus, four chips, radio to the ground.

## Does the 15-minute limit mean cooling is unsolved?

This is where the thread got heated, and where both sides are partly right.

**The physics side.** Radiative cooling follows the Stefan-Boltzmann law, so radiator area scales with power and the fourth power of temperature. A commenter worked through it: at roughly 300 K and typical panel efficiency, a megawatt of compute needs on the order of thousands of square meters of solar panel and over a thousand of radiator. One estimate cited SpaceX's own design at about 160 square meters of radiator for 175 to 250 kW of compute. Heat pumps that raise radiator temperature help, but need power and add complexity.

**The economics side.** A defender argued the calculation is straightforward and cooling is "a dumb hunk of metal" plus pumps, so the only real question is cost per megatoken: launch price per kilogram, satellite cost per watt, and terrestrial power cost in 2030. Critics answered that radiator and solar mass at gigawatt scale make the economics far worse than terrestrial, and that no repair is possible in orbit.

**The prototype's limit is a design choice.** A commenter noted the 15-minute figure reflects mass constraints on a test article; a bigger radiator changes it. Another pushed back that a test that can only burst for 15 minutes is not "solved." Both are consistent with what Google published: this satellite exists to gather data, not to prove cost-effective operation.

Nobody in the thread disputed that cooling can be done. The dispute is whether it can be done at a price that competes with a data center in Ohio.

## What did the thread add beyond cooling?

- **Military and strategic use.** Several commenters argued the real customers are defense and intelligence: processing space-based sensor data in orbit, with cost less important than capability. That is speculation, and Google's post does not mention it.
- **Maintenance.** A commenter asked about swapping failed TPUs. The most common answer was that failed or obsolete units would be deorbited, which raises waste and debris questions.
- **Latency.** Continuous sunlight requires orbits that can put satellites far from any given ground site. Training tolerates delay, interactive inference does not.
- **Other players.** A startup, Starcloud, was mentioned as having flown a small proof of concept, and SpaceX's own orbital compute plan came up repeatedly.
- **Alphabet's stake.** Commenters noted Alphabet holds a large SpaceX stake, a conflict-of-interest angle, though Transporter missions are ordinary rideshare flights sold to many customers.

## How does this relate to SpaceX's own space-compute plans?

Google is buying a ride on SpaceX's rocket while SpaceX pursues its own orbital compute. explainx.ai has tracked Elon Musk's claim that 99.99% of AI compute eventually goes to space and the SpaceX AI1 orbital data center plan. Musk's separate forecast that SpaceXAI could match top models soon is in our Fable / GPT-6-level model post. One of Anthropic's compute suppliers is also SpaceX, as covered in the Colossus 1 partnership post.

Launch access is effectively a shared utility. The differentiators will be hardware survival, cooling and networking, which is exactly what Suncatcher measures. For the chips themselves, see our overview of GPU, TPU, Trainium, Cerebras and Groq architectures and the Frozen v2 chip and Gemini 4 pre-training.

## What are the honest limits?

- **A prototype, not a capacity claim.** Four TPUs on a rideshare tell you nothing about cost per token.
- **Contact is not a thermal or radiation result.** "Operating as expected" means the bus is alive. The weeks of in-orbit data are the experiment.
- **Radiation lab results are not orbit results.** Google said so before launch and repeated it after.
- **No repair.** Terrestrial fleets depend on constant replacement. Failed or obsolete units are expected to deorbit.
- **Ground link.** Training data goes up and results come down; laser links between satellites do not fix that. The paper treats radio as enough for this pilot.

## What this means for what you build or pay

Not much this quarter. Nothing here changes an API price or a model you can call. The relevant takeaways are longer-term:

- **Compute is the binding constraint.** Every major lab is exploring unusual power and cooling paths because the standard ones are saturated. Expect token prices and rate limits to stay volatile.
- **Efficiency pays off sooner.** Routing tasks to smaller models and caching aggressively work today and reduce your dependence on the compute race.
- **Watch 2027.** If two satellites hold a stable laser link and the prototype's thermal data supports bigger radiators, orbital clusters become a credible engineering path. If not, expect the economics argument to win.

## Timeline

| Date | Milestone | 
|---|---|
| Nov 4, 2025 | Google announces Suncatcher and posts the preprint | 
| Sept 24, 2026 | Google publishes the explainer and video series ("next week") | 
| Sept 25, 2026 | Pichai confirms the Transporter-18 ride | 
| Oct 1, 2026 | Launch; Beals confirms contact; *Joule* paper noted as available | 
| 2027 | Two-satellite laser link test | 

## Related reading on explainx.ai

Official sources: Travis Beals, "Our Project Suncatcher prototype satellite is in orbit" (Oct 1, 2026); "Behind Project Suncatcher" (Sept 24, 2026); Google Research, "Exploring a space-based, scalable AI infrastructure system design"; arXiv:2511.19468; Google's launch post on X; Ars Technica's Sept 24 preview (four TPUs, ~15-minute bursts, fridge-sized "MVP").

*Payload and contact status as of Google's October 1, 2026 in-orbit note. Thermal and radiation results from orbit have not been published yet.*
