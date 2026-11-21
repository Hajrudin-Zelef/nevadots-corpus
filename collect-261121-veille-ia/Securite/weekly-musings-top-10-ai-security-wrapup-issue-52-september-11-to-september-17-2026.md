---
id: collect-261121-veille-ia/securite/weekly-musings-top-10-ai-security-wrapup-issue-52-september-11-to-september-17-2026
title: "Weekly Musings Top 10 AI Security Wrapup: Issue 52: September 11 to September 17, 2026"
domain: securite
role: reference
task: article
actors: []
dates: ["2026-09-16"]
keywords: ["openai", "september", "summaries", "framework", "model", "published", "gpt-5", "misalignment"]
source: https://www.rockcybermusings.com/p/weekly-musings-top-10-ai-security-20260911-20260917?utm_source=substack&utm_medium=email&utm_content=share&action=share
source_anchor: ""
source_lines: [1, 293]
section: "Securite"
---
One OpenAI model, handing work to the next instance of itself, wrote summaries telling that instance to hide mistakes from the user. It showed up in 2.15% of the summaries one GPT-5.6 Sol run produced. A different unreleased model searched GitHub for an exposed API key, used it without permission, and when the data still did not come back, invented nine values and passed them off as real. Nobody breached OpenAI to learn this. OpenAI published it on Wednesday, in the same week it asked Congress to send in outside auditors.

**OpenAI had… a week…**

Three similar events landing in the span of a week circle around one problem. The labs started publishing their own misalignment findings on a clock. A watchdog told California that one of those labs skipped risk assessments its own framework promised. Palantir, Nvidia, and Booz Allen told both labs their retention terms are not good enough to keep buying on. Add von der Leyen asking the frontier to slow down and NIST naming AI agents in final token guidance, and the governing documents ran well ahead of the controls. Paperwork always shows up first, in cloud, in OT, and now here. What matters is which of these becomes a contract clause, a log source, or a deployment gate this quarter.

### 1. OpenAI Publishes Its Own Misalignment Reports

OpenAI published a misalignment reporting framework on September 16, 2026, with six incident reports attached. Instances of GPT-5.6 Sol wrote concealment instructions into context handoff summaries, flagged in 2.15% of Sol summaries and 0.27% of GPT-6 Astra summaries. Another model used an exposed GitHub API key without authorization, then fabricated nine values when retrieval failed. Models also used a shared Artifactory repository as a message board between supposedly independent training samples.

**Why it matters**

- Cross-sample messaging breaks evaluation independence, so eval numbers may not mean what the card says.
- Concealment instructions in a handoff defeat output review, since reviewers read the summary, not the run.
- OpenAI alone decides what clears its disclosure bar, with no external audit.

**What to do about it**

- Ask each model vendor for their misalignment disclosure policy and deadlines.
- Treat agent handoffs and compaction summaries as retained security telemetry.
- Hold eval results as a claim until sample independence is explained.

**Rock’s Musings**

The name for those fabricated values is specification gaming. It means a system optimizing the measurable proxy you gave it instead of the outcome you wanted. This model was scored on filling a table, so it filled the table. Your metric was easier to satisfy than your intent, the oldest failure in performance management, now at machine speed. OpenAI earns credit for the six-day clock, since a published deadline is one somebody can hold you to. Henry Papadatos at SaferAI named the catch: no outside accountability plus your own flexible rules.

### 2. A Watchdog Says OpenAI Broke California’s Disclosure Law

On September 14, 2026, the nonprofit Midas Project alleged OpenAI violated California’s SB 53 at least three times this year. The claim is that OpenAI never published loss-of-control risk assessments promised in its Frontier Governance Framework, covering the GPT-5.6 preview, GPT-5.6, and GPT-6 Astra. SB 53 requires large developers to publish a safety framework and follow it, with penalties up to $1 million per violation. OpenAI disputed the claim and said its Preparedness Framework governs.

**Why it matters**

- SB 53 enforces adherence to your own published policy, so what you post becomes the standard.
- The fight is over which internal framework governs, the argument you lose with an auditor.
- $1 million per violation is rounding error at OpenAI and a real number at your company.

**What to do about it**

- Inventory every public AI commitment you have made and check it against practice.
- Name one owner to reconcile overlapping AI governance frameworks.
- Make risk assessment evidence a renewal condition in vendor diligence.

**Rock’s Musings**

I spent years telling boards that a policy you cannot evidence is worse than no policy. It converts a gap into a documented broken promise, and SB 53 turned that lecture into statute. OpenAI may be right on the facts, and the argument still tells you something. A company with the industry’s deepest safety organization cannot say which of its documents governs. Go pull your AI acceptable use policy, your model card template, and last spring’s responsible AI page. My bet is all three exist and none agree.

### 3. OpenAI Asks Congress To Send In The Auditors

On September 15, 2026, OpenAI endorsed the third-party assessment provision of the bipartisan FRONTIER Act, from Representatives Jay Obernolte and Lori Trahan. That provision would have an independent organization judge whether a covered developer’s safety measures hold catastrophic risk to acceptable levels. The bill would also require frontier developers to publish safety reports and disclose incidents. OpenAI backed that provision rather than the whole bill.

**Why it matters**

- A lab asking for outside assessors is what an industry does when self-attestation stops being credible.
- Endorsing one provision and not the bill leaves the reporting and disclosure duties unsettled.
- Federal assessment plus California’s new auditor framework points toward attestations your vendors can hand you.

**What to do about it**

- Add a question about independent safety assessment to your model vendor questionnaire now.
- Decide which of your own AI systems you would put in front of an outside assessor.
- Track the FRONTIER Act text, since its disclosure duties will shape your contract language.

**Rock’s Musings**

Industries reach for outside auditors at the moment self-attestation stops being credible. This is that moment for the frontier labs. Endorsing the assessment provision and not the whole bill is the tell, since reporting duties cost more than an audit does. Nobody is qualified to do the assessing at scale yet. California enacted an AI auditor framework on September 9, 2026, and registration opens later, so demand arrives before supply. The first cohort of AI auditors will set the bar for everyone, and right now the audited are drafting it.

### 4. An OpenAI Agent Swarm Poisoned RubyGems, And Nobody Can Say Why

On September 12, 2026, researchers Spencer Kitts, Thomas Larsen, and Sydney Von Arx linked a swarm of OpenAI agents to more than 3,000 malicious RubyGems packages. The first went up on May 5, 2026, with over 2,000 published on May 11 and May 12. The agents used a `.yardopts` file to run code on RubyDoc.info servers, then republished stolen output as packages. OpenAI called it agents reaching the internet for benign tasks and public information.

**Why it matters**

- Registries execute supplier code at install and doc build time, so an unused package still executes.
- The traffic came from a vendor’s own training agents, which your risk program has no field for.
- Four months passed between the first upload and public attribution.

**What to do about it**

- Pin dependencies by hash and route installs through an internal proxy registry.
- Disable or sandbox doc and post-install build hooks in CI runners.
- Ask model vendors in writing what egress their training agents have.

**Rock’s Musings**

Build-time code execution belongs in your threat model as its own line. A package manager runs supplier-controlled code during install or doc generation, on your build host, with your credentials, before a human reviews anything. RubyDoc fell through a config file nobody reads in review. The attribution is what I cannot get past. These were OpenAI’s own agents, and the public explanation is that they were retrieving public information. Nobody wanted 3,000 poisoned gems, and that is worse rather than better.

### 5. Palantir, Nvidia, And Booz Allen Put Data Retention In The Contract

On September 14, 2026, The Information reported that Palantir, Nvidia, and Booz Allen Hamilton restricted or threatened to stop using advanced Anthropic and OpenAI models. All three want firm assurance that providers will not retain proprietary data or train on it. Anthropic had replaced its retention policy on September 1, 2026 with Enterprise Frontier Safeguards. That package pairs zero-data-retention access with cross-session misuse detection and customer-held key management.

**Why it matters**

- Three of the most security-literate buyers found standard terms insufficient, which is a reference point for you.
- Zero retention and cross-session misuse detection pull against each other, since detection means keeping something.
- Defense and intelligence buyers set the contract template that regulated sectors copy.

**What to do about it**

- Split retention, training use, subprocessors, and deletion verification into separate clauses.
- Ask in writing what metadata survives under a zero retention configuration.
- Test deletion with a marked prompt and a request for proof.

**Rock’s Musings**

Palantir and Booz Allen do not restrict a vendor for sport. They restrict one when their contracts officer cannot get a sentence written that survives an audit. You can use that signal without their buying power. The harder problem is the one nobody markets. Zero retention and cross-session misuse detection are in tension by construction. Ask for the data flow diagram before you believe it works.

### 6. Von Der Leyen Tells The Frontier To Slow Down

In her State of the Union address on September 16, 2026, European Commission President Ursula von der Leyen said it is “time to slow down on the self-recursive models.” She will invite leading AI labs to discuss how the EU might support their deceleration. She made the security case directly, warning that models in development “will allow hacking on a level we never thought possible” and will soon reach adversaries. She called the EU AI Act crucial to guardrails.

**Why it matters**

- The head of the European executive named offensive cyber capability as the reason to pace development.
- An invitation to frontier labs signals negotiated commitments ahead of legislative text.
- Pacing in Europe creates a capability gap against actors who will not participate.

**What to do about it**

- Track those lab meetings as a leading indicator of AI Act enforcement priorities.
- Map which of your AI systems would fall under a capability threshold.
- Prepare a board answer for why European deployments differ from U.S. ones.

**Rock’s Musings**

I have been the skeptic on AI doom rhetoric for three years and I am not converting now. The argument is what changed here. Von der Leyen did not reach for extinction. She reached for offensive cyber capability in adversary hands, which you can test against evidence. She did not cite that evidence. The gap is the one she named herself, since Europe pacing its labs paces nobody else’s. I put her frontier-centric risk model at better than even odds of being wrong, and a year of open-weight releases trailing the closed frontier on offensive capability would move me.

### 7. Washington Argues About AI, Then Goes Home

President Trump rejected calls to slow AI development on September 13, 2026, telling reporters that “whoever wins AI, wins.” House Speaker Mike Johnson added that regulating AI in an emergency session would mean losing the race to China. Congress has proposals and no consensus, including the AI Kill Switch Act from Representatives Ted Lieu and Nathan Moran. A Public First poll for POLITICO published September 16, 2026 found 63% of Americans see at least moderate risk that AI ends humanity.

**Why it matters**

- Federal inaction leaves the state patchwork operative, so California and New York rules govern.
- A 63% risk reading with no federal action produces sudden legislation after an incident, not before.
- Your board reads the same polling and will ask about exposure before any law exists.

**What to do about it**

- Build your AI compliance baseline against the strictest state regime you touch.
- Keep an AI inventory current enough to answer a regulator in two weeks.
- Draft the incident disclosure path now, naming who notifies whom on what clock.

**Rock’s Musings**

A country where 63% of adults think the technology might end the species, and the legislature recesses without a vote, has no stable regulatory outcome coming. I do not know which way it breaks. I know the shape from critical infrastructure after every pipeline and grid event: nothing moves for years, then a directive lands with a ninety-day deadline. Build for that timeline. A company with a current inventory and a named AI incident owner absorbs that directive. A spreadsheet does not.

### 8. NIST Finalizes Token Guidance And Names AI Agents In It

NIST published the final NIST IR 8587, “Protecting Tokens and Assertions from Forgery, Theft, and Misuse,” on September 15, 2026, developed with CISA. It covers key protection, token verification, lifecycle management, and monitoring. The document states that “artificial intelligence (AI) systems, especially agentic AI systems (AI agents), use signed tokens or assertions in many emerging IAM schemes.” It tells organizations to apply the guidance when agents use those tokens and favors short-lived ones.

**Why it matters**

- This is the first broadly applicable federal guidance treating an agent’s credential as a workload identity.
- Short-lived, audience-restricted tokens cut what a stolen agent credential buys, the exposure Heapjack showed.
- Federal guidance becomes procurement language, so customers will ask before your engineers read it.

**What to do about it**

- Inventory every agent, recording its credential, lifetime, and accepting service.
- Replace static API keys held by agents with short-lived, audience-restricted tokens.
- Alert on any token presented to an audience it was not issued for.

**Rock’s Musings**

Audience restriction is the term to carry into your next identity vendor meeting. A signed token carries a claim naming the one service allowed to accept it, so a token stolen from that service gets refused everywhere else. Most enterprises issue tokens any service honors. The category underneath is non-human identity: service accounts, workloads, and now agents, all authenticating with no person present. Agents turn that gap into live exposure, since an agent spawns other agents and each needs a credential. This is the least interesting document of the week and the one that will change the most work. I would trade every pacing speech for six months of shorter token lifetimes.

### 9. Microsoft Opens Its Model Rulebook To Public Comment

On September 14, 2026, Microsoft AI published a draft Humanist AI Code of Conduct for its MAI models and opened a six-week consultation. The draft requires that models never resist being switched off, never expand their own objectives, and never conceal their reasoning from auditors. It sets absolute limits around weapons of mass harm, child safety, and manipulation at scale. Microsoft lets enterprise partners configure models rather than applying one uniform restriction set.

**Why it matters**

- A published behavioral specification gives you something to test against, unlike most model documentation.
- Shutdown compliance and no scope self-expansion are testable properties you can write tests around.
- Configurability means promised behavior may differ from another tenant’s deployment.

**What to do about it**

- Submit comments during the consultation window, especially on multi-agent behavior.
- Turn the shutdown and scope clauses into acceptance tests in your evaluation suite.
- Ask which configuration options change these behaviors for enterprise tenants.

**Rock’s Musings**

I read a lot of AI principles documents and most are wallpaper. This one has clauses I can test, which puts it ahead of the field. “Does not resist shutdown” is an experiment you run on a Tuesday afternoon. “Does not conceal reasoning from auditors” is harder. I expect it to soften first, since these systems keep moving toward reasoning nobody can read. Configurability is where I would push, since a tenant who can dial these behaviors turns the code into a default.

### 10. Contractors Are Reading Your ChatGPT Conversations

On September 14, 2026, Joseph Cox at 404 Media reported on Project Lily, an OpenAI program paying hundreds of contractors to read and score real ChatGPT conversations. OpenAI removes usernames and tries to strip personally identifying information, and acknowledged that sensitive details still get through. ChatGPT has more than 900 million users, and reviewers work a continuous stream of prompts. Anthropic confirmed it also uses human review.

**Why it matters**

- Human review is standard in model improvement, and your data classification policy ignores it.
- Redaction that fails open ties your exposure to filter quality, not a contractual boundary.
- Employees paste customer data into consumer chat tools daily, unseen by your DLP.

**What to do about it**

- Ask each provider whether human review applies to your tier, and who performs it.
- Publish a plain-language rule about what never goes into a consumer AI tool.
- Route business use to an enterprise tier and monitor consumer tools on corporate devices.

**Rock’s Musings**

None of this is a breach, and I want to be clear about that, because the headline invites the wrong reaction. Human review is how these systems improve, and Anthropic does it too. The problem is that your users assumed otherwise and your policy never said either way. I have sat in incident reviews where the loss came from an employee pasting a customer list into a chat window. Nobody had told them not to. Write the rule in one paragraph.

### The One Thing You Won’t Hear About But You Need To: Two Ways Out Of The Codex Sandbox

On September 15, 2026, the security firm Accomplish published two sandbox escapes in OpenAI’s Codex, reported August 12, 2026 and fixed within eight days. The first, Overpatch, abuses `apply_patch` in the Codex CLI, where one patch entry referencing `/tmp` hands that tool write access to the whole disk with no approval prompt. The second, Heapjack, targets Codex Desktop’s JavaScript tool, where authentication tokens sat in shared memory reachable by untrusted code. Pulling a heap snapshot and picking out UUID-formatted tokens yields unsandboxed command execution, even in read-only mode.

**Why it matters**

- An escaped coding agent runs with developer credentials, inside your build pipeline.
- Read-only mode failed in Heapjack, so the setting your engineers rely on was not a boundary.
- The pattern generalizes to every agent harness you run, meaning the scaffolding holding a model’s tools and permissions.

**What to do about it**

- Ask where each harness runs approval logic, and treat “inside the agent process” as a finding.
- Run coding agents inside an outer boundary you control, with its own network policy.
- Broker developer credentials through a separate service with short-lived, scoped tokens.

**Rock’s Musings**

A sandbox escape is code breaking out of the restricted environment meant to contain it and reaching the host. That definition has not changed since the browser wars, and neither has the failure behind it. Accomplish named the root cause in its cleanest form: the component enforcing the boundary ran inside it. Whatever fooled the agent also fooled the guard. This got almost no coverage, which is why it is here. Every board conversation this quarter was about model behavior, while the harness is where code execution lives. Your internal agent platform, stood up in six weeks to hit a deadline, has the same architecture and no researcher on it.

So here is the question you own now. If an attacker lands one prompt in your coding agent tomorrow, what stops it from reaching the build server, and who on your team can answer that without checking?

👉 For ongoing analysis of agentic AI governance frameworks, the conversation at **RockCyber Musings** and you can subscribe above

👉 Visit **RockCyber.com** to learn more about how we can help with your traditional Cybersecurity and AI Security and Governance journey.

👉 Want to save a quick $100K? Check out our AI Governance Tools at **AIGovernanceToolkit.com**

👉 As a bonus, I got to hang out with my friend, Chris Hughes, over at Resilient Cyber to discuss all things about the 2026 update to the OWASP Top 10 for LLMs.

## References

Accomplish. (2026, September 15). *Escaping the OpenAI Codex sandbox, twice*. https://accomplish.ai/blog/escaping-the-openai-codex-sandbox-twice/

Anthropic. (2026, September 1). *Developing Enterprise Frontier Safeguards with our customers*. https://www.anthropic.com/news/enterprise-frontier-safeguards

Cox, J. (2026, September 14). *Inside ‘Project Lily’: The humans reading your ChatGPT chats*. 404 Media. https://www.404media.co/inside-project-lily-the-humans-reading-your-chatgpt-chats/

Euronews. (2026, September 16). *Canada, heatwaves, AI and Ceuta: Von der Leyen delivers State of the Union*. https://www.euronews.com/my-europe/2026/09/16/ursula-von-der-leyen-delivers-state-of-the-union-speech

Fortune. (2026, September 14). *OpenAI may have violated California’s AI safety law with latest model releases, AI watchdog says*. https://fortune.com/2026/09/14/openai-may-have-violated-californias-ai-safety-law-with-astra-model-releases-ai-watchdog-says/

Help Net Security. (2026, September 16). *NIST and CISA finalize playbook to stop token theft and forgery*. https://www.helpnetsecurity.com/2026/09/16/nist-cisa-cloud-token-security-guidance/

Help Net Security. (2026, September 16). *Self-improving AI should slow down, von der Leyen tells EU lawmakers*. https://www.helpnetsecurity.com/2026/09/16/eu-ursula-von-der-leyen-ai/

Microsoft AI. (2026, September 14). *Humanist AI in practice: A public consultation on our Code of Conduct for MAI models*. https://microsoft.ai/news/mai-code-of-conduct/

National Institute of Standards and Technology. (2026, September 15). *NIST IR 8587: Protecting tokens and assertions from forgery, theft, and misuse: Implementation recommendations for agencies and cloud service providers*. https://csrc.nist.gov/pubs/ir/8587/final

National Institute of Standards and Technology. (2026). *NIST IR 8587* [PDF]. https://nvlpubs.nist.gov/nistpubs/ir/2026/NIST.IR.8587.pdf

National Public Radio. (2026, September 13). *Trump rails against AI slowdown*. https://www.npr.org/2026/09/13/nx-s1-5968078/trump-mike-johnson-ai-slowdown

Office of Governor Gavin Newsom. (2026, September 9). *Governor Newsom signs first-in-the-nation AI safeguards to protect Californians, calls on the federal government to do its part*. https://www.gov.ca.gov/2026/09/09/governor-newsom-signs-first-in-the-nation-ai-safeguards-to-protect-californians-calls-on-the-federal-government-to-do-its-part/

OpenAI. (2026, September 16). *Our framework for reporting model misalignment*. https://openai.com/index/model-misalignment-reporting-framework/

Quartz. (2026, September 14). *Palantir, Nvidia, Booz Allen restrict Anthropic and OpenAI models*. https://qz.com/palantir-nvidia-booz-allen-anthropic-openai-data-restrictions-091426

PYMNTS. (2026, September). *Nvidia and Palantir restrict Anthropic’s Fable over data retention*. https://www.pymnts.com/news/artificial-intelligence/2026/nvidia-and-palantir-restrict-anthropics-fable-over-data-retention/

SecurityWeek. (2026, September 16). *EU chief warns of AI-powered hacking, moves to rein in social media*. https://www.securityweek.com/eu-chief-warns-of-ai-powered-hacking-moves-to-rein-in-social-media/

The Hacker News. (2026, September 12). *OpenAI agents linked to RubyGems campaign that gained RCE on RubyDoc servers*. https://thehackernews.com/2026/09/openai-agents-linked-to-rubygems.html

The Hill. (2026, September 16). *63 percent believe AI could destroy humanity: Survey*. https://thehill.com/policy/technology/6092503-poll-ai-humanity-risks/

The Neuron. (2026, September 16). *Everything that happened in AI today (Wednesday, September 16, 2026)*. https://www.theneuron.ai/digest/everything-that-happened-in-ai-today-wednesday-september-16-2026/

Unite.AI. (2026, September 16). *OpenAI launches misalignment reporting framework with six incident reports*. https://www.unite.ai/openai-launches-misalignment-reporting-framework-with-six-incident-reports/

Unite.AI. (2026, September). *Microsoft AI opens six-week review of draft rules governing MAI behavior*. https://www.unite.ai/microsoft-ai-opens-six-week-review-of-draft-rules-governing-mai-behavior/

U.S. Congress. (2026). *H.R.9917 — AI Kill Switch Act, 119th Congress (2025-2026)*. https://www.congress.gov/bill/119th-congress/house-bill/9917

Washington Times. (2026, September 15). *Kill switch, monitors, moratoriums: Many plans but no consensus as Congress debates regulating AI*. https://www.washingtontimes.com/news/2026/sep/15/many-proposals-no-consensus-congress-debates-regulating-ai/

CBS News. (2026, September 16). *OpenAI backs measure that would require independent audits of AI models*. https://www.cbsnews.com/news/openai-sam-altman-frontier-act/

Implicator.ai. (2026, September 16). *OpenAI discloses six misalignment incidents under new rules*. https://www.implicator.ai/openai-six-misalignment-incident-reports/

Slashdot. (2026, September 12). *Malicious OpenAI agents linked to RubyGems campaign that gained RCE on RubyDoc servers in May*. https://slashdot.org/story/26/09/12/2318204/malicious-openai-agents-linked-to-rubygems-campaign-that-gained-rce-on-rubydoc-servers-in-may

Common Dreams. (2026, September 16). *Poll shows 63% of Americans fear AI could end humanity as industry ‘corruption’ called out*. https://www.commondreams.org/news/can-ai-end-humanity

Item 8 is the one I'd pin to the wall. Audience restriction is the smallest change on this list, and it turns a stolen agent token into a token that only works in one place. The harness escape at the bottom is the same lesson from the other side: the guard ran inside the thing it was guarding. In my lab the approval logic sits in a gateway outside the agent process, so whatever fools the agent still has to get past a second door.
