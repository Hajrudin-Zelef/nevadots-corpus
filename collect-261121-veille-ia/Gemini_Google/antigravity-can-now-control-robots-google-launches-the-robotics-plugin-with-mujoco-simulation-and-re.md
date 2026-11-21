---
id: collect-261121-veille-ia/gemini-google/antigravity-can-now-control-robots-google-launches-the-robotics-plugin-with-mujoco-simulation-and-re
title: "Antigravity Can Now Control Robots: Google Launches the Robotics Plugin With MuJoCo Simulation and Real-World Pick-and-Place | DigIntoAI"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["google", "antigravity", "robotics", "plugin", "gemini", "mujoco", "simulation", "control"]
source: https://digintoai.com/en/articles/antigravity-can-now-control-robots-google-launches-the-robotics-plugin-with-mujo-32c5d79817a4
source_anchor: ""
source_lines: [1, 21]
section: "Gemini_Google"
---
On November 17, 2026, Google launched the **Robotics plugin** for **Google Antigravity**. Paired with **Gemini 4 Hydrogen** (API model ID: **gemini-4-hydrogen**; see our Gemini 4 Hydrogen launch report), the new plugin packages hardware onboarding, MuJoCo simulation, spatial perception, and robot arm control into agentic skills that turn Antigravity into a unified Real-to-Sim-to-Real robotics workbench.

## **Four Core Capabilities in the Robotics Plugin**

Developers can install the plugin inside Google Antigravity by opening **Customizations** and searching for **Robotics** on the **Build With Google** shelf. Its built-in skills span four stages:

**Robot Bring-Up: **Starting from a USB connection to a laptop, the plugin installs drivers, discovers external depth cameras, calibrates the arm, and verifies safe motion. Google reports that an I2RT YAM arm calibrates in roughly 10 minutes, with identical onboarding flows for the low-cost SO-ARM101 (SO-101) and industrial dual-arm FR3 Duo.

**MuJoCo in Antigravity: **Builds, audits, and renders physics-accurate 3D scenes and assets directly inside Antigravity from text descriptions, links, documents, or workspace photos, enabling "vibe sim" tuning of shapes, positions, and physical properties before touching physical hardware.

**Perception and Control: **Uses Gemini 4 Hydrogen's spatial and embodied reasoning to identify objects, plan grasps, and control robot arms in a closed loop from live camera views.

**End-to-End Application Building: **Generates controller and simulation code to streamline Real-to-Sim-to-Real deployment in one environment.

Image source: Conceptual workflow diagram created by DigIntoAI based on the Google Developers Blog post.

## **Snack-Sorting Conveyor Demo, Benchmarks, and Access**

**From Whiteboard Sketch to Conveyor Snack Sorting in About an Hour: **In Google's primary demonstration, a developer feeds a whiteboard sketch of a snack-sorting conveyor and real scene photos into Antigravity. Gemini 4 Hydrogen generates an interactive MuJoCo simulation to validate the grasping policy before deploying directly to physical hardware, iterating on gripper closure force and sorting rules in natural language until the arm reliably sorts snacks off the moving belt within roughly one hour. Additional demos include a dual-arm YAM Pro storefront fulfillment setup packing soap bars with a printed thank-you note and an FR3 Duo playing chess.

**Benchmarks and Availability: **On the Artificial Analysis Robotics 0.1 benchmark covering eight tabletop tasks on a physical 6-axis arm, Gemini 4 Hydrogen scores **58**, matching Gemini 4 Argon at a lower per-token cost, and outperforms Gemini Robotics ER 2 across spatial reasoning tasks on the public ERQA benchmark. Google Antigravity is available to paid **Google AI Pro or Ultra** subscribers, while **gemini-4-hydrogen** is also accessible via the Gemini API (Interactions API and google-genai SDKs) and Google AI Studio.
