---
id: collect-261121-veille-ia/gemini-google/google-antigravity-robotics-plugin-simulation-first-neoteo
title: "Google Antigravity robotics plugin: simulation first | NeoTeo"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-11-17"]
keywords: ["google", "robot", "plugin", "skills", "robotics", "simulation", "repository", "antigravity"]
source: https://www.neoteo.com/en/googles-antigravity-robotics-plugin-lets-developers-start-in-simulation
source_anchor: ""
source_lines: [1, 28]
section: "Gemini_Google"
---
Google announced its Robotics plugin for Antigravity on November 17, 2026, and said it was launching that day. The plugin brings together agent skills for robot setup, simulation, perception, control and application development. Developers can also explore simulation workflows without owning or connecting a physical robot, according to Google DeepMind’s repository.

## What does the Antigravity Robotics plugin do?

Google describes the plugin as a set of skills for working through robotics tasks with Gemini 4 Hydrogen. They cover getting a robot set up, creating and testing simulations in MuJoCo, perception and control, and building robotics applications. Perception here means interpreting information such as camera input; control is the software’s direction of a robot’s actions. Google outlines these capabilities in its announcement.

## Can you try the workflow without a robot?

Yes. The repository describes simulation-only workflows that do not require robot hardware. That gives developers a way to work with a simulated task before moving to a physical setup.

The repository documents several installation routes, but they do not all bundle the same MuJoCo tools:

- **Antigravity Marketplace:** Open Customizations, go to Marketplace, then look for Robotics on the Build With Google shelf. This installation includes seven MuJoCo API skills.
- **GitHub through the command line:** Run`agy plugin install https://github.com/google-deepmind/robotics-antigravity-plugin` . The repository says to install MuJoCo skills separately for this route.
- **Manual installation:** Clone the repository into`~/.gemini/config/plugins` . MuJoCo skills need to be installed separately.
- **Using npx:** Run`npx skills add google-deepmind/robotics-antigravity-plugin` . The repository likewise instructs GitHub-based installers to add MuJoCo skills separately.

Google says using the plugin in Antigravity requires a paid Google AI Pro or Ultra subscription. Only skills that call Gemini perception models require a `GEMINI_API_KEY`.

A simulated task can help you work through a development workflow before connecting hardware. Google’s screen recording shows an SO-ARM101 simulation stacking and unstacking blocks in MuJoCo Studio.

Use simulation to rehearse the software workflow; keep direct safety precautions in place when a real robot is moving.

## What should you keep in mind before using a real robot?

Simulation and physical operation are different stages. The repository says users are responsible for safe robot operation: stay beside a moving robot, clear the workspace and keep a way to cut power within reach.

The repository is licensed under Apache License 2.0. Some skills fetch third-party software or robot models, or call third-party services; those components remain subject to their owners’ terms.
