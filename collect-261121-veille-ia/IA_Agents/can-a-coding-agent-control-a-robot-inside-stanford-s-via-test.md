---
id: collect-261121-veille-ia/ia-agents/can-a-coding-agent-control-a-robot-inside-stanford-s-via-test
title: "Can a Coding Agent Control a Robot? Inside Stanford's VIA Test"
domain: ia-agents
role: reference
task: article
actors: []
dates: []
keywords: ["agent", "robot", "robotics", "coding", "model", "general", "stanford", "same"]
source: https://www.mindstudio.ai/blog/coding-agents-as-robot-brains
source_anchor: ""
source_lines: [1, 87]
section: "IA_Agents"
---
## What did Stanford’s VIA experiment actually show?

Stanford researchers connected a general-purpose coding agent, Codex Astra 6, to a commercial Stretch 4 robot through a visual interface, with no robot-specific training added to the model. Across five physical tasks and 25 trials, the agent succeeded 24 times. The system is called VIA, short for Visual Interface Agent. The agent looked at a camera feed, placed a virtual marker showing where it wanted the robot’s gripper to go, adjusted that target across multiple views until it looked right, then told a separate controller to move the real arm there. After the move, it checked the camera again to see whether the action worked and decided what to do next. That’s the whole loop: look, plan a target, act, check, repeat.

## TL;DR

- Stanford’s **VIA system** paired an unmodified coding agent with a Stretch 4 robot and succeeded in 24 of 25 trials across five tasks, with zero added robot-specific training.
- The agent didn’t control individual motors. It chose **spatial targets** for a gripper (where to go, in view after view) and let a separate controller handle the actual arm movement.
- The result lines up with Richard Sutton’s **Bitter Lesson** : general methods that scale with computation tend to eventually beat hand-built, domain-specific systems.
- OpenAI’s decision to fold its robotics research into its main research org, with applied robotics and hardware moving to a separate applied group, reflects the same bet that **general frontier models** are becoming directly useful for physical tasks.
- A separate study (referred to as RUA) found that keeping the same underlying model but improving the **software harness** around it substantially boosted simulation performance, meaning connective tooling matters as much as the model itself.
- The biggest unsolved problem isn’t intelligence, it’s **speed** : one study found a robot spent roughly 90% of its time on model deliberation, with full manipulation runs taking over an hour.
- The article’s framing suggests 2027 could be a meaningful year for robotics progress, but only for teams that pair capable general models with harnesses fast enough for real-world timing.

*Enterprises have Remy.*

**⌂**Self-hosted

**✓**SOC 2

**✚**HIPAA

**☰**Audit log

## Why does the Bitter Lesson matter for robotics?

Richard Sutton’s 2019 essay “The Bitter Lesson” argued that AI progress follows a repeating pattern. Researchers study a hard problem, build in their domain expertise, and get something that works for a while. Then a more general method, one that keeps improving simply by adding more compute and more data, comes along and eventually outperforms the carefully engineered system. Sutton traced this through chess, Go, speech recognition, and computer vision.

Robotics looks like a strong candidate for the same pattern. A lot of what makes physical manipulation hard also shows up in other AI work: understanding a messy visual scene, deciding on an action, noticing when something didn’t go as planned, and adjusting. Those are the same skills that make an agent good at using a computer, reading a screen, clicking the right button, and recovering from an error. If those general skills transfer to operating a robot arm, then every improvement made to a model for unrelated reasons, like making it better at coding or browsing the web, potentially makes it better at robotics too, without anyone on a robotics team doing extra work.

That doesn’t mean specialized robot models are obsolete. A model trained on enormous amounts of physical interaction data can also follow the Bitter Lesson, by scaling its own specialized data and compute. The open question is how much robot-specific engineering is actually necessary once general models get good enough to handle much of the same job out of the box.

## How did the agent plan its movements without robot training?

The mechanism Stanford used is worth breaking down because it explains why “no training” doesn’t mean “no skill.” The agent was given a visual interface: it could see the scene through a camera and place a marker representing where it wanted the gripper to end up. It could check that marker against multiple camera angles, nudging it if the position looked off (too high, too far forward) before committing to a real-world move. Only after refining the target would it instruct the controller to actually move the arm.

Critically, the agent never decided how to move each joint or motor. That mechanical execution was handled by the Stretch robot’s own controller. The agent’s job was choosing where the gripper needed to go and verifying what happened afterward. This division of labor mirrors how a person doesn’t consciously calculate muscle movements to reach for a cup, they just aim and reach. It also mirrors how coding agents already operate when using software: look at the screen, pick an action, check the result, decide the next step. In VIA’s case, the “tool” being operated is a physical gripper instead of a mouse cursor, and the feedback is a camera view instead of a screenshot.

### Built like a system. Not vibe-coded.

Remy manages the project — every layer architected, not stitched together at the last second.

One example from the research illustrates this adaptability. During a table-wiping task, the robot’s own gripper blocked its camera view, so the agent moved the gripper aside to check whether the surface was actually clean. In another attempt, when holding the wipe wasn’t working, the agent abandoned that approach and simply pushed the wipe across the table with its closed gripper. That kind of on-the-fly adjustment, trying something, checking if it worked, and switching strategy, is exactly the behavior the Bitter Lesson predicts from general models: nobody programmed that specific fallback, the agent worked it out from its existing general capabilities.

## Why is OpenAI reorganizing its robotics team around this idea?

OpenAI confirmed moving its robotics researchers into its main research organization, while applied robotics and hardware work moved into its applied products group. The company’s stated reasoning is that general frontier models are becoming more directly useful for robotics work. Financial pressures may be part of the full picture, but the technical logic tracks with the Bitter Lesson argument: if people improving the general-purpose model are, as a side effect, also improving its ability to operate a robot, then the robotics team benefits from company-wide model gains without needing every advance to originate in a dedicated robotics effort. Instead of a relatively small team of robotics specialists carrying all the weight, the entire research organization indirectly contributes anytime it pushes general capabilities like visual reasoning, tool use, and error recovery.

## Is a good model enough, or does the harness matter too?

A capable model alone isn’t sufficient. A separate study, referred to as RUA and conducted in November, kept the underlying model fixed and substantially improved simulation scores purely by improving the software harness surrounding it, the plumbing that governs how the model receives information and issues commands. This indicates that connecting a general model well, giving it the right visual information, the right action space, the right feedback loop, is itself a significant engineering problem, separate from how capable the underlying model is. A brilliant model with a poor interface will underperform a decent model with a well-tuned harness.

## What’s still standing in the way of real-world robots?

Speed. A separate study found that in trials with a G1 humanoid robot, around 90% of total task time went to the model simply thinking, and successful manipulation attempts took over an hour to complete. That’s a very different setup from Stanford’s VIA experiment, but it highlights a problem that exists regardless of which model is doing the reasoning: physical environments don’t pause while a model deliberates. An object sliding off a table, a part shifting on an assembly line, these situations don’t wait for inference time. A controller needs to keep responding to a changing scene in real time, which means the system has to be fast enough for the task, not just eventually correct.

Speed also determines whether automating a task is worthwhile at all. A robot that completes a job correctly but requires a person standing by for long stretches of supervision may cost more attention than it saves. That tradeoff, not raw task success, is likely to decide which robotics applications become practical first.

## Frequently Asked Questions

### What is Stanford’s VIA system?

VIA (Visual Interface Agent) is an experimental setup where a general-purpose coding agent, given only a camera feed and a way to specify gripper targets, controlled a commercial Stretch 4 robot without any robot-specific training, succeeding in 24 of 25 trials across five tasks.

### What is the “Bitter Lesson” and why does it apply to robotics?

### Everyone else built a construction worker.

We built the contractor.

    One file at a time.

UI, API, database, deploy.

It’s an argument from researcher Richard Sutton that general, scalable AI methods eventually outperform systems built on hand-coded domain expertise. In robotics, this suggests that improvements made to general models for other purposes, like visual reasoning or tool use, may transfer directly to physical tasks without dedicated robotics training.

### Does this mean specialized robot models are no longer needed?

Not necessarily. Models trained specifically on large volumes of physical interaction data can also benefit from scaling. The open question is how much robot-specific engineering remains necessary once general models already handle much of the task.

### Why did OpenAI reorganize its robotics research?

OpenAI moved its robotics researchers into its main research group while applied robotics and hardware shifted to its applied organization, citing the growing usefulness of general frontier models for robotics work, alongside other organizational and financial factors.

### What’s the biggest obstacle to practical robots right now?

Speed. Research cited in this piece found that a model’s “thinking” time can dominate a robot’s task duration, with one trial spending roughly 90% of total time on deliberation. Real-world environments change continuously, so a system also needs to respond fast enough to be useful, not just produce correct decisions eventually.
