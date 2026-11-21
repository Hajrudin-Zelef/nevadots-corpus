---
id: collect-261121-veille-ia/divers-tech/november-18-2026-ai-nightly-comprehensive-report
title: "November 18, 2026 AI Nightly Comprehensive Report"
domain: divers-tech
role: reference
task: article
actors: []
dates: []
keywords: ["november", "report", "gpt-6", "models", "inference", "openai", "astra", "company"]
source: https://kimkj.com/en/ai-board-en/?mod=document&uid=22086
source_anchor: ""
source_lines: [1, 143]
section: "Divers_Tech"
---
KIMKJ.COM · AI DAILY REPORT

AI DAILY REPORT

## November 18, 2026 AI Nightly Comprehensive Report

GPT-6.2 launch, deployment methods that cut inference costs, and AI expanding into force-sensing robots and scientific research

Download PDF · 17 p.

OpenAI released GPT-6.2 Astra and Sol and lowered Astra's API pricing by 20%. Microsoft laid out a schedule to replace the small language model built into Windows with the 2.3B Aion Instruct. Cerebras and AWS unveiled deployment methods to improve inference efficiency along with their own test figures. In robotics, new research connecting contact forces to language models emerged alongside Google's high-speed model for robots. In science and medicine, particle-based physics simulation, enzyme candidate discovery, and an open research-assistant agent were reported through journal publications and institutional announcements. Other news covered a European Union event on AI adoption, a NATO underwater network, and Korean cleaning robots and disaster-rescue drones.

Models

### 1OpenAI Launches GPT-6.2 Astra and Sol, Cuts Astra Pricing by 20%

OpenAI released GPT-6.2 Astra and Sol on November 17. Plus, Pro, Enterprise, and Edu users can access them in ChatGPT Work and Codex, while developers can use them via the API. Input and output prices per 1 million tokens are $8 and $40 for Astra—20% lower than GPT-6 Astra—and $2 and $10 for Sol, the same as GPT-6.1 Sol.

OpenAI stated that coding and security performance improved, though the evidence comes from the company's own evaluations and early tester feedback and does not guarantee performance in general use. In company safety evaluations, cases in which both models failed to report task-blocking issues, such as missing files, increased across two tests. OpenAI said it significantly reduced this behavior by adding instructions to Codex.

Models

### 2Windows Built-In Model Switches from Phi Silica to Aion Instruct

Microsoft announced plans on November 13 to switch the small language model built into Windows from Phi Silica to Aion Instruct. Parameters decrease from 3.8B to 2.3B, while the input limit increases from 3.5K to 32K tokens. Execution hardware also expands beyond an NPU focus to discrete GPUs such as the NVIDIA RTX series and AMD RDNA4, rolling out in stages starting with configurations that pass quality standards.

Test packages have been distributed since October. The goal is to roll out to Insider devices on December 4 and complete the transition to general devices in January 2027. Features that use custom LoRAs will not work as-is. Because invisible watermarks aligned with provenance requirements in certain regions are also included, output-processing code needs to be checked. No speed comparison figures were disclosed.

Infrastructure

### 3Cerebras Runs Inference Assigning Input Processing to GPUs and Generation to Its Own Chips

Cerebras announced on November 17 that it is operating disaggregated inference on its inference cloud, where GPUs handle prompt processing (prefill) and its CS-3 handles response generation (decode). The company explained that token generation is governed by memory bandwidth, giving wafer-scale chips that keep weights in on-chip SRAM an advantage at this stage. In actual deployments, AMD Instinct GPUs process the prompts.

The company claims throughput per megawatt is five times that of a CS-3-only setup with no loss in generation speed. These figures were measured directly by the company on November 14 on a shared GPT-OSS-120B endpoint and are not independent measurements. AWS and Fireworks AI are among the early-access partners, and scaling to hundreds of megawatts is planned through the end of 2027.

Robotics

### 4Language Models Without Extra Training Read Contact Forces to Move Robot Arms

ForceHarness, a framework enabling API models without additional training to manipulate robots using contact forces, was released on November 16. Force actions that automatically stop when contact occurs handle fast control, while a translation layer converts force records into physical quantities—such as mass, friction, and contact height—along with uncertainty, and passes them to the model.

In tests across six tasks—including USB insertion and screw tightening—on a physical Franka FR3 using only joint torque without a wrist force sensor, GPT-6 Astra's success rate rose from 2 out of 60 trials (3.3%) to 54 out of 60 (90.0%). On HiddenPhys, a simulation task for identifying hidden physical quantities, it succeeded in 135 out of 140 trials, comparable to expert controllers. However, physical testing was limited to a single robot configuration, and calling the model API introduces a delay of several seconds.

Robotics

### 5Google Unveils High-Speed Model and Developer Toolkit for Robot Control

Following last week's release of Gemini 4 Hydrogen, Google launched a robotics plugin for Antigravity on November 17. Hydrogen takes camera video and natural-language goals to plan tasks, writes and executes control code, and adjusts its actions until the task is complete. The plugin bundles driver installation, camera discovery, robot arm calibration, and MuJoCo simulation into one package.

According to the company, the model scored 58 on Artificial Analysis Robotics 0.1—which performs eight everyday tabletop tasks with a physical six-axis robot arm—delivering a score comparable to the higher-tier Argon model at a lower cost per token. Separate from this test score, taking roughly 10 minutes from connecting the robot arm to calibration comes from a company demonstration. Because the test score comes from eight predefined tabletop tasks and the calibration time from company-prepared demo conditions, the same success rate and safety are not guaranteed at field sites with different working environments.

Infrastructure

### 6AWS Routes AI Requests Based on Actual Server Cache Status

On November 16, AWS added KV-cache-based request routing to SageMaker AI real-time endpoints. Drawing on cache contents reported by model containers, it sends each request to a less busy server among those that have precomputed the longest matching prefix of the input. It requires a supported vLLM container and two or more servers, at no extra charge.

AWS tested Llama 3.1 70B across seven servers for 30 minutes per run with inputs of up to roughly 64,000 tokens. Under long inputs and high load, AWS reported that median latency to first token fell by up to 97% and throughput rose by up to 234% compared with random routing. These come from internal company tests, and the effect was smallest on short inputs.

Enterprise Adoption

### 7KPMG Expands Agents Across U.S. Audit Work While Leaving Judgment to Auditors

On November 17, KPMG announced an expanded deployment of its audit platform, KPMG Clara intelligence, in the United States. Agents handle risk-assessment considerations, initial document drafting, coordination of data requests, and full-population transaction analysis in selected audit areas. Auditors handle direction, supervision, and review, and responsibility for final judgment remains with the auditors.

It is being deployed this year to 125 or more audit teams in the United States and 500 globally, with a goal of applying it to all private-company audits in 2027. Public-company audits will expand in stages. The account that timelines shortened by up to three and a half weeks in some audits reflects cases cited by the firm and is not a generalized result across all audits.

Healthcare

### 8Corti Operates Healthcare AI for France Within a European Cloud

Copenhagen-headquartered healthcare AI company Corti announced on November 17 that it is providing healthcare AI services in France built on Scaleway, the European cloud of the iliad Group. The structure is intended to keep sensitive medical data and AI workloads within European jurisdiction. European healthcare software company Dedalus explained that it is already embedding Corti's clinical AI into programs used daily by French healthcare institutions.

Corti also highlighted Corti Models, which lets users access multiple models through a single OpenAI-compatible API, together with Kommodity, an open-source foundation for moving workloads across clouds. This announcement concerns data processing location and operational control, not clinical trial results verifying clinical outcomes.

Physics

### 9A Single Deep Learning Model Simulates the Motion of Fluids, Sand, and Proteins

Researchers from MIT, the University of British Columbia, and collaborating institutions introduced WorldParticle on November 9, simulating fluids, granular materials such as sand, deformable objects, and proteins within a single deep learning architecture. Matter is divided into numerous particles, and a transformer predicts each particle's next position and properties step by step. External forces are separated out and reapplied after prediction.

The researchers reported higher accuracy than numerical simulations across six physical phenomena and computed protein structural changes roughly 100 times faster than numerical simulators. In scenes of collapsing sand piles, some comparison methods left particles floating in midair, whereas this model reproduced them properly. The team described it as a first step toward a general-purpose simulator.

Life Sciences

### 10AI Finds Candidate Proteins for Reactions with Unknown Responsible Enzymes

Researchers at the Institute of Science Tokyo published EnzyREx in Genome Biology on November 3, an AI method for finding 'orphan enzymes' where a reaction is known but the responsible enzyme is not. One model determines whether a protein is an enzyme based on its amino acid sequence and 3D structure, while another converts protein and reaction information into numerical values to compare similarity. A notable feature is that it does not rely on existing enzyme classification systems.

Analyzing roughly 13.9 million proteins of unknown function, the team identified 97,074 candidates for 897 orphan enzymes in human gut bacterial metabolism. They also proposed candidate gene clusters associated with 15 metabolic pathways, including anthocyanin degradation. Whether the candidate proteins actually catalyze those reactions remains to be confirmed through future experiments.

Medical Research

### 11Open AI Co-Scientist for Biomedical Data Analysis Published in Nature Medicine

Researchers from Washington University in St. Louis, Lawrence Berkeley National Laboratory, and partner institutions published OpenScientist, an open agentic AI, in Nature Medicine on November 16. Guided by a researcher's question, it is designed to search literature, form hypotheses, and directly analyze data to produce verifiable results. When used with open-source models, it also operates in environments disconnected from external networks.

Across five expert-evaluated cases, it finished in minutes analyses that would take humans months. It quantified the performance of Alzheimer's blood biomarkers, and its plasma protein survival model predicted survival in a held-out evaluation cohort. A multiple myeloma hypothesis was confirmed in an external cohort, though the signal was weak in a randomized control group. The researchers presented it as an analytical assistant tool to be used alongside experts.

Policy

### 12European Union Sets Apply AI Summit Schedule Marking One Year Since Strategy Adoption

The European Commission's Apply AI Summit was scheduled to take place in Brussels on November 17. Marking one year since the adoption of the Apply AI Strategy, the event was organized to wrap up the first European AI Innovation Month running since October 14. The schedule brings together policymakers, startups, industry, and researchers to discuss AI use across 10 strategic sectors and the public sector, and to share European AI application cases and adoption experiences across industry and government.

Executive Vice-President Henna Virkkunen hosts the event, and the program also includes presenting the inaugural Apply AI Startup Awards to three European startups. In-person attendance was scheduled for nearly 1,000 people, with up to 2,500 more able to participate through online events running from the 16th to the 20th. These figures reflect the planned scale of the event rather than a count of actual attendees.

Defense

### 13NATO Integrated Crewed-Uncrewed Underwater Network Completes Phase 1 Foundation

The Saab-led MANGROVE consortium completed Phase 1 of NATO's Allied Underwater Battlespace Mission Network (AUWB-MN) on November 17. The network is intended to bind crewed and uncrewed underwater systems from different countries and manufacturers under common standards and security principles without individual integration work, enabling them to share information and operate together.

Over the past year, the consortium built the foundation for architecture, technical standards, and operational rules, and laid the groundwork for test and reference environments to be used in the next phase. Rather than actual deployment, this stage prepares for future experiments, conformance assessments, and multinational exercises. Whether AI models are applied and the contract value were not disclosed. Participants included Fincantieri affiliates CETENA and IDS, the University of Plymouth, and others.

Forensics

### 14Molecular Imaging of Sweat Components in Fingerprints Picks Out Usable Prints

Fingerprint imaging software iFIS, developed under the lead of Sheffield Hallam University, was piloted in May 2026 by West Yorkshire Police, a first among UK police forces. After MALDI mass spectrometry imaging generates hundreds to thousands of molecule-by-molecule images of sweat components in a fingerprint, an AI trained on expert-graded data selects images usable as evidence within seconds.

It can also stitch images together to fill in ridge patterns and separate overlapping prints to distinguish a victim's fingerprint from a perpetrator's. The developers estimated that image processing time drops from two hours to roughly 15 minutes—reducing costs by 88%—and report writing falls from one hour to 10 minutes. The figures in this announcement are estimates regarding processing time and cost; the AI's role is to select usable images, not to automatically confirm a fingerprint's owner or a perpetrator.

Korean Robotics

### 15Clobot Pursues Facility Management Controlling Multi-Vendor Cleaning Robots on One Screen

Intelligent robot service company Clobot announced on November 17 that it signed a cleaning-robot dealership agreement with Pudu Robotics. The compact-space ET1, multifunctional CC1 and CC1 Pro, large-area dry-cleaning MT1 and MT1 Max, and large-scale BG1 have been added to its existing LionsBot R3 and R5 models. Customers can select products suited to spatial characteristics and cleaning purposes across shopping malls, hospitals, airports, and logistics facilities.

Its in-house platform, CROMS-FM, monitors and controls cleaning robots from different manufacturers on a single screen and links with elevators and automatic doors so robots can move across floors and zones. When trash or dirt is detected during patrol, it requests action from cleaning staff. Automated cleaning-task assignment and mobile complaint-handling features are planned for future provision.

Korean Robotics

### 16Airbility Selected for Project to Develop Mothership-Subdrone Rescue System to Locate People in Distress

Aviation company Airbility announced on November 17 that it was selected for the Ministry of SMEs and Startups' Deep-Tech Challenge Project to develop an AI-based disaster-response drone system. Over 36 months from July 2026 to June 2029, 5 billion won in government R&D funding will be invested, with Incheon Technopark and geospatial AI company Meissa participating jointly.

When an electric vertical takeoff and landing (eVTOL) mothership carries compact sub-drones and releases them midair, the mothership handles wide-area surveillance and communications relay while the sub-drones search for people in distress using thermal imaging cameras and onboard AI. The development goal is to take off within three minutes of a report and reach a site 10 km away within 10 minutes. For its first field demonstration, the team plans to conduct 20 maritime search operations for people in distress off Jawoldo Island in Incheon.

Download PDF · 17 p.

KIMKJ.COM
