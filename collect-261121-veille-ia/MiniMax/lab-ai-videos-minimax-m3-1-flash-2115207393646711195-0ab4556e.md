---
id: collect-261121-veille-ia/minimax/lab-ai-videos-minimax-m3-1-flash-2115207393646711195-0ab4556e
title: "Lab ai videos minimax m3 1 flash 2115207393646711195"
domain: minimax
role: reference
task: article
actors: ["MiniMax"]
dates: []
keywords: ["flash", "minimax", "shock", "gpt-6", "astra", "deepseek", "development", "opus"]
source: ""
source_anchor: ""
source_lines: [1, 47]
section: "MiniMax"
---
The original post
Hi everyone! This time, I compared Claude Opus 5.5, GPT-6 Astra, DeepSeek V4.1 Flash 1017, and MiniMax M3.1 Flash in a challenging HYPERSONIC SHOCK LAB — Computational Fluid Dynamics (CFD) Benchmark. 🚀🔥
Unlike my previous single-shot comparisons, I used a budget-constrained, iterative development protocol to evaluate not only the models' ability to implement complex physics simulations, but also their engineering accuracy, numerical stability, iterative improvement capabilities, development speed, and API costs.
Benchmark Setup:
I used Pi Harness for all four models, with the following conditions:
- 60-minute development limit and $10 API budget per model.
- High effort for Opus 5.5 and GPT-6 Astra.
- Max effort for DeepSeek V4.1 Flash 1017 and MiniMax M3.1 Flash.
- Four independent GPT-6 Luna / xhigh supervisors managed the development workflow using fixed instructions, without contributing solutions or code.
- The workflow followed: Analysis & Planning → Implementation → Review → Repair Planning → Repair → Testing → Fix & Retest.
- Review and repair cycles continued as long as time and budget remained.
All models received the same task, initial files, and tools. Planning, coding, testing, and orchestration delays counted toward the 60-minute limit. I preserved the final artifacts exactly as they stood when development stopped.
Task Summary:
I asked the models to develop a fully autonomous, visually compelling 75-second hypersonic wind-tunnel simulation, implemented as a single offline HTML file using JavaScript and Canvas 2D. The challenge required a genuine 2D conservative finite-volume compressible Euler solver, approximate Riemann fluxes, ideal-gas equations of state, CFL-controlled numerical integration, physically computed shock waves, and approximately 160 × 64 fluid cells.
The simulation consisted of five consecutive phases:
- P1 — Base Flow: Maintain a stable, uniform Mach 5 flow without numerical divergence.
- P2 — Oblique Shock: Generate a physically computed shock from a 12° compression ramp and validate its angle and pressure ratio against analytical shock relations.
- P3 — Expansion Fan: Simulate Prandtl–Meyer expansion, verifying increasing Mach number and decreasing pressure.
- P4 — Shock Interaction: Introduce an opposing 8° compression ramp and resolve the interaction between two shock structures.
- P5 — Mach Sweep: Increase inlet Mach from 5 to 6 and verify that the shock angle approaches its new analytical solution.
Overall Physics Scores /100, API Costs, and Development Times:
🥇 Opus 5.5 · High — 100/100 — $10.233 — 22:06
🥇 GPT-6 Astra · High — 100/100 — $10.146 — 29:38
🥇 DeepSeek V4.1 Flash 1017 · Max — 100/100 — $0.671 — 60:00
- MiniMax M3.1 Flash · Max — 0/100 — $0.279 — 22:48
Results:
🥇 Best initial oblique-shock accuracy — Opus 5.5, with only 0.0414° angular error and 0.1047% pressure-ratio error.
🥇 Best post-expansion Mach accuracy — GPT-6 Astra, with only 1.2287% relative error.
🥇 Best final shock-angle convergence accuracy — GPT-6 Astra, with a 0.0373° error.
— Shortest recorded development time among passing submissions — Opus 5.5, at 22 minutes and 6 seconds.
— Best API cost efficiency among passing submissions — DeepSeek V4.1 Flash 1017, achieving 100/100 for just $0.671.
— Most completed iterative improvement cycles — DeepSeek V4.1 Flash 1017, completing five full review, repair-planning, repair, test, and retest cycles.
— Strongest reviewed solver and measurement implementation — GPT-6 Astra, with no material defect identified in the examined numerical paths.
— Deterministic physics reproduction — Opus, Astra, and DeepSeek, all reproducing their measured results across restart testing.
— Most critical numerical stability failure — MiniMax M3.1 Flash, which produced an invalid simulation and received 0/100.
— Lowest overall API expenditure — MiniMax M3.1 Flash ($0.279), but without a valid benchmark submission.
— Best cost-to-physics-acceptance ratio — DeepSeek V4.1 Flash 1017, …
The author did not share the exact prompt. The post text is a good starting point; the replies on X often have more.
Try this prompt on SandBase
Run the same prompt against this model, live, and see what you get. You pay the model price for what you run.
- Start from this postThe post text opens in the Text arena with MiniMax M3.1 Flash selected, asking for a single HTML file.
- Get the codeMiniMax M3.1 Flash writes the animation as HTML, canvas or WebGL in one answer.
- Play it in a browserSave the answer as an .html file and open it. Ask for changes in the same chat.
A run costs a few cents of model time. Runs on: minimax/minimax-m3.1-flash
More tests & comparisons made with MiniMax M3.1 Flash
Make your own with an Agent
One prompt or one photo in, a finished video out. Agents on SandBase plan the shots, call the models and check the result.
