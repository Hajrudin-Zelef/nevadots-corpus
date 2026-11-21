---
id: collect-261121-veille-ia/xiaomi-mimo/mimo-v2-6-flash-local-on-two-mac-studios-vs-cloud-benchmark
title: "MiMo V2.6 Flash Local on Two Mac Studios vs Cloud: Benchmark"
domain: xiaomi-mimo
role: reference
task: article
actors: ["Xiaomi"]
dates: []
keywords: ["local", "cloud", "checks", "macs", "scored", "ceiling", "studios", "benchmark"]
source: https://dev.to/mihai_leanzero/mimo-v26-flash-local-on-two-mac-studios-vs-cloud-benchmark-52a2
source_anchor: ""
source_lines: [1, 114]
section: "Xiaomi_MiMo"
---
MiMo V2.6 Flash doesn't fit on one of our Macs, so we ran it local across two Mac Studios with vision loaded, and it built a payments app that our benchmark scored 0.6605. In Xiaomi's own cloud, the same model scored 0.7909. I expected worse. Before the benchmark's 3D ceiling was applied, the local run had earned 0.8309 of the weighted checks and the cloud run 0.838, and the ceiling made the difference.

It's Xiaomi's MiMo-V2.6-Flash, a 309-billion-parameter mixture of experts with 15 billion active per token. We ran a community MLX conversion, EigenLabs/MiMo-V2.6-Flash-MOPD-MLX-4bit-mtp, and it takes 161 GB on our SSD. Neither of our Mac Studios can hold that alone. Our workhorse is an M3 Ultra with 96 GB and our workmule an M5 Max with 128 GB, so we cut the model's 48 layers 19 and 29, joined the two Macs with one Thunderbolt 5 cable, and let them pass hidden states to each other over RDMA. Driving it was Goose Swarm, our edition of the open-source goose agent.

Why should you care if you're not about to buy two Mac Studios? Most of what I read about running big models locally stops at whether the weights fit. Making the weights fit took some arithmetic. Keeping two Macs in step, token by token, for five hours took far more of the effort, and so did getting the model to see.

It's one run per setup on one task, so read it as a field report.

Our Gauntlet benchmark gives an agent 150 model calls to build a payments console: two backend services, a web UI and a 3D field of payment towers, scored on 99 checks. It also has an admission rule. If certain 3D checks fail, the score cannot pass a ceiling, however much credit the rest earned. I read all three runs from the board on 29 October:

| Run | Final | Earned before ceiling | Ceiling | Calls | Agent time | Context | Rank | 
|---|---|---|---|---|---|---|---|
| Cloud, OpenRouter pinned to Xiaomi | 0.7909 | 0.838 | 0.799 | 131 | 2 h 08 min | 1M | 8th of 49 | 
| Local, 2 Macs, vision, 256k | 0.6605 | 0.8309 | 0.669 | 150 | 4 h 51 min | 262,144 | 22nd | 
| Local, 2 Macs, text-only, 120k | 0.5894 | 0.8083 | 0.599 | 130 | 4 h 32 min | 119,808 | 27th | 

The live board is the current word if any of this moves. Both local runs went through goose's OpenAI-compatible provider pointed at our engine on the workhorse; the cloud run went through OpenRouter, pinned to Xiaomi's own endpoint.

So where did the 0.13 go? Almost all of it is the ceiling. Two 3D checks capped the local run at 0.669. One, `s_tower_geometry` (0.778), shoots independent rays at the towers and checks their bevels, ribs and gaps against the spec. The other, `t_vs7dbg_truth` (0.8), asks the app's own debug hook which tower sits under a pixel and compares that with what the GPU picking returns; the camera numbers matched, but over empty background the hook said no tower while the GPU pick buffer returned an id (`pickTriplet=False`). Cloud MiMo passed both. Its own cap of 0.799 came from `t_stream_diff`, the live-update check, which it scored 0 and the local run scored 1.0.

So I read the remaining gap as a handful of 3D details on one page of the app.

1. 
**Vision fixed what the text-only run could not.** Four checks that scored 0 or 0.5 without a vision tower all scored 1.0 with it, and the old run's 0.599 ceiling went away.
2. 
**The local run lost 12 checks to the cloud and won 7.** Most losses are 3D pixel work; the wins are the live stream and backend recovery (stream diff 1.0 vs 0, notifier exactly-once 1.0 vs 0.69).
3. 
**One 0.7-second race cost three checks and the only penalty.** Webhook registration lost to the vendor's schedule, so a correct forged-signature handler never got tested and one payment never arrived.
4. 
**The split itself broke twice, in ways a single Mac never would.** A dtype mismatch between the Macs and two Macs sampling different tokens each crashed a run. Both are fixed, on a branch.

The arithmetic is short. EigenLabs lists the weights at 170,974,651,261 bytes, about 159 GiB, in 37 shards. Together our two Macs have 224 GB of unified memory. Our engine keeps a 5% reserve under the Metal limit, and the KV cache, the vision tower and the prompt-processing workspace all need room on top of the weights.

Despite the name, it isn't uniformly 4-bit. Its card says the routed experts keep Xiaomi's upstream MXFP4 values "bit for bit", while attention, the dense MLP, the embeddings and the output projection are 8-bit affine. Xiaomi's cloud endpoint is listed on OpenRouter as fp8. If you want the background on why these formats behave differently on a Mac, I measured MLX vs GGUF on Apple Silicon and MXFP4 against affine 4-bit earlier.

What makes 256k possible at all is the architecture. Xiaomi's card lists 48 layers: 39 use sliding-window attention with a 128-token window and only 9 use global attention. Only those 9 keep a cache that grows with the conversation. Our planner charges 23,040 bytes per token for it across both Macs, plus one copy for the prompt cache and a workspace for the attention scores. At 262,144 tokens the plan said 75.1 GB on the workhorse and 107.05 GB on the workmule. Measured with a 210,724-token prompt, the peaks were 75.9 GB and 109.1 GB, and swap stayed flat on both Macs.

Why 19 layers on the workhorse and 29 on the workmule? An M3 Ultra with less memory, which also carries the vision tower, gets fewer layers. It also gets the last layers: mlx-lm's pipeline code "split[s] layers in reverse so rank=0 gets the last layers", and the workhorse is rank 0.

There is one copy of the weights. It lives on the workhorse's external SSD and the workmule reads it over NFS, so a cold load took 241 to 246 seconds across five loads with no NFS errors. That `-mtp` in the build's name means it also ships multi-token-prediction heads. Our split's loader drops them, so the speeds below are plain decoding.

Apple added RDMA over Thunderbolt in macOS 26.2. Its release notes describe it as low-latency communication between Thunderbolt 5 hosts "for use cases including distributed AI inference using MLX". MLX's backend for it is called JACCL, and the MLX distributed docs say you switch it on per Mac from recovery mode with `rdma_ctl enable`, then reboot.

We measured the link on 26 October with an MLX `all_sum` between the two Macs. A 1 KB message took a median 21 µs (a rerun gave 30), and a 1 GB one moved at 9.31 GB/s. Over the same cable, MLX's ring backend needed 164 µs and managed 3.8 GB/s. Our interface reported an 80 Gb/s link, and 9.31 GB/s (about 74 Gb/s) is close to that line rate.

This is a pipeline split. Each Mac runs its own layers and hands the hidden state for each chunk of tokens to the other. Tensor parallelism, where both Macs work on each layer, needs much more traffic; a pipeline sends one activation per step. In exchange, both Macs must stay in lockstep, with the same requests in the same batch and the sends and receives in the same order. When they drift, no error names the cause. One side waits for bytes that never come, and you get a GPU timeout or a bare receive failure.

How fast is it? On a 100k-token prompt (measured on the earlier 18/30 split at a 114k window) the first token took 360 seconds cold (about 277 tokens per second of prefill) and 0.52 seconds when the prompt was already cached; decoding ran at 24.8 tokens per second. At 210,724 tokens the first token took 962 seconds and decoding dropped to 17.7 tokens per second. Our engine's telemetry for the benchmark run says 22.1 tokens per second of decoding on average. I wouldn't run an agent on this without prompt caching. Goose resends the whole conversation on each call, and without the cache each call would pay the cold prefill again.

They're in the order we hit them, all on the `mimo-distributed` branch on top of mlx-lm 0.31.3.

Our first local run was text-only by design. The mlx-community build we started with says in its README that it is "a text-only conversion: the vision and audio encoders and the MTP/DFlash draft weights are not included". Two hours into its first attempt it called `read_image` on its own screenshot. Goose sent the PNG and the engine answered 404 "Only 'text' content type is supported". Goose retried and then gave up on the whole run. Our fix made goose ask a loopback engine what it can read before sending an image, and send a named placeholder instead.

Moving to the omni build with a vision tower brought two loading bugs. An omni checkpoint names its weights `language_model.model.*`, and mlx-lm's sharded loader refused it on both Macs with "Pipeline loading is only supported for MLX converted models". Then the wrapper we put around the chat template called itself through a forwarding layer and the engine's readiness probe answered "maximum recursion depth exceeded". A smaller one: rank 0 prepared an image twice whenever it arrived while a batch was running, because we had wrapped both `get` and `get_nowait` on its queue, and Python's `get_nowait` calls `get`.

Our first benchmark launch with vision died a minute in. Rank 0 said `[jaccl] Recv failed with error code -12` and rank 1 said `[METAL] ... GPU Timeout`. We traced each send and receive on both Macs. Rank 1 sent a (2, 768, 4096) tensor as float32 while rank 0 had posted a bfloat16 buffer of the same size in elements, 12.6 MB against the 25.2 MB arriving. It came from mlx-lm 0.31.3. When a second request joins a batch, the sliding-window cache pads the empty row with `mx.array([])`, which is float32, and that promotes the whole batch. Our own earlier fix for this covered the full-attention cache but not the rotating one, and MiMo has 39 sliding-window layers. On one Mac this would only be slow. Across two it breaks the pairing, because rank 0 types its receive buffer from its own bfloat16 activations.

Our second crash came about two and a half hours into the first 256k run. Rank 1 timed out again, right after operation 430,956 of the trace, with rank 0 waiting for a 103-token chunk and rank 1 sending 256. Each Mac had been sampling its own token. A pipeline rank feeds the token it holds into its own layers and stores the answer in its own prompt cache. They agreed on all 69 answers until the run's first image request. Then answer 70 differed from token 9 and answer 71 from token 2, so rank 1 cached a different conversation from rank 0. On the next request it found a shorter cached prefix, so it had more prompt left to read and sent a 256-token chunk where rank 0 expected its last 103. Now each rank takes rank 0's token, with one tiny `all_sum` per step. The relaunch ran clean. I still don't know why the samples first diverged on an image. There's no random draw in our media path. My guess is a numeric difference in the output layer between an M3 Ultra and an M5 Max, but I haven't proven it, and the fix doesn't depend on it.

The last one was arithmetic. For most of a day the split refused anything above 114,688 tokens with vision loaded, because our test setup had baked an 8 GB margin under the Metal limit into the MiMo config, on top of the engine's own 5% reserve. I caught it counting the same safety twice. With the margin removed and the one reserve kept, the planner fitted 262,144 tokens, and the measured peaks came in within about 2 GB of the plan with swap flat. All of this sits on a development branch of Goose Swarm; the latest release, 3.0.134, has none of it.

First, what vision bought. For the text-only run, the 0.599 ceiling came from `s_visible_surface`, and its worst 3D checks were the ones a screenshot settles: `s_visible_surface` 0, `t_height_pixels` 0 (none of six tower tops within 3 pixels), `q_inspector_framing` 0 and `s_currency_collar` 0.5. With the vision tower, all four scored 1.0. I can't prove the screenshots are why (it's also a newer checkpoint and twice the context), but the model read three of its own screenshots during the run, all of the 3D field, and the 3D field is what improved. Before the benchmark I wanted proof it could see: a rendered table read back correctly, and 12 of 12 basic colours were named right.

Then what it lost. Locally the run scored below the cloud on 12 checks and above it on 7. Four moments explain most of it.

An app in this task has to register a webhook with a mock payments vendor, and the vendor is designed to refuse the first few attempts. Later in the sync it sends a forged event, and a correct app answers 401. MiMo wrote that handler correctly:


```
if not secret or not signature or not self.verify_signature(secret, signature, raw_body):
    self.hooks['forged'] += 1
    return 401, error('invalid_signature', 'The delivery signature does not verify')
```
It started registration on a background thread alongside the first sync walk, retrying every 2 seconds. At 21:25 its reasoning had the assumption in it: "forged signature test happens presumably after registration".

The vendor refused the first three registrations, the walk reached the page where the forged event is scheduled, and the trace shows it dropped with `"reason": "no_webhook_registered"`. Registration succeeded about 0.7 seconds later. Our scorer row reads `40/40 deliveries 2xx-acked, forged -> None`, worth 0.6. My reading is that the model treated registration as something that could happen beside the walk when it had to happen first, or at least retry in under a second.

Cloud MiMo did better: `54/54 deliveries 2xx-acked, forged -> 401`. So did our own text-only run, with the same 54 out of 54 and a 401. So running locally didn't cause it: our earlier local run got it right.

Those same 0.7 seconds dropped a payment that the vendor creates halfway through the walk and announces only by webhook. Its total read 12,288 and the scorer wanted 12,289, worth 0.75. At 21:59, testing its own app, it wrote: "12289 means one extra — maybe a webhook delivered a payment.created for a mid-walk create." So it knew where the missing payment came from, but the race still dropped it. Both the cloud and the text-only run reported 12,289. The same missing payment also left 3 of 384 daily bucket cells wrong, and that was the run's only critical penalty (x0.98). One race, three checks.

Our contract asks for legend and inspector text of at least 12 CSS px at 4.5:1 contrast. This app met both: 12 to 13 px, contrast 16.8:1, nothing clipped. It still scored 0.25 on `v_presentation_text`, because our probe also counts screenshot pixels within 8 levels per channel of the text colour and wants at least 3. In a thin "Avenir Next", "settled" at 12 px left 2 such pixels and the digit "1" in the inspector, at 13 px, left none. A bold heading above them left 266.

Cloud MiMo's app used Verdana and passed 4 of 4 groups. Our local run's three screenshots were all of the 3D canvas, so it never looked at its own text. Part of this is on us. Our ink rule isn't in the contract text the model reads, so a model can meet each stated number and still lose the check on a rule it couldn't see.

At 21:25 the model restated the label rule from the contract almost word for word: a label is eligible if its anchor is inside the canvas and the GPU pick at the anchor returns that tower. Then the code it wrote added a test the contract doesn't have:


```
if (rect.x < 0 || rect.y < 0 || rect.x + rect.w > Wcss || rect.y + rect.h > Hcss) continue;
```
That line hid one label, a KWD payment whose 110-pixel box ran past the right edge of the canvas. Our scorer expected 7 labels and found 6, and the exact-set miss cost about a third of the check (it scored 0.636). My guess is that "don't draw labels off screen" looks like good UI hygiene, and the model added it on instinct against the spec it had just quoted. Cloud MiMo passed with 3 of 3, but its decisive view had only 3 labels, so it may never have met the edge case. I can't say more without the cloud's code.

This is the check that capped the cloud. An app here receives payment updates over a server-sent event stream and has to patch the 3D field in place, uploading only the changed bytes. MiMo planned the upload budget before writing any 3D code ("for updates I must use bufferSubData on the instance buffer") and the code follows the plan, applying only newer versions per tower. It scored 1.0. Cloud MiMo's row says `no SSE batch observed applying`, 0. Locally it also beat the cloud on exactly-once notifications (1.0 against 0.69) and on an atomic outbox (1.0 against 0.5).

Its batch took 707 ms to show up on screen, where the text-only run managed 204 ms, so the latency check fell to 0.25. My guess is that it batched renders and the deferred render pushed the update past the fast rung.

Quantisation differs. Ours is a community 4-bit MLX build with MXFP4 experts and 8-bit attention, and OpenRouter lists Xiaomi's endpoint as fp8. Nothing in the scorer rows points at a quantisation error, but nothing rules one out either.

Context differs too. 262,144 tokens locally against about 1M in the cloud meant 2 compactions against 0, at 23:35 and 01:44. In the cloud, MiMo read 37.0M prompt tokens over its run and the local one 16.1M. I can't tie a lost check to a compaction.

It's probably the same checkpoint, but I couldn't re-verify that. We picked the MOPD build because, as we read it at the time, Xiaomi said it serves MOPD on its API under the same model name. Today its blog pages render without text for our fetcher, so I can't quote that sentence. Our text-only run used the earlier RL checkpoint, so the two local runs differ in two ways at once.

Whether the cloud run actually sent screenshots to the model I can't check either. Its build folder from 2 October is gone from all four of our Macs, and only its scorer rows remain. That's our mistake, and it's why I can't quote the cloud's code above.

Locally the run took 4 hours 51 minutes and all 150 calls; the cloud took 2 hours 8 minutes and 131. Our board entry is the second 256k launch, because the first one crashed on the token bug above. Each setup ran once, and we've measured before that the same model can swing a fair bit between two builds. OpenRouter billed the cloud run at least $0.28. I didn't measure the local run's electricity.

If your code can't go to the cloud, I think yes, as long as you accept the costs above. On this task, a model running on hardware we own earned 99% of the cloud run's weighted checks and ended at about 84% of its final score, and it beat the cloud on the backend work that keeps a payments ledger honest. It was 2.3 times slower to finish, and it took us four days of bugs to keep two Macs in step. If you try this, test early with two requests at once and with an image; both of our crashes hid there. And give the model a way to see its own screen; that was worth more than any other change we made.

What would you trade for it? I'd like to hear from anyone running a split like this on their own Macs, or deciding against it: is 0.13 of score and twice the wall clock a fair price for keeping the code at home?

Originally published on leanzero.net. More Atlassian, Forge and local-AI write-ups at leanzero.net/blog, and if you're planning a migration or a Forge app, that's what we do: leanzero.net/services.

## Top comments (0)
