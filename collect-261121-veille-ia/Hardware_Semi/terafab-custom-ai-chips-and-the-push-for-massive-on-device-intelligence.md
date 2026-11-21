---
id: collect-261121-veille-ia/hardware-semi/terafab-custom-ai-chips-and-the-push-for-massive-on-device-intelligence
title: "Terafab, Custom AI Chips, and the Push for Massive On-Device Intelligence"
domain: hardware-semi
role: reference
task: article
actors: []
dates: []
keywords: ["chips", "terafab", "tesla", "custom", "massive", "musk", "push", "on-device"]
source: https://medium.com/@anancysilver/terafab-custom-ai-chips-and-the-push-for-massive-on-device-intelligence-90eec89bd1d2
source_anchor: ""
source_lines: [1, 179]
section: "Hardware_Semi"
---
# Terafab, Custom AI Chips, and the Push for Massive On-Device Intelligence

In late 2025 and into 2026, Elon Musk and Tesla outlined a semiconductor plan centered on a project referred to as Terafab. The goal is a massive manufacturing complex capable of producing chips at a scale that would support Tesla vehicles, the Optimus humanoid robot, and SpaceX’s orbital data-center ambitions.

This article is a detailed explanation of the underlying technology. It covers the kinds of chips involved, how they work inside the products they will power, memory and packaging (including wafers, DRAM, and HBM), the difference between training and inference, real-time inference, why Optimus is treated as a major long-term driver, the history of Tesla’s clusters and Dojo, how the AI on Optimus relates to Grok, the meaning of captive silicon, and how Tesla’s chips differ from Nvidia’s.

## Terafab

SpaceX and Tesla have publicly discussed a first-phase investment on the order of $16.8 billion for a facility in Grimes County, Texas (near Gibbons Creek Reservoir, east of College Station). This was formally announced by Texas Governor Greg Abbott on August 6, 2026, as a joint SpaceX/Tesla project with xAI involvement and site clearing and earthworks began.

At full build-out the campus is described as exceeding 100 million square feet of manufacturing space. The arithmetic on that figure is straightforward: 100 million square feet is roughly 1,736 American football fields, or about 2.7 times the area of Central Park.

Musk has repeatedly linked the project to a target of roughly 1 terawatt of compute per year, which he has translated into a production goal on the order of 100–200 billion chips annually. The chips are intended for Tesla vehicles, Optimus robots, and SpaceX orbital AI data centers.

Intel is involved as a development/foundry partner (Intel 14A process). Musk has confirmed talks with TSMC but has said SpaceX/Tesla will build and run the fab themselves (TSMC at most subleasing part of it). Early AI5 volume is still expected from Samsung and TSMC; Terafab is for the big ramp later. The long-term plan (per recent reporting citing Nikkei) targets roughly 700,000 wafers per month for memory by 2032 (out of a ~1 million wafer total).

“Every chip factory on Earth produces only 2 percent of what they’ll need” Musk reflected on demand projection for a future fleet of cars, robots, and orbital systems.

## What the Chips Are For and How They Work in the Products

Terafab is planned to make two main families of custom AI chips, plus associated memory and packaging under one roof. None of these have been produced at Terafab yet (the first test chips are targeted for 2027). The chips under discussion fall into two broad categories that Tesla has been developing for years: inference accelerators for real-time decision-making on the edge (in vehicles and robots), and higher-capacity systems intended for training and for space-based compute.

Current production hardware in Tesla vehicles is the AI4 / Hardware 4 computer. Next-generation inference chips are designated AI5 (sometimes referred to in internal roadmaps as SI5) and AI6 (SI6). These are custom system-on-chip designs optimized for running already-trained neural networks that process camera feeds and produce control outputs. Musk has stated that a single AI5 is expected to be roughly comparable to an Nvidia H100 on Tesla’s own workloads, with substantially lower power and cost; a dual-AI5 configuration has been described as approaching Blackwell-class performance under the same conditions. AI6 is projected to deliver still larger gains relative to today’s AI4 — on the order of 40× to 80× in various compute metrics according to the published roadmap figures, though those numbers depend on measurement methodology and remain forward-looking. Process for the big ramp is planned on Intel’s 14A node (~1.4 nm class).

A separate line, sometimes still called **Dojo D3** (or linked to AI7 / Dojo-related naming in some materials), is oriented toward training and toward space deployment. Earlier Dojo efforts used wafer-scale packaging and on-chip SRAM. The newer approach moves toward a multi-chiplet design with stacked high-bandwidth memory and is described as optimized for space, training, and scaling. Tesla continues to purchase large quantities of Nvidia GPUs for its existing training clusters (Cortex and related systems used by Tesla and xAI). The custom silicon is primarily for the products themselves and for future internal capacity.

**Edge inference chips (AI5 / AI6 family)** are low-power, high-efficiency designs for real-time, on-device AI. In Tesla vehicles and Cybercabs they run the neural networks for Full Self-Driving. The chip continuously processes camera and sensor data for perception, path planning, and vehicle control. The core driving loop stays onboard rather than depending on a remote server. In Optimus humanoid robots the same style of chip handles onboard vision, balance, manipulation, and task decision-making. Power efficiency is critical because the robot is battery-powered and needs to operate for extended periods. Musk has framed Optimus as one of the largest long-term demand drivers. These chips are specialized for edge inference (running already-trained models quickly and efficiently) rather than large-scale training.

**Space-optimized chips (D3)** are higher-power designs for the space environment. Key differences include radiation tolerance / ruggedization, higher power density, and adaptation for orbital conditions (thermal management in vacuum, radiation exposure, limited maintenance). Their purpose is to power SpaceX’s planned orbital AI data centers — solar-powered satellites carrying large amounts of AI compute. The chips sit inside satellites that generate power from continuous solar exposure and reject heat more easily in space. Musk’s stated vision is that a large fraction of future AI compute (he has sometimes said roughly 80 percent) would live in orbit rather than on Earth, avoiding terrestrial power, land, and cooling constraints. The satellites would be launched on Starship.

Public statements and company materials focus almost entirely on three areas: Tesla vehicles / Cybercabs (Full Self-Driving), Optimus humanoid robots, and SpaceX orbital AI data centers / AI satellites. The project is described as captive silicon for Musk’s own companies — chips made only for Tesla and SpaceX’s own products, not sold to outside customers. The factories exist to supply their cars, robots, and satellites.

## Memory, Wafers, DRAM, HBM, and Packaging

Terafab’s stated goal is to bring logic, memory, and advanced packaging under one roof so the companies can co-design and iterate faster. Exact public specifications of the memory types or packaging flows for the Terafab versions of the chips are still limited, but the concepts are clear.

A modern AI chip is not just a single piece of silicon. It is a carefully arranged system of compute, memory, and the connections between them. The way those pieces are organized — the architecture — and the way they are physically assembled — the packaging — often matter as much as the raw process node.

### **Wafers**

A wafer is a thin, round disc of highly pure silicon, typically about the size of a dinner plate (300 mm in diameter in modern fabs). Chip factories pattern circuits onto the wafer through dozens of lithography, etching, and deposition steps. After fabrication the wafer is tested and cut apart into individual chips (dies). Yield — the percentage of good dies — is critical: a small drop in yield at this scale becomes millions of lost chips. When people talk about “700,000 wafers a month,” they mean that many full discs of silicon being processed.

### **Memory types and why they differ**

**DRAM (dynamic random-access memory)** is ordinary computer memory — the kind used in phones, laptops, and most servers. Data is stored in capacitors that must be refreshed periodically. It is relatively inexpensive and offers good capacity, but the physical distance between the memory chips and the logic die limits how fast data can travel. Bandwidth is measured in gigabytes per second. Edge chips for vehicles and robots are more likely to use power-efficient variants such as LPDDR (low-power DRAM). Recent public figures for AI5 have been in the 96 GB range, with higher capacities discussed for AI6. Bandwidth, rather than raw capacity, has been emphasized as the more critical constraint for robot and vehicle performance.

**HBM (high-bandwidth memory)** is a special kind of DRAM. Instead of laying memory chips flat next to each other, manufacturers stack them vertically (like a multi-story building) and connect the layers with thousands of tiny vertical wires. The whole stack sits right next to, or even on, the main processing chip through advanced packaging. This gives much higher bandwidth — the amount of data that can move every second — because there are so many parallel connections and the distance is extremely short. Think of it as a multi-lane highway right next to the factory instead of a single-lane road farther away. The result is bandwidth measured in terabytes per second rather than hundreds of gigabytes. For large neural networks this is decisive: the chip can only perform useful math as fast as it can be fed weights and activations. If the memory cannot keep up, the expensive compute units sit idle. Space-oriented designs are more likely to emphasize HBM because they need higher sustained bandwidth and operate under different power and cooling constraints.

### **Packaging and architecture**

**Advanced packaging** (2.5D, 3D, multi-chip modules) is how the logic die and memory are physically joined. In a multi-chip module (MCM) several dies sit on a shared substrate or interposer so they can communicate at very high speed. Stacking memory directly on or beside the logic die further shortens the path.

Doing this packaging inside the same complex as the logic fabrication has two practical effects. First, the design-test loop becomes much shorter. Engineers can change an interconnect, reassemble, and measure results without shipping wafers to an external packaging house and waiting for them to return. Second, the teams that design the neural networks and the teams that design the silicon can co-optimize more tightly — adjusting memory interfaces, precision, and data pathways together rather than as separate hand-offs.

### **Why architecture choices differ between edge and space chips**

An edge chip for a car or Optimus is optimized for low power, low latency, and continuous real-time operation. Its architecture strips out anything not required for Tesla’s camera-to-control pipeline. Memory capacity and bandwidth are sized for the specific models that will run on it, not for general-purpose flexibility.

A space chip faces radiation, vacuum thermal conditions, and the need to handle larger or more varied workloads. Its architecture therefore emphasizes radiation tolerance, higher sustained bandwidth (often via HBM), and the ability to scale into larger multi-chip systems. Both still ultimately run models that were trained on terrestrial clusters; the difference is in how efficiently and reliably they can execute those models under their respective constraints.

In short, memory and packaging are not afterthoughts. They determine how close the chip can come to its theoretical compute performance in the real products. Bringing them under the same roof as the logic fabrication is one of the core reasons Terafab is being planned as more than a conventional foundry.

## Training Versus Inference, and What “Real-Time Inference” Means

A neural network is a software model made of many numbers (weights) that has learned patterns from data.

Training is the expensive learning phase. You show the model millions of examples (for Tesla, huge amounts of video from cars or robots, plus simulation) and adjust the numbers until it gets good at the task. This needs massive clusters of powerful computers and is done offline, once or periodically. The result is a fixed set of weights — a trained model.

Inference is using the finished model to make predictions or decisions on new data. Once trained, the model is smaller and more efficient. You run it on new data (a live camera feed) and it outputs actions (steer left, grasp the cup, balance while walking, etc.). The Terafab chips are mainly for inference. They run models that have already been trained elsewhere.

Real-time / edge inference means doing that on the device itself, with low latency, without needing to send data to a remote data center and wait for a reply. In a Tesla or Optimus:

- Cameras and sensors feed data continuously.
- The chip runs the neural network locally and outputs steering, braking, walking, grasping, or other actions in milliseconds.
- This is necessary for safety and responsiveness; a round-trip to the cloud would be too slow and unreliable.

A car traveling at highway speed cannot wait seconds for a decision; a robot that is walking or grasping must update its motor commands many times per second. Edge chips such as AI5 are designed so that this loop runs locally on the device. Latency is measured in milliseconds, power is constrained by the battery or thermal budget of the vehicle or robot, and the silicon is stripped of features that are unnecessary for the specific models Tesla runs.

By contrast, general-purpose data-center GPUs from Nvidia are engineered to handle a wide variety of model architectures, precisions, and software frameworks. They excel at both training and flexible inference, but they consume far more power and are not optimized for the tight thermal and cost envelope of a mass-produced robot or car. Tesla’s stated strategy is to keep buying Nvidia hardware for large-scale training while using its own, more specialized chips for the high-volume, power-constrained inference that occurs inside the products.

## Edge Chips, Space Chips, Clusters, and Tesla Dojo

Edge chips (AI5/AI6) are the inference accelerators that live inside the vehicle or robot. They are battery-efficient and tightly co-designed with Tesla’s camera and control stack.

Space chips (D3) are built for orbital data centers. They prioritize radiation tolerance, higher sustained bandwidth, and the ability to operate in vacuum conditions. Both still run models trained on terrestrial clusters.

A cluster is simply a large collection of accelerators (GPUs or custom chips) networked together so they can train or run very large models. Tesla’s main training clusters at Gigafactory Texas are called Cortex (mostly Nvidia hardware). Earlier Dojo efforts attempted to build a custom training supercomputer around wafer-scale tiles. The original ground-based Dojo project was shut down in 2025. A newer version (Dojo 3 / D3) was restarted with a focus on space-based AI compute and is expected to use stacked HBM. Training of FSD and Optimus models currently runs primarily on the Cortex clusters (Nvidia), with custom silicon work continuing for future chips. xAI (now under SpaceX) also has large Nvidia-based clusters used for training Grok models. The models trained on those clusters — vision transformers, planning networks, control policies — are then quantized and optimized so they can run efficiently on the edge chips.

## Optimus as a Volume Driver and the AI That Runs on It

Optimus is repeatedly described as one of the largest long-term drivers of chip volume because a humanoid robot that performs useful physical work could, in principle, be produced in far larger numbers than cars. Cars are measured in millions of units. Musk has talked about Optimus eventually reaching hundreds of millions or even a billion units (far beyond vehicle production). Every robot needs one or more of these edge inference chips running continuously on battery power. That creates a much larger long-term demand signal than vehicles alone. In one approximate split he has floated, roughly 25 percent of eventual Terafab output might go to Optimus and 75 percent to orbital AI spacecraft, but Optimus remains a major terrestrial volume driver. If production reaches even a fraction of the numbers Musk has sketched, the aggregate demand for inference silicon becomes enormous.

The AI inside Optimus is not a single monolithic model identical to Grok or ChatGPT. It consists of specialized neural networks for vision, locomotion, manipulation, and task planning, trained primarily on Tesla’s data and simulation pipelines. These are the same family of models that power Full Self-Driving. Higher-level reasoning or language interaction can incorporate models from the broader xAI / Grok family. Musk has described a setup where Grok acts as the higher-level “thinking” / planning layer (System 2), while Tesla’s real-time neural networks handle the fast, instinctive control (System 1). Optimus V3 already uses Grok for voice. There is also a related project called Digital Optimus (or Macrohard) that pairs Grok’s reasoning with Tesla’s real-time screen/video processing for digital work.

It is not “full Grok” running entirely on the robot (that would be too large and slow). The onboard chips run optimized, smaller versions or distilled models for real-time control, with Grok available for higher-level guidance when needed. You can think of the robot’s onboard AI as purpose-built for physical autonomy, with the option to tap into larger models like Grok. In that sense the robot carries its own “captive” models — weights that live only on Tesla silicon and are not exposed as a general-purpose API. Grok-style models can be used for planning or conversational layers when latency and power allow, or when the robot is connected, but the safety-critical, high-frequency control remains on the local, specialized networks.

Potential uses discussed publicly include factory and warehouse work, general household and commercial tasks, and potential large-scale deployment. Military use, broad public sales, or Mars work have been mentioned as long-term possibilities by Musk, but they are not detailed production plans yet. The chips enable the real-time AI that lets the robot see, balance, and act without constant remote control.

## How This AI Differs from Grok, Gemini, or ChatGPT

Grok, Gemini, and ChatGPT are large general-purpose language models. They are excellent at conversation and broad knowledge but are too big and power-hungry to run efficiently on a robot’s battery for continuous real-time control. Optimus’s core AI is specialized (vision + action for physical tasks). It is smaller, faster, and optimized for the onboard chips. Larger models like Grok can still assist with higher-level reasoning or conversation, but the moment-to-moment movement and perception run on the specialized trained networks. The distinction is between a general-purpose conversational system and a tightly optimized physical-control system that can optionally call on the larger model.

## How Tesla’s Chips Differ from Nvidia’s

Nvidia’s high-end GPUs (H100, B200, and the Rubin generation now reaching customers) are general-purpose accelerators. They support a wide range of numerical formats, model architectures, and software stacks, and they are sold to any customer who can obtain them — cloud companies, researchers, carmakers, and others. They are excellent at both training big models and running them (inference). They are very powerful but power-hungry (hundreds of watts, often 700 W+ for high-end cards). Their strength is flexibility and raw training performance; their cost is high power draw and premium pricing. They currently set the industry standard for large data-center clusters.

Tesla’s AI5 and AI6 chips are application specific. They are custom designed for Tesla’s own needs only (not sold to outsiders). They are optimized mainly for edge inference — running already-trained neural networks in real time inside cars and Optimus robots. They focus on Tesla’s specific vision-based autonomy models (cameras in → driving or robot actions out). They are much more power-efficient for that job so they can run on a vehicle or robot battery. They omit circuitry that is irrelevant to Tesla’s camera-to-control pipeline, use process nodes and packaging chosen for volume and power efficiency, and are never offered for sale outside Tesla and SpaceX. Musk has claimed performance-per-watt and cost advantages of several times versus Nvidia on Tesla’s own inference workloads (and has said AI5 could be roughly comparable to an H100 on those workloads at far lower cost and power, with dual-AI5 approaching Blackwell-class). Those claims are specific to the targeted models and operating conditions; they do not imply that an AI5 would outperform an H100 on arbitrary training or general inference tasks.

The two approaches are complementary: Nvidia hardware trains the models at scale; Tesla hardware runs the resulting models cheaply and efficiently inside millions of vehicles and robots. In-house design and packaging further differentiate the effort. Because Tesla controls both the silicon and the neural networks, it can co-optimize them — removing unused precision, adjusting memory interfaces, and iterating without external foundry or packaging delays. That closed loop is the practical meaning of “captive silicon.”

Nvidia chips are powerful, flexible “Swiss Army knives” used everywhere in AI. Tesla’s chips are specialized tools built only for running Tesla’s autonomy models efficiently in cars and robots. Tesla still buys large numbers of Nvidia chips for training its models in data centers (Cortex clusters, xAI, etc.), while its own chips handle the day-to-day real-time work in the products.

## Closing Perspective

The Terafab concept, the AI5/AI6 roadmap, the D3 space-oriented work, and the continued reliance on Nvidia for training form a coherent, if still largely prospective, strategy. The verified elements are the large Texas investment figure, the 100-million-square-foot full-build-out target, the linkage of the chips to vehicles, Optimus, and orbital compute, the technical distinction between specialized edge inference silicon and general-purpose training GPUs, and the documented timelines pointing to 2027 test chips and 2028 volume production. Many of the more dramatic claims about immediate construction starts in the strongest form or exclusive purchase agreements lack independent confirmation as completed facts.

What is already clear is the engineering logic: train large models on powerful, flexible clusters; then run the distilled, optimized versions on custom, power-efficient chips that live inside the products themselves. Whether the manufacturing scale materializes as described will determine how quickly that logic can be applied to millions of cars and, potentially, far larger numbers of robots. The chips are custom AI processors (logic + associated memory + packaging) mainly for running real-time neural networks onboard robots and cars, and for larger-scale AI compute in orbit. Optimus is a large driver simply because of the enormous number of units Musk is targeting.

Skin of Sin | Nancy Silver

#SkinOfSin

## Sources

**Official announcements and government records**

- Office of the Texas Governor. “Governor Abbott Announces SpaceX Expansion In Grimes County.” August 6, 2026.
https://gov.texas.gov/news/post/governor-abbott-announces-spacex-expansion-in-grimes-county

**Primary reporting on the Terafab project, investment, and location**

- Electrek. “Tesla, SpaceX confirm ‘Terafab’ chip fab site — $16.8B first phase.” August 6, 2026.
https://electrek.co/2026/08/06/tesla-spacex-terafab-grimes-county-16-8-billion/
- Austin American-Statesman. “Elon Musk, SpaceX confirm $16.8B Terafab facility in Texas.” August 6, 2026.
https://www.statesman.com/business/article/elon-musk-terafab-spacex-grimes-county-22375537.php
- Manufacturing Dive. “SpaceX to invest $16.8B on first phase of Terafab project.” August 10, 2026.
https://www.manufacturingdive.com/news/spacex-to-invest-16b-first-phase-terafab-project-grimes-county-texas/827422/
- Engineering News-Record. “SpaceX Records Fight Exposes $119B Terafab Buildout Plans.” October 7, 2026.
https://www.enr.com/articles/63809-spacex-records-fight-exposes-119b-terafab-buildout-plans

**Musk statements on scale, timeline, and chip targets**

- Electrek. “Tesla and SpaceX announce $25B ‘Terafab’ chip factory.” March 22, 2026.
https://electrek.co/2026/03/22/tesla-spacex-terafab-chip-factory-ai-desperation/
- Reuters. “Elon Musk lays out Terafab AI chip project plan.” April 23, 2026.
https://www.reuters.com/business/autos-transportation/elon-musk-lays-out-terafab-ai-chip-project-plan-2026-04-23/
- All Things Elon. “Musk Details Tesla’s Terawatt-Scale Terafab Chip Plan.” November 7, 2026.
https://www.allthings-elon.com/articles/tesla-terafab-terawatt-chip-plan-baron-2026-11-07
- 24/7 Wall St. “Musk Says His Terafab Chip Plant Could Cost $1 Trillion.” November 7, 2026.
https://247wallst.com/investing/2026/11/07/musk-says-his-terafab-chip-plant-could-cost-1-trillion/

**Tesla AI chip roadmap (AI5/AI6/SI5/SI6 and D3)**

- Tom’s Hardware. “Tesla unveils AI accelerator roadmap, adopts new ‘SI’ branding… SI6 to bring 80X more compute performance, Dojo 3 headed to space.” November 6, 2026.
https://www.tomshardware.com/tech-industry/artificial-intelligence/tesla-unveils-ai-accelerator-roadmap-adopts-new-si-branding-in-super-intelligence-push-si6-to-bring-80x-more-compute-performance-dojo-3-headed-to-space
- Wccftech. “Tesla Pulls 2nm AI Chip Production Onto US Soil, Splitting AI6 and AI6.5 Between Samsung Texas and TSMC Arizona.” April 19, 2026.
https://wccftech.com/tesla-pulls-2nm-ai-chip-production-onto-us-soil-splits-ai6-ai6-5-between-samsung-tsmc/

**Memory capacity adjustments for AI5/AI6**

- CryptoBriefing. “Tesla cuts RAM on AI5 and AI6 chips to speed Optimus production.” October 1, 2026.
https://cryptobriefing.com/tesla-cuts-ram-ai5-ai6-chips/

**Overview and additional context**

- Wikipedia. “Terafab.”
https://en.wikipedia.org/wiki/Terafab
- Sillectus. “What Is Terafab? Musk’s Texas AI Chip Fab Explained.” October 29, 2026.
https://sillectus.com/articles/what-is-terafab/

## Elon Musk statements on X

- Elon Musk (@elonmusk). Post sharing the Terafab.ai project link and calling for people to join the effort to iterate quickly on new chip designs. November 6, 2026.
https://x.com/elonmusk/status/2118854622907674904
- Elon Musk (@elonmusk). Reply noting the joint announcement with Intel on the Terafab project. April 16, 2026.
https://x.com/elonmusk/status/2044844008170832169
- SpaceX announcement (widely shared and referenced by Musk). Official statement confirming the Grimes County site, $16.8 billion first phase, 100 million square feet, and >1 terawatt compute target. August 6, 2026.
 https://www.spacex.com/updates#terafab
(Shared on X by Sawyer Merritt: https://x.com/SawyerMerritt/status/2085387193892438322)
