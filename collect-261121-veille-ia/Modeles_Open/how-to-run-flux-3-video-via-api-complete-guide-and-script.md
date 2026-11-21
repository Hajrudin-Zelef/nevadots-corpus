---
id: collect-261121-veille-ia/modeles-open/how-to-run-flux-3-video-via-api-complete-guide-and-script
title: "How to Run FLUX 3 Video via API: Complete Guide and Script"
domain: modeles-open
role: reference
task: article
actors: []
dates: []
keywords: ["script", "video", "python", "flux3", "wideo", "flux", "draft", "full"]
source: https://quantslant.com/how-to-run-flux-3-video-api-script/
source_anchor: ""
source_lines: [1, 220]
section: "Modeles_Open"
---
👁 **127** reads

FLUX 3 Video does not come with a standard consumer app interface - Black Forest Labs distributes it exclusively through an API. That might sound intimidating, but all you need is an API key, a few dollars in balance, and a single script. Below, we break down three provider routes (fal.ai, OpenRouter, and the native BFL API), share a battle-tested Python script, and cover the essential prompting rules to get clean results. For our full benchmarks and pricing breakdown, see our FLUX 3 Video review.

## Provider Comparison: fal.ai vs OpenRouter vs BFL API

| Provider | 5s at 720p | Draft mode | Supported modes | Best for | 
|---|---|---|---|---|
| fal.ai | $0.425 | Yes ($0.03/s) | Text, image, keyframes, first/last frame, extension, editing | Lowest price, most features | 
| OpenRouter | $0.85 | No | Text, first and last frame | Single key for LLMs and video | 
| BFL API | $0.85 | Yes ($0.06/s) | All modes, including QHD and 4K | Direct source, full capabilities | 

Pricing verified as of October 11, 2026. We generated 6 clips via OpenRouter and 3 via fal.ai - you can inspect the outputs in our guide on what FLUX 3 Video can do. The script below uses fal.ai because it offers the lowest rates and includes a dedicated draft mode.

## Step-by-Step Setup

1. Create an account at fal.ai, add funds under “Billing”, and generate an API key in the “Keys” section.
2. Install the required library: `pip install requests` .
3. Download the script flux3_wideo.py (full source code below).
4. Set your API key in an environment variable. In PowerShell: `$env:FAL_KEY="your_key"` . In macOS/Linux terminal:`export FAL_KEY="your_key"` .
5. Generate a draft: `python flux3_wideo.py "scene description" --szkic` . The script calculates the estimated cost and prompts for confirmation.
6. Once satisfied with the draft preview, run the exact same command without `--szkic` for full quality at 720p, or append`--rozdz 1080p` for 1080p.
7. To animate an existing image, add `--obraz https://example.com/image.jpg` . The image must be accessible via a public URL.

## Our Real-World Script Test

We ran the script on October 11 in a clean directory using draft mode with the following prompt:

python flux3_wideo.py "A red and cream tram passes along a rainy Krakow street at dusk, reflections of shop lights on the wet cobblestones, people with umbrellas wait at the stop. A sign on the tram stop reads "PLAC WOLNICA". Static camera, realistic. Sound: rain, tram bell, wheels on rails." --szkic --plik tramwaj_szkic.mp4 --tak

The result: a 4.8 MB MP4 file, 5 seconds long, 1280×704 resolution, with full audio, delivered in 37 seconds. The red and cream tram moves cleanly across the frame, pedestrians with umbrellas wait at the shelter, and storefront lights reflect off the wet cobblestones. The sign text “PLAC WOLNICA” (Wolnica Square) is illegible in draft mode because the plaque is simply too small in the frame. If you need text to render sharply, describe it as a prominent foreground subject, as we demonstrated in our chalkboard test.

## Complete Python Script

Download: flux3_wideo.py (91 lines). It reads your key securely from environment variables, outputs a cost estimate before sending the request, retries automatically on network drops, and saves the final output directly to MP4.

```
"""flux3_wideo.py - Generate FLUX 3 Video clips via fal.ai (quantslant.com, Oct 11, 2026)
Requires: Python 3.9+ and the requests library (pip install requests).
API Key: Set via FAL_KEY environment variable (fal.ai -> Dashboard -> Keys). Never hardcode your key.
Examples:
  python flux3_wideo.py "A red tram passes through a rainy Krakow street at dusk" --szkic
  python flux3_wideo.py "The camera slowly dollies forward..." --obraz https://example.com/image.jpg --czas 6
  python flux3_wideo.py "..." --czas 10 --rozdz 1080p --proporcje 9:16 --bez-dzwieku
Pricing (fal.ai rates as of Oct 11, 2026): $0.085/sec for full quality, $0.03/sec for draft mode.
The script displays an estimate and asks for confirmation (skip with --tak).
"""
import argparse, os, sys, time, pathlib
import requests
RATES = {'full': 0.085, 'draft': 0.03}  # USD per second, fal.ai, Oct 11, 2026
def main():
    a = argparse.ArgumentParser(description='Generate FLUX 3 Video via fal.ai')
    a.add_argument('prompt', help='English prompt; wrap spoken foreign lines in quotes')
    a.add_argument('--obraz', help='Source image URL for image-to-video mode')
    a.add_argument('--czas', type=int, default=5, help='Duration in seconds, 5-20 (default: 5)')
    a.add_argument('--rozdz', default='720p', choices=['720p', '1080p'])
    a.add_argument('--proporcje', default='16:9', help='16:9, 9:16, 1:1, 4:3, 3:4, 21:9, 2:1, or auto')
    a.add_argument('--szkic', action='store_true', help='Draft mode: faster and cheaper for ideation')
    a.add_argument('--bez-dzwieku', action='store_true', help='Disable audio generation')
    a.add_argument('--plik', default='', help='Output file name (default: flux3_<timestamp>.mp4)')
    a.add_argument('--tak', action='store_true', help='Skip cost confirmation prompt')
    x = a.parse_args()
    api_key = os.environ.get('FAL_KEY')
    if not api_key:
        sys.exit('Missing FAL_KEY variable. Set it first, e.g. in PowerShell: $env:FAL_KEY="..."')
    if not 5 <= x.czas <= 20:
        sys.exit('Duration must be between 5 and 20 seconds.')
    mode = 'image-to-video' if x.obraz else 'text-to-video'
    endpoint = f'blackforestlabs/flux-3/{mode}' + ('/draft' if x.szkic else '')
    body = {'prompt': x.prompt, 'duration': x.czas, 'aspect_ratio': x.proporcje, 'generate_audio': not x.bez_dzwieku}
    if x.obraz:
        body['image_url'] = x.obraz
    if not x.szkic:
        body['resolution'] = x.rozdz  # draft mode defaults to 720p
    cost = x.czas * RATES['draft' if x.szkic else 'full']
    print(f'Model: {endpoint}nDuration: {x.czas}s, estimated cost: ${cost:.3f}')
    if not x.tak and input('Proceed with generation? [y/N] ').strip().lower() != 'y':
        sys.exit('Aborted.')
    headers = {'Authorization': 'Key ' + api_key, 'Content-Type': 'application/json'}
    r = requests.post('https://queue.fal.run/' + endpoint, headers=headers, json=body, timeout=120)
    if r.status_code >= 300:
        sys.exit(f'Error {r.status_code}: {r.text[:400]}')
    task = r.json()
    print('Task queued, waiting for output...')
    t0 = time.time()
    while True:
        time.sleep(8)
        try:
            status_resp = requests.get(task['status_url'], headers=headers, timeout=60).json()
        except requests.RequestException:
            continue  # Transient network drop - keep polling
        status = status_resp.get('status')
        print(f'  {int(time.time() - t0)}s: {status}')
        if status == 'COMPLETED':
            break
        if status not in ('IN_QUEUE', 'IN_PROGRESS'):
            sys.exit(f'Task failed: {status_resp}')
        if time.time() - t0 > 1800:
            sys.exit('No result after 30 minutes. Check manually: ' + task['response_url'])
    for _ in range(6):
        try:
            result = requests.get(task['response_url'], headers=headers, timeout=90).json()
            break
        except requests.RequestException:
            time.sleep(10)
    else:
        sys.exit('Failed to retrieve output. URL: ' + task['response_url'])
    url = result['video']['url']
    out_file = pathlib.Path(x.plik or f'flux3_{int(time.time())}.mp4')
    out_file.write_bytes(requests.get(url, timeout=300).content)
    print(f'Done in {int(time.time() - t0)}s: {out_file.resolve()} ({out_file.stat().st_size // 1024} KB)')
if __name__ == '__main__':
    main()
```
## Prompting Rules for FLUX 3 Video

- **Prompt in English** , and isolate foreign language dialogue or signs in explicit quotes. For example, when we tested non-English speech, prompting`She says clearly in Polish: "..."` produced flawless lip-sync and accurate pronunciation in both test clips.
- **Specify audio separately** using a dedicated “Sound:” section at the end of your prompt. Note: in our espresso machine packshot, the model only rendered ambient background birds without the machine hiss. Specialized product sound effects are often better added in post-production.
- **Make in-frame text prominent** (e.g., “the chalkboard clearly shows exactly two lines of text”). Small background signage frequently turns into illegible noise.
- **Define camera moves in one clear sentence** : “The camera slowly dollies forward”, “180-degree orbit at a constant speed”, or “static camera”. In our testing, orbital movements completed without warping subject geometry.
- **Multi-shot scenes** should be separated with “Hard cut. Shot 2:” while reiterating core character descriptions across each shot.
- **Use negative constraints at the end** : “no subtitles, no on-screen text, no music” if you require clean plates without automated additions.
- **Lock aspect ratio manually** when supplying a start image. Using “auto” led to unwanted auto-cropping on our test cafe photograph.

## Script Command-Line Flags

| Flag | Description | Default | 
|---|---|---|
| `prompt` | Scene description (required) | - | 
| `--obraz URL` | Starting image URL for image-to-video mode | None (text-to-video) | 
| `--czas` | Clip duration in seconds (5 to 20) | 5 | 
| `--rozdz` | Resolution: 720p or 1080p (draft is locked to 720p) | 720p | 
| `--proporcje` | Aspect ratio: 16:9, 9:16, 1:1, 4:3, 3:4, 21:9, 2:1, auto | 16:9 | 
| `--szkic` | Draft mode for fast previews ($0.03/s) | Disabled | 
| `--bez-dzwieku` | Generate video without an audio track | Audio enabled | 
| `--plik` | Custom output MP4 file name | flux3_<timestamp>.mp4 | 
| `--tak` | Bypass interactive cost confirmation | Prompts for confirmation | 

## Cost Breakdown

| Clip Specs | fal.ai | OpenRouter / BFL API | 
|---|---|---|
| Draft (5 seconds) | $0.15 | BFL $0.30, OpenRouter N/A | 
| 5s at 720p | $0.425 | $0.85 | 
| 10s at 720p | $0.85 | $1.70 | 
| 20s at 720p | $1.70 | $3.40 | 
| 20s at 1080p | No data | $5.80 | 

Audio generation is included at no additional cost. In total, we spent $6.10 across 9 generation runs while testing FLUX 3 for this series.

## Troubleshooting Common Errors

- **“Missing FAL_KEY variable”** - The environment variable was exported in a different terminal session. Set it in the active shell window where you execute the script.
- **HTTP 401 or 403** - Invalid key or restricted key permissions. Regenerate a new API key from your fal.ai dashboard.
- **Balance error** - Account has zero or insufficient credits. Top up your balance in the Billing tab.
- **Source image fails to load** - The URL must be publicly reachable (no authentication walls) and point directly to a valid PNG, JPEG, or WebP file.
- **Dropped download connection** - We encountered this once during testing; our script retries the download up to six times and prints the raw result URL if it fails, ensuring you do not pay twice for the same completed task.

## Update: 17 November 2026

The FLUX 3 Video workflow below uses the fal endpoint blackforestlabs/flux-3/text-to-video.

A studio wants a draft of a rolling orange that falls off the edge of a checkout counter. The arc has to continue through the edge.

### The working brief

The orange rolls right, reaches the edge, then falls straight out of frame while the camera stays level.


On this route, supported durations in seconds include auto, 5, 6, 7, 8, 9, 10, 11. supported resolutions include 720p, 1080p, 1440p, 2160p. available aspect ratios include auto, 21:9, 2:1, 16:9, 4:3, 1:1, 3:4, 9:16.

Look for a duplicate orange or a change in gravity where the counter ends.

Mark the end of the counter in the preview. Track the center of the orange as it crosses that point; a replacement fruit can appear during the fall.

Model details · API pricing

### Sources

- https://bfl.ai/blog/flux-3 (accessed November 17, 2026)
- https://fal.ai/models/blackforestlabs/flux-3/text-to-video (accessed November 17, 2026)
- https://artificialanalysis.ai/video/leaderboard/text-to-video (accessed November 17, 2026)
- https://artificialanalysis.ai/video/leaderboard/image-to-video (accessed November 17, 2026)

## Frequently Asked Questions

### Can I use FLUX 3 Video without coding?

Yes. Both fal.ai and OpenRouter offer web playgrounds where you can enter prompts directly into an interface. Writing a custom script is primarily useful when batching generations or automating file delivery.

### How do I generate clips longer than 20 seconds?

By chaining extensions. On fal.ai, the `extend-video` endpoint takes the final frame of an existing video and synthesizes another 5 to 15 seconds. On the native BFL API, video extensions cost $0.41/s at 720p.

### Can generated clips be used commercially?

Commercial rights depend on the terms of Black Forest Labs and your chosen API intermediary. Review your provider’s specific API license agreement before using outputs in commercial campaigns.

For more detailed prompts and side-by-side clips, explore our guide to what FLUX 3 Video can do, our benchmark of FLUX 3 vs Wan 3.0 vs Seedance 2.5, and our roundup of the best AI video generators.

*→*

## Sources and Verification Date

- fal.ai: FLUX 3 text-to-video schema and pricing
- OpenRouter: FLUX.3 Video
- BFL API pricing documentation
- BFL: FLUX 3 Video launch notes (Aug 4, 2026). Script verified on October 11
- 2026 in a clean environment (Windows 11
- Python 3.13
- draft mode
- $0.15)

## More on this from QUANTSLANT

*guides*

*·*

**Freepik AI Advanced Inpaint: Edit Product Photos Fast**Freepik AI Advanced Inpaint lets you remove backgrounds, fix lighting, and retouch product photos without Photoshop. Here is how to use it step by step.
