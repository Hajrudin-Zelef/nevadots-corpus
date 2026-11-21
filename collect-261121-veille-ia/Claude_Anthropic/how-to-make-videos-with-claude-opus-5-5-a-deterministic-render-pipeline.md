---
id: collect-261121-veille-ia/dev-langages/how-to-make-videos-with-claude-opus-5-5-a-deterministic-render-pipeline
title: "How to Make Videos with Claude Opus 5.5: A Deterministic Render Pipeline"
domain: dev-langages
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["scene", "opus", "render", "frame", "videos", "claude", "time", "playwright"]
source: https://huggingface.co/blog/karmen-beatapi/how-to-make-videos-with-claude-opus-5-5
source_anchor: ""
source_lines: [1, 160]
section: "Claude_Anthropic"
---
**Originally published on the official BeatAPI blog:** How to Make Videos with Claude Opus 5.5

**TL;DR:** Claude Opus 5.5 outputs code, not video frames. For code-rendered motion graphics, ask it to write a scene that can render any requested time `t`. Playwright can capture each frame of an HTML scene, and ffmpeg can encode those frames to MP4. Below is a prompt, a runnable render loop, a sample visual, and a cost calculation for one reported token budget. Model tokens are one cost; hosted rendering, narration, licensed assets, and optional video generation can add others.

Not directly. Anthropic released Opus 5.5 on September 22, 2026, as `claude-opus-5-5`, with a 1M-token context window and up to 128K output tokens. It outputs text only.

It can write animation programs, plan a storyboard, and revise code after render checks. The public examples show what is possible, but their quality, token use, and production time depend on the prompt and surrounding tools:

- **Deedy Das (@deedydas)** demonstrated code-rendered video using JavaScript, a headless browser, and ffmpeg. The LaunchVideo team credits that demonstration as its inspiration.
- **LaunchVideo** , a public, source-available demo, takes a product URL or prompt and renders a short launch film. Its creators report about four minutes and roughly 90,000 input plus 15,000 output tokens per film. Those are their figures, not our benchmark.

For an HTML or Canvas scene, the pipeline has four steps:

1. **Opus 5.5 writes a scene.** For this tutorial, that is one HTML file with Canvas or SVG.
2. **A headless browser samples it.** Playwright moves the scene to time`t` , takes a screenshot, and repeats for every frame.
3. **ffmpeg encodes** the frames into H.264 and muxes in audio.
4. **Optional audio** comes from a separate audio file, generated sound, or TTS, then gets muxed into the MP4.

The rule that makes this reliable is **determinism**. The renderer doesn't run in real time: frame 400 might be captured seconds after frame 399. In this simple renderer, nothing on screen should depend on the wall clock. Avoid `Date.now()`, `requestAnimationFrame` loops, CSS transitions, and timers unless you replace their clocks with a deterministic virtual clock. Seed random values. Then each frame can depend on `t`, the input data, and fixed assets, which makes individual scenes easier to re-render and compare.

- **A. Single HTML file + Playwright + ffmpeg.** A small starting point for launch clips, social videos, and animated charts. Use a beat sheet and a few keyframes before asking for the full scene.
- **B. Remotion, HyperFrames, or Manim.** Alternatives for longer compositions and explainers; each has its own rendering workflow. Time narration to word timestamps if you need voiceover. Remotion's free license covers individuals and teams of up to three people; larger organizations need to check its current license terms.
- **C. Product-repo video.** Let the model inspect real UI components and assets, then verify the rendered result against the product. Do not use a plausible imitation as a product demo.
- **D. Longer assisted runs.** Split the storyboard into scenes, render each scene, review joins and audio, then assemble the final film. More iterations and optional generated footage increase cost.

Install the tools:

```
npm i playwright && npx playwright install chromium   # plus ffmpeg from your package manager
```
Ask Opus 5.5 for `scene.html` using the prompt template below. The scene must set `window.DURATION` and expose `async window.seek(t)`. To check your local setup before making a model call, save this small, hand-written scene as `scene.html`:

```
<!doctype html>
<style>body { margin: 0; background: #0c1116; }</style>
<canvas id="frame" width="1080" height="1080"></canvas>
<script>window.DURATION = 2;
const ctx = document.querySelector("#frame").getContext("2d");
window.seek = async (t) => {
  ctx.fillStyle = "#0c1116";
  ctx.fillRect(0, 0, 1080, 1080);
  ctx.fillStyle = "#5be2a1";
  ctx.beginPath();
  ctx.arc(120 + 400 * t, 540, 80, 0, 2 * Math.PI);
  ctx.fill();
};
</script>
```
Then save the following as `render.mjs` and run `node render.mjs scene.html out.mp4`. It should produce a two-second square clip with a moving green circle. Replace the test scene with Opus's HTML only after that local check, and inspect its frames before using it in a published video:

```
// render.mjs — node render.mjs scene.html out.mp4
import { chromium } from "playwright";
import { spawn } from "node:child_process";
import { once } from "node:events";
import path from "node:path";
import { pathToFileURL } from "node:url";
const [scene, out, fps = 30] = process.argv.slice(2);
if (!scene || !out) throw new Error("Usage: node render.mjs scene.html out.mp4 [fps]");
const browser = await chromium.launch();
const page = await browser.newPage({ viewport: { width: 1080, height: 1080 } });
await page.goto(pathToFileURL(path.resolve(scene)).href);
await page.waitForFunction(() => typeof window.seek === "function");
await page.evaluate(() => document.fonts.ready);
const total = Math.round((await page.evaluate(() => window.DURATION)) * fps);
const ff = spawn("ffmpeg", ["-y", "-f", "image2pipe", "-framerate", `${fps}`, "-i", "-",
  "-c:v", "libx264", "-pix_fmt", "yuv420p", "-crf", "18", out],
  { stdio: ["pipe", "inherit", "inherit"] });
for (let i = 0; i < total; i++) {
  await page.evaluate((t) => window.seek(t), i / fps);
  if (!ff.stdin.write(await page.screenshot({ type: "png" })))
    await once(ff.stdin, "drain");
}
ff.stdin.end();
const [exitCode] = await once(ff, "close");
await browser.close();
if (exitCode !== 0) throw new Error(`ffmpeg exited with ${exitCode}`);
```
We used a hand-written six-second, 1080×1080, 30-fps scene to check the renderer. The frame below shows a **sample** TikTok follower chart; it is not a Claude-generated clip or a real account's data. For longer films, render scenes separately and join them with ffmpeg's concat demuxer. Add audio in a second pass with `-c:v copy`.

*Sample frame from the local renderer test. The numbers and scene are synthetic.*

The same pattern works for **data-driven videos**. If the scene reads its numbers from a JSON file, one scene can render a creator's growth recap, a weekly top-posts reel, or one version per platform. The same BeatAPI key that calls Opus 5.5 also covers social media data tools (for example, TikTok, YouTube, Instagram, and X profiles and posts). An agent connected to the hosted MCP server can fetch those and write the JSON for you.

```
Make a {LENGTH}s video about {TOPIC} for {AUDIENCE}.
Output ONE self-contained HTML file, {W}x{H}, no network requests.
Contract: set window.DURATION = {LENGTH}; expose async window.seek(t).
Every pixel is a pure function of t. No Date.now, requestAnimationFrame
loops, timers, CSS transitions, or unseeded Math.random.
Style: palette {HEX COLORS}, font {FONT}, motion "{e.g. snappy, no fades}".
Facts (use only these): {VERIFIED BRIEF}
Audio: {voiceover word timestamps, or target BPM with cuts on beats}.
First reply with a beat sheet and 4 keyframes only; write the file after I approve.
```
The **Facts** and **Style** lines matter most. Check every factual statement against the supplied brief, then inspect frames at the beginning, middle, and end before rendering the whole scene.

**An illustrative calculation.** LaunchVideo reports about 90,000 input and 15,000 output tokens per film. At standard uncached list prices checked on September 27, 2026:

|  | Input (90K tokens) | Output (15K tokens) | Per video | Per 100 videos | 
|---|---|---|---|---|
| Anthropic list ($4 / $20 per M) | $0.36 | $0.30 | **$0.66** | $66 | 
| BeatAPI list ($2 / $10 per M) | $0.18 | $0.15 | **$0.33** | $33 | 

This estimates model tokens for one reported generation budget. It excludes rendering infrastructure, TTS, paid assets, retries, cached-token rules, and any generated footage. Thinking tokens contribute to output usage. A 50% difference follows only for the compared standard input and output rates at the same token counts; it is not a guarantee about the total video budget. Check the current rate and a representative settled request before scaling up.

For a separate model-version comparison and migration checklist, see the Opus 5.5 vs Opus 5 guide.

The Claude Opus 5.5 API page is the next step if you want to call the model through BeatAPI. It shows the current model ID, request formats, price, and the route to create a key. On September 27, 2026, its public catalog listed:

- **Model:**`claude-opus-5-5` , with a 1M-token context and 128K max output tokens.
- **Price:** $2 per million input tokens and $10 per million output tokens (listed as 50% off on the homepage).
- **Endpoints:** OpenAI Responses (`/v1/responses` ), OpenAI Chat Completions (`/v1/chat/completions` ), and Anthropic Messages (`/v1/messages` ). They return JSON directly or stream.
- **Optional data source:** the same account can access the currently listed Social Data actions through`https://beatapi.io/mcp` . A profile response is a snapshot; a growth chart needs separately collected historical snapshots.

Switching an existing OpenAI SDK integration is a base-URL change (illustrative, not run):

```
import os
from pathlib import Path
from openai import OpenAI
client = OpenAI(
    base_url="https://api.beatapi.io/v1",
    api_key=os.environ["BEATAPI_API_KEY"],
)
prompt = (
    "Write a self-contained six-second 1080x1080 HTML animation. "
    "Set window.DURATION=6 and expose async window.seek(t). "
    "Use only deterministic drawing; return HTML only, with no code fences."
)
response = client.chat.completions.create(
    model="claude-opus-5-5",
    messages=[{"role": "user", "content": prompt}],
    max_tokens=12000,
)
html = response.choices[0].message.content or ""
if not html.lstrip().lower().startswith("<!doctype html>"):
    raise ValueError("Review the model response before saving it as HTML")
Path("scene.html").write_text(html, encoding="utf-8")
```
Tools that speak Anthropic Messages can point their Anthropic base URL at `https://api.beatapi.io`, and the client appends `/v1/messages`. BeatAPI's integration docs describe this for Claude Code through `ANTHROPIC_BASE_URL`. Set the model to `claude-opus-5-5` and keep your key in an environment variable, not in chat or source files.

- **Code-only scenes are motion graphics.** They do not create filmed footage or natural lip-sync by themselves. Add licensed footage or another generation step if you need it.
- **Visual quality varies.** A scene may look like animated slides; inspect the result and revise the storyboard, typography, and timing.
- **Factual errors are possible.** Supply a verified brief for explainers and review the rendered claims.
- **Long runs can break.** Subagent chapters may not join cleanly, and the model sometimes slips a timer into the scene. Render short test ranges first.
- **Long jobs need process supervision.** Keep the renderer running until ffmpeg exits successfully and check the final MP4 before treating the job as complete.
- **Setup is still required.** You need Node, Playwright, Chromium, ffmpeg, and optionally TTS.

No. It outputs text only. It makes videos by writing animation code that a headless browser renders frame by frame and ffmpeg encodes into an MP4.

Node.js, Playwright with Chromium, and ffmpeg, plus access to the model. A TTS engine such as Kokoro-82M or ElevenLabs is optional for narration. Remotion, HyperFrames, and Manim are optional frameworks.

For the 90K input and 15K output tokens reported by LaunchVideo, standard uncached token pricing gives an illustrative $0.66 at Anthropic's list price or $0.33 at BeatAPI's listed price on September 27, 2026. Rendering, revisions, audio, and other assets are additional.

The renderer captures frames at its own pace, not in real time. If every frame depends only on `t`, the video comes out identical on every render and can be rendered in parallel.

Yes, if your code uses OpenAI Chat Completions, OpenAI Responses, or Anthropic Messages. Change the base URL to BeatAPI, use your BeatAPI key, and set the model to `claude-opus-5-5`.

**If you want to use BeatAPI for the model call:** check the current `claude-opus-5-5` rate and request format on the Opus 5.5 API page, then render and review one short scene before attempting a longer video.
