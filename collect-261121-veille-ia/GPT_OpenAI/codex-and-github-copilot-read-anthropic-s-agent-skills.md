---
id: collect-261121-veille-ia/gpt-openai/codex-and-github-copilot-read-anthropic-s-agent-skills
title: "Codex and GitHub Copilot Read Anthropic's Agent Skills"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: ["2026-10-21"]
keywords: ["agent", "skills", "foundation", "open", "responses", "openai", "anthropic", "spec"]
source: https://www.beri.net/article/agntcon-mcpcon-2026-recap-aaif-open-responses-agent-skills-opensandbox-openenv-multi-provider-portability
source_anchor: ""
source_lines: [1, 96]
section: "GPT_OpenAI"
---
**Two of the four projects the Agentic AI Foundation took in at AGNTCon + MCPCon are ready to write into a gateway, agent framework or sandbox RFP this quarter, and two are not.** Open Responses, OpenAI's request format, now has a charter, an Apache 2.0 license and an acceptance test suite you can point at any vendor's endpoint. Anthropic's Agent Skills is a short, stable folder format that OpenAI's Codex and GitHub Copilot already read. Alibaba's OpenSandbox and Hugging Face's OpenEnv are useful code at early version numbers (OpenSandbox 1.1.1, OpenEnv v0.10.0), and the foundation's own announcement gives none of the four a version number.

The news itself is simple. On October 22, in San Jose, the Agentic AI Foundation (AAIF) announced that Anthropic contributed Agent Skills, OpenAI contributed Open Responses, Hugging Face contributed OpenEnv and Alibaba contributed OpenSandbox. That takes the foundation to 10 hosted projects, up from three when it launched in December 2025. The same day, AAIF said it had passed 300 members, counting 305 organizations, with MongoDB, Royal Bank of Canada and Vercel among the new Gold members and NIST among the new Associate members.

What follows is the part a press release leaves out: which of these interfaces you can test today, where the listed supporters fall short of the spec, and what to ask vendors for before your next renewal.

## What Did AAIF Actually Take In at AGNTCon 2026?

AAIF took in a request format, a skills packaging format, a sandbox platform and a reinforcement learning environment framework, and they sit at four different layers of an agent stack. The foundation is a directed fund of the Linux Foundation, and the four new projects join MCP, AGENTS.md, goose, A2A, Agent Router and agentgateway under its vendor-neutral governance.

The event was large enough to matter. The Linux Foundation's event page lists 3,500+ attendees from 1,000+ organizations and 150+ talks across 11 tracks, and notes the conference was formerly called MCP Dev Summit.

Here is where each project stands, from its own pages:

| Project | Contributed by | What it standardises | Release status on its own page | License | 
|---|---|---|---|---|
| Open Responses | OpenAI | The request and response schema for calling a model, streaming and tool calls | Dated spec snapshots, acceptance tests, charter adopted Oct 21, 2026 | Apache 2.0 (code), CC BY 4.0 (docs) | 
| Agent Skills | Anthropic | A folder with a `SKILL.md` file that packages instructions, scripts and resources | Published spec plus a `skills-ref` validator | Not stated on the spec page | 
| OpenSandbox | Alibaba | A sandbox lifecycle and execution API with Docker and Kubernetes runtimes | Latest release 1.1.1 (Oct 9), 1.2.0 in release candidate | Apache 2.0 | 
| OpenEnv | Hugging Face | Containerized RL environments with a Gym-style API | v0.10.0 (Oct 22); "Early development, so APIs may still change" | BSD-3-Clause | 

The table's sources: the Open Responses governance charter and repository, the Agent Skills specification, the OpenSandbox repository and the OpenEnv repository, all read on October 24, 2026.

## Is Open Responses Ready to Write Into a Gateway RFP?

Yes, as a test you can run, which is the useful part. Open Responses is an open specification for calling language models across providers, based on the OpenAI Responses API, covering a shared schema for "items" (the unit of model output and tool use), streaming events and tool calling.

Three things make it more than a donated document.

First, it has governance with teeth. The technical charter, adopted October 21, 2026, sets up a Technical Steering Committee whose Chair is the contact with AAIF, puts trademarks with LF Projects, and requires a two-thirds TSC vote to amend the charter. OpenAI no longer owns the name.

Second, the spec is versioned by date. The repository keeps each spec release in a dated folder under `public/openapi/` and says those snapshots are immutable. That gives you something precise to cite in a contract: a dated snapshot rather than "compatible with the OpenAI API".

Third, it ships acceptance tests. Ten run in the browser against any base URL and key: basic text, streaming, system prompts, tool calling, image input, multi-turn history, two compaction-endpoint checks and two phase-labelling checks. Seven more run only from the CLI and cover WebSocket behaviour, including continuing a `store:false` response with `previous_response_id` and recovering when that ID is gone. The repository gives the command: `bun run test:compliance --base-url http://localhost:8000/v1 --api-key $API_KEY`.

### Where the supporter list and the spec diverge

The Open Responses homepage lists NVIDIA, Vercel, OpenRouter, Hugging Face, LM Studio, Databricks, Red Hat, AWS, Ollama, OpenAI, vLLM and Llama Stack. Being on that list does not mean an endpoint passes every test, and the providers' own documentation shows the gaps.

Ollama's OpenAI compatibility docs say its `/v1/responses` endpoint, added in v0.13.3, supports streaming, function tools and reasoning summaries, but only the "non-stateful flavor": no `previous_response_id`, no `conversation` and no `truncation`. Continuation through `previous_response_id` is exactly what the CLI suite tests. FuriosaAI's Furiosa-LLM docs say it "implements the OpenResponses specification", then list what it does not support: audio and file inputs, every built-in tool (web search, file search, code interpreter, computer use, MCP), and `background=true`, which is accepted but has no effect.

Both projects publish their limits plainly, which is to their credit. They also show that "supports Open Responses" can mean anything from the full suite to a stateless subset, so your gateway contract should say which.

This matters more after this week. LiteLLM, the open-source gateway, shipped a backdoored release on October 21. If every model behind your gateway speaks one tested request format, swapping the gateway becomes a configuration change, which is the position you want to be in the next time a supply chain incident forces you to move fast. We covered the managed side of that decision in AI gateway vs API management.

## Why Agent Skills Is the Safest Bet of the Four

Agent Skills is a folder containing a `SKILL.md` file with YAML frontmatter and markdown instructions, plus optional `scripts/`, `references/` and `assets/` directories that an agent loads only when a task needs them. The specification is short. Two fields are required: `name` (1 to 64 lowercase letters, numbers and hyphens, matching the folder name) and `description` (up to 1,024 characters, saying what the skill does and when to use it). Optional fields cover `license`, `compatibility`, `metadata` and an experimental `allowed-tools` list. A reference library validates a skill with `skills-ref validate ./my-skill`.

The case for standardising on it now is adoption by the products your engineers already run. OpenAI's Codex documentation says "Skills build on the open agent skills standard" and reads them from `.agents/skills` in the repo, `$HOME/.agents/skills` for a user and `/etc/codex/skills` for an administrator. GitHub's Copilot docs say skills work in the Copilot cloud agent, Copilot code review, the Copilot CLI, the Copilot app and agent mode in VS Code, and read project skills from `.github/skills`, `.claude/skills` or `.agents/skills`. `gh skill publish` validates against the agentskills.io spec.

So a skill your platform team writes once, for example "how we cut a release" or "how to file a change request in our ITSM", can live in one repo directory and load in Codex, GitHub Copilot without a rewrite. Google is moving its own users the same way: we covered Workspace's Gems to Skills migration earlier this month.

### The risk is the `scripts/` folder

A skill can carry executable code, and the spec leaves which languages run, and with what permissions, to each agent. `allowed-tools`, the field that pre-approves tools for a skill, is marked experimental, with support that "may vary between agent implementations". Treat a third-party skill as you would a third-party package. Pin it, review the scripts, and keep it out of agents that hold production credentials. We made the same argument about MCP servers in MCP server governance: enforce at the client, because a registry listing proves nothing about what the code does.

## Why OpenSandbox and OpenEnv Belong in Pilots for Now

OpenSandbox is the more interesting of the two for enterprise buyers, because it offers one open API across your own Docker hosts and Kubernetes clusters. Its repository lists SDKs in Python, Java/Kotlin, TypeScript, C#/.NET and Go, an `osb` CLI, an MCP server, per-sandbox egress policies and a Credential Vault that lets an agent call external services without the sandbox seeing the real credentials. The repo has 16.1k stars.

Read the caveats before you pilot it. The isolation story depends on the runtime: the README names Firecracker microVMs for its Fast Sandbox runtime on Kubernetes, with self-reported creation times of 64 ms at P50, but gives no threat model for the plain Docker runtime. A container sharing a host kernel is a weaker boundary than a microVM, and coding agents run untrusted code by design. The project does sign its official images with Cosign and recommends pinning production images by digest, which you should do.

If you already pay for a hosted sandbox, compare OpenSandbox's lifecycle API with what you have before you move anything. Our E2B vs Modal vs Daytona comparison found idle time, more than isolation, drives the bill, and Nvidia's OpenShell reference architecture from GTC Berlin is another option in the same layer.

OpenEnv matters less to most enterprise platform teams today. It is a framework for reinforcement learning environments that run as Docker images and expose `reset()`, `step()` and `state()`, plus MCP tools at `/mcp`. Its README says it is "in early development, so APIs may still change". If you fine-tune agents with RL, follow it. Hugging Face's Ben Burtenshaw named the companies behind it, including Microsoft, Meta-PyTorch, Modal and Prime Intellect. If you don't, it can wait.

## What Vendor Neutrality Does Not Settle

A neutral home settles who controls the spec, and leaves open which vendor's extensions your code ends up depending on. Open Responses is OpenAI's API shape, and OpenAI keeps shipping hosted features (built-in tools, server-side state, compaction) that a self-hosted server may never implement, as the Furiosa and Ollama limits show. The strongest argument against standardising early is that you will write to the common subset and then quietly lean on one provider's extras anyway. Teams that built on OpenAI's Assistants API went through a version of this when it was retired; we covered that migration in August.

The defence is to test the subset you actually use. If your agents never call a built-in tool and always send full history, a stateless implementation is fine, and you have more providers to choose from. If you rely on `previous_response_id`, you have fewer, and the acceptance suite will tell you which. A shared protocol also leaves fleet upgrades to you, as IBM's A2A platform showed.

## What to Do Before Your Next Gateway or Framework Decision

**This Week:**

1. Run the Open Responses acceptance suite against every model endpoint your gateway routes to, including self-hosted ones on Ollama or vLLM, and write down which of the 17 tests each one passes.
2. List the Responses API features your agents actually call (stateful continuation, built-in tools, background mode, compaction). That list, cross-checked against the test results, is your portability map.
3. Grep your repos for `.agents/skills` ,`.github/skills` and`.claude/skills` . Engineers are probably already writing skills; find out where before you set a policy.

**This Month:**

1. Pick one internal workflow that three or more teams repeat, write it as an Agent Skill, run `skills-ref validate` on it, and load it in both Codex and Copilot to confirm it behaves the same.
2. Set a review rule for any skill with a `scripts/` folder: who approves it, and which agents may load it.
3. If you run your own code-execution sandboxes, stand up OpenSandbox on a non-production Kubernetes cluster on the Firecracker runtime, and test its egress policies against your existing sandbox's.

**Before Renewal:**

1. Ask your gateway and model vendors which dated Open Responses snapshot they conform to and for their acceptance-test results. The suite is public and free to run, so a refusal is worth noting in the evaluation.
2. Ask agent-framework vendors whether they load Agent Skills from a standard directory, and whether `allowed-tools` is enforced or ignored.
3. Leave OpenSandbox and OpenEnv out of contract language until OpenEnv's APIs settle and OpenSandbox documents a threat model for its Docker runtime.

## The Bottom Line

A vendor format that moves into a Linux Foundation body and gains a conformance test becomes something a buyer can put in a contract. Open Responses and Agent Skills have reached that point: a charter, a validator or test suite, and adoption in products you already license. OpenSandbox and OpenEnv have the neutral home and the code, but OpenEnv still warns its APIs may change and OpenSandbox gives no threat model for its Docker runtime, so for now they belong in pilots. Run the 17 tests before you sign the next gateway contract.
