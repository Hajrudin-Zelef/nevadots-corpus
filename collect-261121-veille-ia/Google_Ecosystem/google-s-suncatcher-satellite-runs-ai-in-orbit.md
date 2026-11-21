---
id: collect-261121-veille-ia/google-ecosystem/google-s-suncatcher-satellite-runs-ai-in-orbit
title: "Google's Suncatcher Satellite Runs AI in Orbit"
domain: google-ecosystem
role: reference
task: article
actors: ["Google"]
dates: ["2026-11-03"]
keywords: ["google", "satellite", "suncatcher", "more", "orbit", "first", "data", "satellites"]
source: https://www.technology.org/2026/11/03/google-suncatcher-satellite-gemma-4-orbit-tpu/
source_anchor: ""
source_lines: [1, 37]
section: "Google_Ecosystem"
---
## An AI Chip Working in Orbit

Google’s first orbiting data center can power one of its four AI chips for only about 20 minutes at a stretch, and engineers used part of that window to ask for poetry. The Project Suncatcher prototype satellite answered its first prompt from orbit after about three weeks of commissioning, running Google DeepMind’s Gemma 4 model on a data center class Tensor Processing Unit and sending a short haiku about life in space back to Earth.

### **Key Takeaways**

- The Suncatcher prototype carries four Google TPUs and generates up to 1 kW of power, enough to run one TPU for about 20 minutes at a time.
- Its first prompt asked the onboard Gemma 4 model for a haiku about being in space, and Google says the satellite is operating nominally.
- Google plans two more satellites in 2027 with more power and laser links between spacecraft.

## From Launch to First Prompt

The prototype launched on October 1 aboard SpaceX’s Transporter-18 rideshare mission on a Falcon 9 rocket, alongside more than 100 other spacecraft. Google built it in partnership with satellite operator Planet.

The team spent roughly three weeks testing the satellite’s subsystems, including powering up the TPU trays and running early machine learning tests. During a ground-station pass, engineers uplinked the first prompt. The three-line poem came back as the satellite flew over Svalbard, Norway.

The engineers followed up with space-themed limericks. More usefully, they also asked the spacecraft to report on conditions around the satellite.

## The Real Test Starts Now

The poetry was a demonstration. The next phase checks how the chips cope with thermal stress and space radiation.

Radiation can cause bit flips, which change values stored in memory and can corrupt data without triggering normal monitoring tools. Google has tested its TPUs in particle accelerators on Earth, and those results suggested radiation matters little for inference but more for training models. Orbit will show whether the lab results hold.

If the chips perform, Google will launch two more TPU satellites in 2027. They will generate more power and test optical links between satellites.

## Close Formation, Shared Work

Suncatcher stands apart from most orbital data center plans because it aims to fly satellites close together and link them with high-speed lasers, so they can share large workloads. SpaceX’s planned data centers are also expected to use laser links but orbit too far apart to split processing effectively. Startups such as Starcloud and Cowboy Space plan to run inference on single satellites.

## A Moonshot Tied to Starship

Google is designing Suncatcher to be economical in the 2030s, which makes launch prices the deciding variable. The idea, first unveiled in late 2025, depends on fully and rapidly reusable heavy rockets. In a peer-reviewed analysis published this month, Suncatcher researchers estimated SpaceX would need to fly Starship 1,800 times to repeat the learning-driven cost cuts it achieved with earlier vehicles. Alphabet also owns a significant stake in SpaceX, and the two companies’ plans for orbital computing are closely linked.

Skeptics still say space data centers remain far from practical, and this single satellite is a laboratory, not a server farm. Google described the mission as a hardware test from the start, and its October 1 launch opened what is meant to be a year-long experiment. The pressure on Google’s core business is more immediate: Alphabet’s stock fell sharply this week after the company said adding AI to search is slowing ad revenue growth.

*Written by Alius Noreika*
