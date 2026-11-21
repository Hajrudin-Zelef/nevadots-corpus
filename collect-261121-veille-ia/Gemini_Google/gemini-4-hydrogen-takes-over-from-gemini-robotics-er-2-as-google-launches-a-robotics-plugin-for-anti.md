---
id: collect-261121-veille-ia/gemini-google/gemini-4-hydrogen-takes-over-from-gemini-robotics-er-2-as-google-launches-a-robotics-plugin-for-anti
title: "Gemini 4 Hydrogen Takes Over From Gemini Robotics ER 2 as Google Launches a Robotics Plugin for Antigravity"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["robotics", "gemini", "google", "hydrogen", "model", "antigravity", "plugin", "takes"]
source: https://www.humanoidsdaily.com/news/gemini-4-hydrogen-robotics-antigravity-plugin
source_anchor: ""
source_lines: [1, 105]
section: "Gemini_Google"
---
Supported by RoboStrategySupport for Humanoids Daily comes from RoboStrategy

Sponsored
Investing involves risk, including possible loss of principal. Read the prospectus before investing.

Google on Tuesday published a robotics write-up for **Gemini 4 Hydrogen**, the faster, cheaper model it added to the Gemini 4 line on November 12, and released a Robotics plugin for its Antigravity coding environment. In the post on the Google Developers Blog, the Gemini Robotics team says Hydrogen takes over the job of Gemini Robotics ER 2, the embodied reasoning model DeepMind released in July, and scores higher than it on nearly every measure Google reports.

- Gemini 4 Hydrogen averages 84.8 across 15 embodied-reasoning tests in Google's comparison, against 75.5 for Gemini Robotics ER 2. Google says Hydrogen brings the ER models' skills into the main Gemini 4 series.
- On Artificial Analysis's AA-Robotics benchmark, where a model controls a real 6-axis arm through eight tabletop tasks, Google reports a score of 58. Gemini 4 Argon scored 55.3 in October and GPT-6 Astra 69.4.
- A new open-source Robotics plugin for Google Antigravity handles arm bring-up over USB, builds MuJoCo simulations from chat, and runs the same controller code in simulation and on hardware. Supported arms include the I2RT YAM, the LeRobot SO-101 and the Franka FR3 Duo.
- Hydrogen costs $1.50 per million input tokens and $7.50 per million output tokens in the Gemini API. In Antigravity it is open to AI Pro subscribers, where Argon was limited to Ultra.

The release continues a run of Gemini 4 robotics work that we covered in our look at how frontier language models are speeding up robotics research. There, the Google examples ran on Argon, the larger model: it taught Apptronik's Apollo a cornhole throw from human video and helped develop Gemini Robotics 2.5 in four months. Hydrogen moves the same kind of agentic workflow, where a model writes and runs robot code from a plain-language goal, to a cheaper tier.

Carolina Parada, VP and head of robotics at Google DeepMind, called Hydrogen the company's best model for "plug-in your robot and prompt it" robotics. She wrote that a robot arm can be connected over USB in 10 minutes, and that the team built a snack-sorting conveyor demo in about an hour.

## ER skills move into the main Gemini line

Gemini Robotics ER 2 shipped in July as one of three models in the Gemini Robotics 2 release, alongside the main vision-language-action model and an On-Device version. The ER model handles perception and planning: finding objects, pointing at them, breaking a task into steps and judging whether a step worked. At the Mercedes-Benz pilot announced last week, an ER model tracked the kitting workflow while Gemini Robotics 2.5 moved Apollo 2's arms and hands.

## Follow humanoid robotics without following every announcement.

Get the week’s most important launches, funding and research—and why they matter—in one email from Humanoids Daily.

Once a week. Unsubscribe anytime.

Read recent issues
Google's comparison chart puts the two models side by side, both at high thinking, on 15 tests: ERQA, safety, sight glass and gauge reading, visual moments, multi-pointing, steerable pointing, semantic keypoints, picked-object recognition, task breakdown, error detection, success detection, moment finding, hindsight labeling and temporal segmentation. Hydrogen averages 84.8 and ER 2 averages 75.5. The chart shows Hydrogen ahead on 13 of the 15, roughly level on multi-pointing, and behind on steerable pointing. The largest gaps are on picked-object recognition and hindsight labeling. The post singles out multi-view correspondence and 3D trajectory reasoning as areas where Hydrogen beats ER 2, but it does not publish the underlying scores.

Google does not say whether a standalone ER 3 is planned, or whether partners using ER 2 inside their own stacks should switch to Hydrogen.

## Controlling an arm directly

In its robotics post, Google describes Hydrogen as a model that, given a goal and a camera view, "plans the task, writes and runs the control code, and keeps adjusting until the job is done."

The benchmark Google uses for that is AA-Robotics, which Artificial Analysis launched as a research preview on October 14. A model runs zero-shot inside a coding-agent harness, with an HTTP API to a Hiwonder 6-axis arm, three table cameras and one wrist camera. It attempts eight tasks: pressing a red button, placing a toy crocodile in a rectangle, pouring a drink, tidying blocks into a box, a shape-matching puzzle, writing "hello world", building the tallest tower and measuring a box to within 2 mm. A human judge scores each attempt out of 100, with partial credit.

Google reports a score of 58 for Hydrogen and calls it similar to Argon "at a lower cost per token." The October leaderboard, which does not yet list Hydrogen, has:

| Model | AA-Robotics score | 
|---|---|
| GPT-6 Astra | 69.4 | 
| GPT-6.1 Sol | 64.6 | 
| Gemini 4 Hydrogen (reported by Google) | 58 | 
| Gemini 4 Argon | 55.3 | 
| Claude Opus 5.5 | 48.8 | 
| Claude Fable 5.1 | 46.3 | 

No model in the October results passed the box-measuring task, and Argon scored zero on shape matching. Artificial Analysis also logged an average of 52 minutes per task for Argon, against a human baseline of about 20 seconds. Google's Hydrogen post gives no per-task breakdown or task times.

Hydrogen's API price is $1.50 per million input tokens and $7.50 per million output tokens, flat across its 1-million-token context window. Google's launch post on November 12 placed it below Argon and Gemini 4 Carbon as a "daily driver" for agentic work, and said it finishes tasks 2.5 times faster than Gemini 3.8 Flash.

Anthropic's Claude Fable 5.2, released November 9, has not been scored on AA-Robotics. On RoboDojo Sim, a different benchmark, Anthropic reported 64.9% for Fable 5.2 and 34.2% for Argon in its own runs.

## The Antigravity Robotics plugin

The plugin, published on GitHub under the Apache 2.0 license, adds a set of skills to Antigravity, Google's agentic coding environment. Google calls the result "a one-stop robotics workbench." The skills cover three jobs:

- **Bring-up:** installing drivers, finding cameras, calibrating the arm and checking that it moves safely, starting from a USB cable. Google says an I2RT YAM arm with an external depth camera, plugged into a laptop, was calibrated and ready for tasks in about 10 minutes.
- **Simulation:** building and previewing MuJoCo scenes inside Antigravity, from text descriptions, links, documents or photos of the workspace. Google calls adjusting shapes, positions and physical properties through chat "vibe sim", and uses it to check reaching and grasping before running on hardware. The plugin bundles seven MuJoCo skills, from the Python API to rendering and MuJoCo Studio.
- **Control:** the agent writes a controller, tests it in MuJoCo and runs the same code on the real arm. It can also work in simulation only, with no hardware attached.

The README names the I2RT YAM and YAM Pro, the low-cost SO-101 from Hugging Face's LeRobot project, and the Franka Research 3. Google's sample prompts include "Bring up my SO-101 arm and make it wave" and "Stack snacks on top of each other." The README also carries a safety warning: users should stay next to the robot, keep the workspace clear and keep a way to cut power within reach.

Google showed three demos built with the plugin:

- **Snack-sorting conveyor:** starting from a whiteboard sketch, Hydrogen built a MuJoCo simulation of the cell, tested the grasping policy over several runs and deployed it to a real arm. Google says the arm "reliably sorts snacks off the moving belt" after about an hour of work end to end. It gives no success rate.
- **Soap store:** a web storefront connected to two YAM Pro arms, which pack mint and citrus soap bars and print a thank-you note for each order.
- **Chess:** a Franka FR3 Duo setup that simulates a chess board and plays against itself or a person, built "after a few prompts."

## Access and price

Hydrogen is available as `gemini-4-hydrogen` in the Gemini API, with day-one support in the `google-genai` SDKs, and in Google AI Studio, where developers can try spatial prompts and live camera streams in the browser. In Antigravity it requires a paid Google AI Pro or Ultra subscription. Argon's robotics features launched in October for Ultra subscribers only in AI Studio and Antigravity.

The plugin installs from the Build With Google shelf in Antigravity's Customizations menu, or from the command line. Skills that call Gemini's perception models need a Gemini API key. Google is collecting projects and prompts in the repository's discussions.

### Share this article

## Follow humanoid robotics without following every announcement.

Get the week’s most important launches, funding and research—and why they matter—in one email from Humanoids Daily.

Once a week. Unsubscribe anytime.

Read recent issues
## Read next

  - Published on
  - Reading time
  - 8 min read
  - Published on
  - Reading time
  - 5 min read
  - Published on
  - Reading time
  - 4 min read
  - Published on
  - Reading time
  - 4 min read
  - Published on
  - Reading time
  - 4 min read
  - Published on
  - Reading time
  - 7 min read
