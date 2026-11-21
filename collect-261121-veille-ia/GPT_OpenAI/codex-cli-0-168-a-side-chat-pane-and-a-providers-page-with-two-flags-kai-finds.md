---
id: collect-261121-veille-ia/gpt-openai/codex-cli-0-168-a-side-chat-pane-and-a-providers-page-with-two-flags-kai-finds
title: "Codex CLI 0.168: a side chat pane and a providers page, with two flags · Kai Finds"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["codex", "providers", "side", "pane", "model", "page", "notes", "recordings"]
source: https://kaifinds.sabaidev.la/finds/codex-cli-0-168-side-chat-pane-model-providers
source_anchor: ""
source_lines: [1, 113]
section: "GPT_OpenAI"
---
Coding agentsWorth it or skip it

# Codex CLI 0.168: a side chat pane and a providers page, with two flags

Codex CLI 0.168.0 adds a side conversation pane, a model providers page, audio recordings and animated GIFs. Two of them need flags. Here is who should update.

Codex CLI 0.168.0, OpenAI's terminal coding agent, adds a side conversation pane, a model providers page, audio recordings and animated GIF generation. The headline pane is behind a feature flag, and so is GIF generation. The providers page and audio support are described in the notes without one.

This piece is for developers and makers who already run Codex and want to know whether the update changes their week. It is based on the official release notes, not a hands-on test.

## What it does

The release notes list six new features. Here they are in order of how much they might matter to a solo builder.

**Model providers page.** You can browse model providers under Sessions, then Model providers, and choose the default for new chats. If you move between providers, this saves you from setting the choice every time. A related bug fix says open and resumed chats keep their saved model provider after you change the default, so changing it for new chats should not quietly rewrite old ones.

**Side conversation pane.** Enable `features.tui_workspace` and Codex shows a side conversation beside the main chat. You can drag the divider to resize the panes, or press leader `z` to zoom the focused pane. The practical use is a place for a side question without cluttering the main thread. The notes do not describe how the two conversations share context, so do not assume either way.

**Audio recordings.** With an audio-capable model, you can attach recordings. The notes add two details: Codex skips recordings that were confirmed rejected on later turns, and it uses clip duration to keep more short recordings in context.

**Animated GIFs.** Enable `features.image_generation_animation` to generate or edit animated GIFs, with optional frame count and timing controls.

**Terminal extras.** Fullscreen Codex displays image previews and pets in tmux on Ghostty and Kitty with `allow-passthrough all`. You can also set `tui.notification_method = "osc777"` for terminal notifications. Approval notifications show longer command previews and distinguish terminal input from network access requests.

The GitHub release page, where the full notes and changelog live, is linked at the end of this article.

The openai/codex/releases/tag/rust-v0.168.0 repository on GitHub

## Key facts

- Version: Codex CLI 0.168.0, published November 13 on GitHub.
- Side conversation pane: needs `features.tui_workspace` .
- GIF generation and editing: needs `features.image_generation_animation` .
- Model providers page: Sessions, then Model providers, sets the default for new chats.
- Audio recordings: for audio-capable models.
- Notifications: `tui.notification_method = "osc777"` for terminal notifications.
- Fixes include fuzzy file search weighting filename matches more heavily, local history and resume pickers filtering new threads by the signed-in ChatGPT account, and a Windows fix so idle local MCP servers leave filesystem I/O available to the rest of Codex.
- Docs: the app-server reference now covers listing model providers, creating custom gateways, and updating gateway, Bedrock and GCP settings.
- Licence: not stated in the material we have, so check the licence in the repository.

## Setup in 5 minutes

The release notes give no install or upgrade command, so there is no command block here. Use whichever way you installed Codex and check the repository for the current instructions. After you are on 0.168.0:

1. Open the Sessions area and find Model providers. Pick the default you want for new chats.
2. To try the side pane, enable `features.tui_workspace` . The notes do not say where the setting lives, so check the Codex docs for how you set feature flags. Once on, you should see a side conversation next to the main chat, with a divider you can drag.
3. Press leader `z` to zoom the focused pane.
4. For GIFs, enable `features.image_generation_animation` the same way.
5. If you want terminal notifications, set `tui.notification_method = "osc777"` .

## How it fits your workflow

**Switching providers.** Say you use one provider for quick edits and another for longer reasoning. A default for new chats means you set the starting point once. Because resumed chats keep their saved provider, you can return to an old thread without it silently changing model.

**A side thread during a long task.** You are mid-refactor in the main chat and want to ask something unrelated, like what a flag does. The side pane gives that question its own space. Whether it carries the main chat's context is not documented, so test it on something harmless before relying on it.

**Audio notes and quick assets.** If your model supports audio, you can attach a recording, such as a spoken bug description, instead of typing it out. For a landing page or a README, animated GIF generation, once enabled, could produce a small demo loop with frame count and timing controls. These are scenarios from the notes, not results anyone here has seen.

## Limits and open questions

- **Flags.** The side pane and GIF generation are off until you enable their flags. Flagged features can change, and the notes do not say when, or whether, they become defaults.
- **Audio and privacy.** The notes do not say where recordings are sent. Recordings presumably go to your model provider, but that is an assumption, so read the docs before attaching anything sensitive.
- **Side pane context.** Not documented in the notes.
- **Terminal support.** Image previews and pets are described for tmux on Ghostty and Kitty with`allow-passthrough all` . Other terminals are not mentioned.
- **Config warning.** If you explicitly configure`features.multi_agent` , Codex now warns and points to`agents.enabled` for disabling subagent tools.
- **Cost.** The notes say nothing about pricing or usage costs for audio or GIF generation.
- **Licence.** Check the licence in the repository before building on it.

## The bottom line

Codex 0.168.0 is a broad release with a few clear wins. The model providers page and audio support are the most practical additions for people who switch models, and neither is described as flag-gated. The side pane and GIF generation are the experimental parts, and each needs a flag.

If you use several providers or audio-capable models, update and look at the providers page first. If you only run one provider and want everything on by default, nothing here forces your hand.

Full notes: https://github.com/openai/codex/releases/tag/rust-v0.168.0

## Worth it, with caveats

### Worth it if

- You switch between model providers in Codex and want a default for new chats
- You want a side conversation next to the main chat and are fine enabling a flag
- You use an audio-capable model and want to attach recordings

### Skip it if

- You want the side pane or GIF generation without touching feature flags
- You only ever use one provider and never attach audio
- You need stable defaults and avoid experimental features

## FAQ

## How do I get the side chat pane in Codex 0.168.0?

Enable features.tui_workspace. The release notes say that shows a side conversation beside the main chat, with a draggable divider to resize the panes and leader z to zoom the focused pane.

## Do I need a flag for the model providers page?

The release notes do not mention one. They say you can browse model providers under Sessions, then Model providers, and choose the default for new chats.

## How do I generate animated GIFs?

Enable features.image_generation_animation. The notes say it lets you generate or edit animated GIFs, with optional frame count and timing controls.

## Does changing the default provider affect my existing chats?

According to the bug fix list, open and resumed chats use their saved model provider after you change the default for new chats.

## Sources

Based on the docs and sources above, not a hands-on test.

Kai Mercer · AI creator · Kai Finds
