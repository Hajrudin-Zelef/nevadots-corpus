---
id: collect-261121-veille-ia/gpt-openai/codex-0-161-flipped-the-default-model-to-gpt-6-1-sol-if-you-never-set-model-you-already-switched
title: "Codex 0.161 flipped the default model to GPT-6.1 Sol. If you never set model, you already switched."
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["model", "codex", "config", "default", "gpt-6", "catalogs", "bedrock", "catalog"]
source: http://dev.to/piekwerk/codex-0161-flipped-the-default-model-to-gpt-61-sol-if-you-never-set-model-you-already-switched-577b
source_anchor: ""
source_lines: [1, 37]
section: "GPT_OpenAI"
---
OpenAI shipped Codex CLI 0.161.0 on October 7. The release notes lead with one line: "GPT-6.1 Sol is now the default model in the bundled and Amazon Bedrock catalogs." Two pull requests, #49318 and #49339, no migration guide. If your config has a `model` line, nothing changed for you. If it does not, your sessions changed model this week while your config file still looks exactly the way it looked when you wrote it.

The flip lives in the catalogs, not in your config. #49318 added Sol as the default in the bundled catalog, #49339 did the same for the Bedrock catalog, which is why Bedrock shops got the same move on the same day. A catalog default is what resolves when no `model` key and no `--model` flag says otherwise. So the population that switched on October 7 is precisely the population that never chose: fresh installs, shared dotfiles without a model key, CI wrappers that call `codex exec` bare.

This is the part I find sharp. One release earlier, 0.160.0 merged "Treat explicit provider model catalogs as authoritative" (#49135), and a companion fix stopped explicit catalogs from reusing stale entries after refresh failures. Read the two releases together: first the catalogs became the source of truth, then the default inside them moved. The mechanism that was hardened on Monday was used to switch you on Tuesday. That is not a scandal, it is how defaults work. It is also why I treat every unpinned setting as a loan from the vendor, repayable in testing time.

Teams treat a missing setting as neutral. It is not. An unset `model` is a delegation: you told the vendor to pick, and they picked. Nobody on your team approved the pick, because there is no ticket for a line that does not exist. Maybe Sol is strictly better for your work. The release notes do not argue that, they do not mention performance at all, and I am not going to invent a benchmark here. The honest position is that your prompts, tools, and retry budgets were tuned against the old default, and the burden of proof now sits on whoever assumes nothing moved.

Run these in order.

First, find out what you actually resolve to. Start a session and read the model banner, or check what your profile prints:


```
codex --profile work
# read the model name in the header before you type anything
```
Second, check where a pin could live. Codex reads `~/.codex/config.toml`, with optional per-profile overrides:


```
# ~/.codex/config.toml
model = "gpt-6.1-sol"          # copy the exact id from your catalog,
                               # do not trust a blog post for the string
[profiles.slow]
model = "gpt-6.1"              # the heavier lane, pinned on purpose
```
Third, the version control test. If your Codex config lives only in `~/.codex` on each laptop, `git log -p` has nothing to say about it, and that silence is the finding. Config that can move without a diff is config you do not control. We run all our agent configs as reviewed, version-pinned files for exactly this reason, it is the core of what AgentConfig Studio ($29) ships, and the free Next.js sample shows the shape if you just want the pattern.

Two quieter items matter if you manage a fleet. `/mcp login <name>` (#49290) now completes MCP OAuth flows inside an active terminal session, which removes the worst part of onboarding a team onto internal MCP servers. And under the hood, enterprise MCP auth now "restricts" and "fails closed on config refresh" (#49260): a broken config refresh no longer silently degrades to a wider-open state. Fail-closed is the right default and the same direction Claude Code took with its hook guards, which I wrote about when 2.1.295 shipped. Bedrock shops also get multi-agent V2 and Ultra reasoning on compatible models (#49345).

Pin the exact model id where the choice matters, keep aliases only where you genuinely mean "whatever the vendor ships this quarter." A named model in a reviewed config is a decision with an owner and a diff. A default is a decision nobody signed. The same drift hit Copilot users this week when Haiku 5.5 went GA and org model policies quietly decided what runs subagents, four config surfaces decide that one too. Your model layer will keep moving on random Tuesdays. Your config layer is the part that can hold still, but only if you write down what you meant.

- openai/codex release 0.161.0, published October 7, 2026.
- openai/codex release 0.160.0, catalog authority change #49135.

## Top comments (0)
