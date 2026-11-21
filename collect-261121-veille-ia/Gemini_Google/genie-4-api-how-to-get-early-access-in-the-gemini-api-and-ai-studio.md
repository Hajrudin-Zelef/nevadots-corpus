---
id: collect-261121-veille-ia/gemini-google/genie-4-api-how-to-get-early-access-in-the-gemini-api-and-ai-studio
title: "Genie 4 API: How to Get Early Access in the Gemini API and AI Studio"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["genie", "google", "gemini", "access", "project", "early", "studio", "deepmind"]
source: https://dev.to/hassann/genie-4-api-how-to-get-early-access-in-the-gemini-api-and-ai-studio-21p2
source_anchor: ""
source_lines: [1, 161]
section: "Gemini_Google"
---
As of October 29, 2026, there is no public Genie 4 API. Google DeepMind's Genie 4, announced on October 27, 2026, is a research preview for trusted testers, and Google says it's "coming soon" to early access partners of the Gemini API and Google AI Studio, with no date, docs, model ID or price published yet.

So you have two jobs: get in line, and build the parts of a Genie 4 app that already run on the public Gemini API.

No. Google DeepMind's Genie 4 has no public API, no API reference page and no model ID in the Gemini API as of October 29, 2026. Google names "early access partners of the Gemini API and Google AI Studio" as a next group after trusted testers, but hasn't published an application form, date or price for that track.

Here's what is and isn't on the record:

| Item | Status on Oct 29, 2026 | Source | 
|---|---|---|
| Announcement date | October 27, 2026 | Google DeepMind | 
| Current access | Research preview for trusted testers | Google DeepMind | 
| Next developer group | Early access partners of the Gemini API and Google AI Studio | Google DeepMind | 
| Next consumer group | Google AI Ultra subscribers in Project Genie | Google DeepMind | 
| Public waitlist | One form, for Genie 4 in Project Genie | Waitlist form | 
| Genie page in Gemini API docs | None ( `/gemini-api/docs/genie` returns 404) | Checked Oct 29, 2026 | 
| Genie model on the models page | Not listed | Gemini models page | 
| Model ID, price, rate limits | Not announced | None | 
| Resolution, fps, session length | Not announced | None | 
| Watermark on output | SynthID on all video and audio | Google DeepMind | 

For the feature rundown, read our Google Genie 4 explainer. This post sticks to the API.

Google mentions developer access twice in the announcement. Near the top:

"We have started rolling Genie 4 out as a research preview for trusted testers, gathering feedback before we release it to consumers and developers, starting with Google AI Ultra subscribers in Project Genie and early access partners of the Gemini API and Google AI Studio."


And in the "Next steps for Genie 4" section:

"Genie 4 has been released as a research preview to trusted testers, and is coming soon to Google AI Ultra subscribers in Project Genie and early access partners in the Gemini API and Google AI Studio."


Read closely: "early access partners" is a selected group, not an open beta, the only link attached is the consumer Project Genie waitlist, and nothing gives a quarter or says "general availability."

Google also lists its own limits: environments stay coherent "over short sessions," while details and object permanence "can still shift or drift over time."

There's no developer application yet, so these steps position you for when one appears:

1. 
**Join the Genie 4 waitlist.** Click "Join the Genie 4 waitlist" on Google DeepMind's post. It opens Google's waitlist form for Project Genie. It isn't a developer program, but it's the only signal Google has asked for.
2. 
**Get a Gemini API key and a billing account.** Create a key in Google AI Studio. An active Gemini API project is a reasonable bet for partners, not a stated rule.
3. 
**Watch the Gemini API docs.** Check the models page and the release notes in the docs sidebar.
4. 
**Build on the Interactions API.** Google's Interactions API guide says "all new models, multimodal capabilities, tools, and agentic features will launch on the Interactions API." Genie 4 isn't named there, but if it lands in the Gemini API, that's the stated default.
5. 
**Ship something world-model shaped.** Google's testers come from creative industries, journalism, education, gaming and engineering.

For Project Genie's cost, see Is Project Genie free?, and for world models you can try today, Project Genie alternatives.

Genie 4 isn't a lone model. Google describes Gemini 3.8 Flash as "a guiding agent in the loop": it observes the scene, breaks a goal into steps, reviews incoming frames to track progress and confirm each step succeeded, and adapts the plan when the user steps in.

That supervisor half is public right now. `gemini-3.8-flash` is on the Gemini API, with a free tier and paid rates of $0.75 per 1M input tokens and $3.75 per 1M output tokens through December 31, 2026, per the Gemini API pricing page. You can point it at frames from any source today (a game engine, a screen recorder, a Genie 3 capture) and swap in Genie 4 frames later.

Here's a minimal supervisor in Python, based on the request shapes in Google's image understanding docs. Keep the model name in a variable so you can swap it on launch day:


```
import base64, json, os
from google import genai
MODEL = os.environ.get("SUPERVISOR_MODEL", "gemini-3.8-flash")
client = genai.Client()  # reads GEMINI_API_KEY
PLAN = {"type": "object", "properties": {
    "steps": {"type": "array", "items": {"type": "string"}}}, "required": ["steps"]}
VERDICT = {"type": "object", "properties": {
    "done": {"type": "boolean"}, "reason": {"type": "string"}}, "required": ["done", "reason"]}
def frame(path):
    with open(path, "rb") as f:
        return {"type": "image", "mime_type": "image/png",
                "data": base64.b64encode(f.read()).decode()}
def ask(text, image, schema, previous=None):
    kwargs = dict(model=MODEL, input=[{"type": "text", "text": text}, image],
                  response_format={"type": "text", "mime_type": "application/json",
                                   "schema": schema})
    if previous:
        kwargs["previous_interaction_id"] = previous
    turn = client.interactions.create(**kwargs)
    return turn.id, json.loads(turn.output_text)
goal = "The character walks to the red door and opens it."
turn_id, plan = ask(f"Goal: {goal}\nBreak it into short visual steps.",
                    frame("frame_000.png"), PLAN)
for i, step in enumerate(plan["steps"], start=1):
    turn_id, verdict = ask(f"Step {i}: {step}\nDoes this frame show it done?",
                           frame(f"frame_{i:03d}.png"), VERDICT, previous=turn_id)
    print(i, step, verdict)
    if not verdict["done"]:
        break  # re-plan here, or send a steering prompt to your simulator
```
Two details carry the loop. `response_format` with a JSON schema keeps every answer machine-readable, and `previous_interaction_id` lets the server hold the plan between turns, so you don't resend history. The Interactions API stores requests by default; set `store=False` if you need stateless calls.

We haven't run this against a live key, so test it on your own frames first.

For a broader walkthrough of the model, see how to use the Gemini 3.8 Flash API.

A supervisor that quietly changes its JSON shape breaks your loop. Hand the checking to your coding agent and Apidog CLI:


```
npm install -g apidog-cli@latest
apidog login --with-token <YOUR_TOKEN>
npx -y skills add https://apidog.com   # pick your agent when prompted
```
Then give Claude Code, Codex or Cursor one [REDACTED PROMPT]

In Apidog project , create a test case for POST

https://generativelanguage.googleapis.com/v1beta/interactions with header

x-goog-api-key from the {{GEMINI_API_KEY}} environment variable. Send model

gemini-3.8-flash, a short goal, one base64 PNG frame and a response_format JSON

schema with "done" (boolean) and "reason" (string). Assert status 200 and that

output_text parses as JSON with both fields. Run it against the gemini

environment and summarize any failures.


```
The agent looks up the payload shape instead of guessing it, validates the file, creates the case and runs it:
```

bash

apidog cli-schema get test-case-create

apidog cli-schema validate test-case-create --file case.json

apidog test-case create --project  --file case.json

apidog run --test-case  --project  -e  -r cli,json

```
Two `apidog run` flags are worth adding for a vision loop. `-n, --iteration-count <n>` repeats the case so you can spot flaky verdicts, and `-d, --iteration-data <path>` feeds a CSV or JSON file of frames and goals, one per iteration. When the Genie 4 model ID ships, you change one environment variable and rerun.
Swap `-r cli,json` for `-r junit` and the same run drops into CI. Our [Apidog CLI CI/CD guide](http://apidog.com/blog/apidog-cli-ci-cd-pipeline?utm_source=dev.to&utm_medium=wanda&utm_content=n8n-post-automation) shows the pipeline, and [Apidog CLI in Claude Code](http://apidog.com/blog/apidog-cli-in-claude-code?utm_source=dev.to&utm_medium=wanda&utm_content=n8n-post-automation) covers the agent setup. Apidog's Free plan covers up to 4 users.
## What to watch for when the Genie 4 API ships
Google hasn't described the API. These questions decide how you integrate it:
| Question | Why it matters | Known on Oct 29, 2026? |
| --- | --- | --- |
| Model ID | Your config and test cases key off it | No |
| Output transport | Real-time worlds may need streaming or WebSockets, like the Gemini [Live API](https://ai.google.dev/gemini-api/docs/live), rather than one response | No |
| Session model | Long or stateful sessions change how you store and resume worlds | No |
| Inputs | Google demos keyboard controls, text steering and a reference image as the first frame; how those map to API fields is unpublished | No |
| Pricing unit | Per second, per frame, per session or per token changes your cost model completely | No |
| Session length limits | Google itself says coherence holds over short sessions | No |
| Audio output | Genie 4 generates audio natively; the API format for it is unpublished | No |
| Watermarking | Google says all Genie 4 video and audio carry SynthID | Yes, for the model |
Plan for them now: keep the model name, base URL and pricing math in config, put the frame source behind one interface, and log every frame and verdict, since Google says drift grows with session length.
If you're coming from Genie 3, our [Google Genie 3 overview](http://apidog.com/blog/google-genie-3?utm_source=dev.to&utm_medium=wanda&utm_content=n8n-post-automation) and [how to use Google Genie 3](http://apidog.com/blog/how-to-use-google-genie-3?utm_source=dev.to&utm_medium=wanda&utm_content=n8n-post-automation) cover the previous model.
  
## FAQ
**Is there a Genie 4 API key?** Not yet. No Genie 4 model appears in the Gemini API as of October 29, 2026. A standard Gemini API key from Google AI Studio works for `gemini-3.8-flash`, the model Google uses as Genie 4's supervisor, but it doesn't unlock Genie 4.
**How do I become a Gemini API early access partner for Genie 4?** Google hasn't published a process. Join the Project Genie waitlist, keep an active Gemini API project, and watch the Gemini API models page and release notes for an announcement.
**Is there a Project Genie API?** No. Project Genie is a consumer experience at [labs.google/projectgenie](https://labs.google/projectgenie). Google lists the Gemini API and Google AI Studio as the developer route for Genie 4.
**How much will the Genie 4 API cost?** Google hasn't announced pricing, a free tier or a billing unit. Treat any figure you see before an official pricing page as a guess.
**Can I test Genie 4 in Google AI Studio today?** Not unless you're a trusted tester. Google says AI Studio early access partners come next, with no date given.
```
## Top comments (0)
