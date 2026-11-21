---
id: collect-261121-veille-ia/gemini-google/google-deepmind-ships-genie-4-with-mid-session-scene-edits-ai-weekly
title: "Google DeepMind Ships Genie 4 With Mid-Session Scene Edits | AI Weekly"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["genie", "google", "scene", "world", "post", "deepmind", "gemini", "audio"]
source: https://aiweekly.co/alerts/google-deepmind-ships-genie-4-with-mid-session-scene-edits
source_anchor: ""
source_lines: [1, 48]
section: "Gemini_Google"
---
## TL;DR

- TechCrunch confirmed AMD acquired World Labs and Amazon is pursuing Decart, placing Genie 4 inside a rapidly consolidating world-model sector with no independent players left at scale.
- The DeepMind blog names Gemini 3.8 Flash as Genie 4's planning layer, revealing a hybrid architecture where the language model directs narrative and Genie renders pixels in real time.
- Unite.ai's API breakdown prices interactive video output at $17.50 per million tokens (roughly $0.10 per second), setting a hard cost floor that limits mass-deployment scenarios.

Google DeepMind let subscribers edit a generated world while it runs. In a launch post on blog.google, the team describes Genie 4 as "our most realistic and interactive world model yet," one that lets a user "prompt mid-session to change the world you created."

The pitch, in the post's own words: "Shift the weather, drop in a character, or redirect the action." Genie 4 also pairs the visuals with "real-time native audio that adapts as the scene unfolds," from ambient soundscapes to character dialogue.

Alongside Genie 4, Google is pushing updates into Genie 3. Sessions now run "up to 3 minutes per generation (up from 60 seconds)," and users can link worlds together for longer journeys or seed a scene from a 360-degree panorama image. A new Project Genie agent conducts the build conversationally. "Now crafting an interactive universe is as effortless as having a conversation," the post reads, and the agent can call Gemini models to surface context-aware suggestions mid-scene.

Access is tiered. Eligible Google AI Ultra subscribers get Project Genie starting today; AI Pro members can join a waitlist. Published creations land in a community gallery, alongside collaborations the team highlights with BLVCKL!GHT ("Villa Giallo") and filmmaker Anand Gandhi ("The Life of Ants"). All generated video and audio carry SynthID watermarking, and the post lists known limitations around camera latency, complex physical interactions, audio precision, and scene permanence.

Genie 4 lands into a dense stretch for generative video on our video-generation tracker, which logged 55 stories across the prior 90 days.

## What others are reporting

1. 
                
                                  First-party technical post names Gemini 3.8 Flash as the guiding agent and details the hybrid architecture: Gemini plans narrative while Genie renders pixels and audio simultaneously, pixel by pixel. Genie 4 simulates what happens next: how the world responds, how it sounds, and how the people in it move, react, and speak.
2. 
                
                                  Provides the competitive consolidation context absent from the launch post: AMD acquiring World Labs, Amazon pursuing Decart, and Rivian buying Third Dimension as the world-model sector clusters. What we'll increasingly see in the future are these hybrid world models, where you have a combination of Gemini writing code and text, and Genie ingesting that in real time.
3. 
                
                                  Developer-side breakdown: $17.50 per million tokens for video output, 120-second hard session cap, 720p at 24fps, and billing that continues if a browser tab closes without an explicit complete() call. Sessions cap at 120 seconds because the model renders over a rolling window without a persistent 3D scene.
4. 
                
                                  Financial press framing centers Google's own candid admission of active engineering gaps alongside the SynthID watermarking requirement applied to all outputs. We are actively tackling key challenges such as camera latency, intricate physical interactions, real-time audio precision, and long-term scene permanence.
5. 
                
                                  Categorizes Genie 4 as 'draft mode' prototyping infrastructure, notes the absence of published benchmarks at launch, and details specific failure modes including object permanence drift and text legibility. Characters move naturally, respond to their surroundings, and can be steered interactively while the scene plays.
6. 
                
                                  Includes a Genie 3 vs. Genie 4 comparison table, flags the 99.99 euro per month Ultra price for European subscribers, and advises against adoption for anyone not already paying that rate. You no longer have to regenerate a world if something doesn't suit you — simply type in a new prompt while exploring.
7. 
                
                                  XR-industry outlet highlights trusted-tester use in filmmaking pre-visualization and archival photojournalism, framing Genie 4 as creative-industry infrastructure with use cases beyond gaming. Users can set an event in motion and Genie 4 simulates what happens next: how the world responds, how it sounds, and how the people in it move, react, and speak.
8. 
                
                                  Focuses on the commercial rollout arc from internal research preview to paid Ultra subscriber access, framing the staged release as deliberate product sequencing rather than a broad consumer launch. Users can change scenes while they are running; audio and longer sessions are also part of the update.

          Originally reported by
          **blog.google**
        

Original headline: Google Launches Genie 4 World Model With Real-Time Scene Edits and 3-Minute Sessions
