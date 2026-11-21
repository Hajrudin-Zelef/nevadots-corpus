---
id: collect-261121-veille-ia/ia-agents/the-agentic-ai-foundations-four-new-projects-turn-vendor-neutral-into-a-six-step-process-and-two-don
title: "The Agentic AI Foundation’s Four New Projects Turn “Vendor-Neutral” Into a Six-Step Process, and Two Donors’ Own Applications Say They Still Lead Their Projects - SXZ.io"
domain: ia-agents
role: reference
task: article
actors: []
dates: []
keywords: ["projects", "foundations", "four", "project", "process", "vendor-neutral", "donors", "agentic"]
source: https://sxz.io/aaif-four-new-projects-vendor-neutral-six-step-process-donors-still-lead/
source_anchor: ""
source_lines: [1, 119]
section: "IA_Agents"
---
# The Agentic AI Foundation’s Four New Projects Turn “Vendor-Neutral” Into a Six-Step Process, and Two Donors’ Own Applications Say They Still Lead Their Projects

The Agentic AI Foundation announced four projects as vendor-neutral, but its own process puts the legal transfer after a board vote its tracker had not recorded for three of them, and two donors’...

The Agentic AI Foundation (AAIF) announced four new projects on October 22: Agent Skills from Anthropic, Open Responses from OpenAI, OpenEnv from Hugging Face and OpenSandbox from Alibaba. The Linux Foundation’s press release describes the ten projects the foundation now hosts as “all operating under AAIF’s open, vendor-neutral governance,” and The Register reported that the four donors “have handed over control” of them. We read the foundation’s own repositories, proposals tracker and charters to see what that means in practice. “Vendor-neutral” turns out to be the end of a written sequence of steps rather than a status, and the four projects are at different points in it.

Three findings stand out. The foundation’s six-step process puts the legal transfer of trademarks, assets and repositories after the Governing Board vote, and on October 23 its tracker showed no Governing Board approval and an unsigned contribution agreement for three of the four projects. Two donors’ own applications say the donor still leads the project today. And the charter Open Responses merged on the day of the announcement dropped a rule that had kept any single vendor below a majority of its core-maintainer seats. The tracker labels may lag the paperwork, and none of this shows a donor misusing its position. It does mean the neutral-governance claim describes an end state these projects are still moving toward.

## What the foundation says joining requires

The foundation’s Project Lifecycle Policy, last updated October 15, is explicit about the entry price. Projects are required to “have their existing project host transfer project trademarks and other project assets to the LF as a requirement to join the AAIF,” and to “adopt a technical charter, in the form provided by the LF or as approved by the LF, that specifies the intellectual property policy for the project and how decisions will be made.” (The LF is the Linux Foundation.)

The policy sets three stages with different bars. Sandbox needs “at least one actively committing maintainer, with documented intent to grow the contributor base,” and says “Sandbox status is not an endorsement, and every Sandbox project’s README must state this explicitly.” Growth needs a “growth plan to demonstrate diverse maintainership” and documented production use by two unaffiliated organizations. A project that joins directly at Impact must meet the Growth criteria plus “a diverse group of maintainers, with multiple organizations represented on the project’s governing body” and “a healthy number of committers from at least two organizations.”

The sequence that delivers all this is the Project Proposal Process (version 0.3, effective August 28). It has six steps: intake, a Technical Committee (TC) question window, a TC vote, a Governing Board vote, legal paperwork and technical onboarding. The legal transfer comes late. “Following GB approval, LF Legal, LF PMO, and Project Submitter execute the contribution agreement, technical charter, and transfer of trademarks and other required project assets,” and within four weeks of that signature the submitter and the Linux Foundation’s IT team “Transfer GitHub repository ownership to the Linux Foundation, or grant LF IT administrative control.” The process also says the TC recommends a stage in its vote and that the Governing Board “confirms it on approval.” So the stage labels on the three projects that have not yet passed the board are recommendations.

### Who decides: the eight Platinum members

The same eight companies sit on both bodies that decide. The foundation’s members page shows eight Platinum logos (Amazon Web Services, Anthropic, Block, Bloomberg, Cloudflare, Google, Microsoft and OpenAI), prices Platinum membership at $350,000 with the note “(Not Available),” and promises a “Guaranteed seat on the Governing Board.” Gold membership costs $200,000 and carries “Nomination rights for a shared Gold seat on the Governing Board through election.” The TC’s charter says “Each Platinum Member of AAIF may appoint one representative to the TC,” and its README lists eight members from exactly those eight companies, chaired by David Soria Parra of Anthropic with Caitie McCaffrey of Microsoft as co-chair. The Governing Board page lists those companies’ representatives plus Shopify’s Gold member representative and a senior adviser from the Gates Foundation.

The foundation-wide charter, amended July 29, says “The governance of each Technical Project is as set forth in the charter for that project,” so these committees decide admission and stage rather than roadmaps. It also adds a structural rule for admission. For a three-year “Startup Period,” adding a top-level project needs two-thirds of the entire Governing Board and also “the approval of all Contributing Platinum Members,” defined as “those Platinum Members that have contributed at least one top-level Technical Project to the Directed Fund.” Read literally, any Platinum member that has donated a top-level project can block the next one during that period.

Two procedural details matter for neutrality. The TC charter allows decisions by “lazy consensus,” in which a proposal “is automatically approved if no formal objection from a TC voting member is recorded within 7 days,” and in the TC charter, the lifecycle policy, the proposal process and the foundation charter we found no recusal or conflict-of-interest language. Against that background, the Open Responses application was opened by OpenAI’s TC representative, Nick Cooper, who is also its listed “Technical Committee Sponsor,” and the Agent Skills application names the TC chair, David Soria Parra, as one of its two maintainers and application contacts. Nothing in the rules we could find prohibits either arrangement, because the rules do not address it.

## Four projects against the checklist

The foundation keeps the official record of each proposal as an issue in its project-proposals repository, and each issue records labels, comments and dates. We pulled all four issues, then read each project’s governance files, package metadata and commit history. In this tracker’s vocabulary, an *Approved* label replaces *GB Vote* once the Governing Board has voted (the closed Agent2Agent issue shows that sequence). The table reflects what we saw at 10:58 UTC on October 23.

| Project (donor) | Stage label | Application to TC approval | Tracker labels, October 23 | Governance text in the repository | Maintainers named | 
|---|---|---|---|---|---|
| Agent Skills (Anthropic) | Impact | Sept 24 to Oct 1 | TC Approved, GB Vote, contribution-agreement/unsigned | None yet; the application says GOVERNANCE.md and MAINTAINERS.md “will be published during onboarding” | Two, both at Anthropic | 
| Open Responses (OpenAI) | Impact | Aug 21 to Sept 25 | TC Approved, GB Vote, contribution-agreement/unsigned | LF Projects technical charter, merged October 22 (UTC) | Nine from seven organizations; OpenAI holds two | 
| OpenEnv (Hugging Face) | Growth | Sept 28 to Oct 6 | TC Approved, GB Vote, Paperwork-in-review, contribution-agreement/unsigned | GOVERNANCE.md last updated June 10, with no mention of AAIF | Nine: three Hugging Face, four Reflection, one Meta, one Microsoft | 
| OpenSandbox (Alibaba) | Sandbox | May 5 to July 22 | Approved, contribution-agreement/signed, TC Approved | GOVERNANCE.md “describes how OpenSandbox is governed today”; no AAIF charter text found | Nine named in the application, all Alibaba-affiliated | 

Read against the six steps, OpenSandbox has finished the board vote and the signature and is inside the four-week technical-onboarding window. The other three have finished the TC vote and show no recorded board outcome, so by the tracker’s own record they were announced before the legal transfer step.

### Agent Skills (Anthropic): the name and domain move first, the governance comes later

Anthropic’s announcement says it is donating “the Agent Skills name and logo, the GitHub organization, and the agentskills.io domain,” with engineers from Anthropic, Google, OpenAI and other companies maintaining the standard “as individuals” and two lead maintainers, David Soria Parra and Mahesh Murag. The September 24 application is blunter about the starting point: “Anthropic leads the project today, and this contribution exists to bring other implementers into its governance.” It adds that “Today the maintainers listed above are all at Anthropic. That changes with this contribution,” that “There is no separate steering body,” and that “GOVERNANCE.md and MAINTAINERS.md will be published during onboarding.” Anthropic also “funds and operates all project infrastructure,” including the GitHub organization, the domain, documentation hosting and the Discord server.

The tracker lists the project as Impact, the stage whose written bar includes a diverse group of maintainers and multiple organizations on the governing body. The application answers that with a plan: the steering committee’s “composition and decision rules will be settled with the Linux Foundation during onboarding.” Adoption is the case for the higher stage, since Anthropic says more than 70 agent platforms have integrated the standard, a vendor figure we did not verify. But on the day of the announcement the evidence for the governance half of the Impact bar was a promise.

Two details show how the foundation’s own pages trail the donation. The Agent Skills page on aaif.io links to github.com/anthropics/skills, Anthropic’s own skills repository, and its statistics list “First Commit October 15, 2025,” which is the date of the first commit in that repository. The standard itself lives in agentskills/agentskills, whose history starts on December 16, 2025 and whose last commit is dated August 9, 2026. Its commits are also less Anthropic-heavy than its governance: 6 of 109 come from anthropic.com addresses, and the largest committer, with 55 commits, lists Vercel on their GitHub profile.

### Open Responses (OpenAI): a vendor cap in the application is missing from the adopted charter

Open Responses started in its own GitHub organization with a community charter. That January charter said that “All governance roles are held by individuals, not organizations. No seats are reserved for specific companies,” that “No single vendor may control a majority of Core Maintainer seats,” and that if the steering committee could not resolve a dispute, “the Lead Core Maintainer will act as the tie-breaker.” The August 21 application, opened by OpenAI’s Nick Cooper, repeated the cap: “a single vendor may not hold a majority of core-maintainer seats.”

The charter adopted on October 22 (UTC) in a pull request titled “Adopt final LF Projects technical charter” contains neither rule. We searched the new charter and the CONTRIBUTING file for “lead,” “vendor,” “single,” “individuals” and “seat” and found none of them. Deadlocks now go elsewhere: “any voting member of the TSC may refer the matter to the Series Manager for assistance in reaching a resolution.” The pull request was opened at 01:04 UTC and merged by its author at 04:11 UTC, with no review or discussion recorded. The roster is still broad, with nine maintainers from seven organizations and two from OpenAI, so no vendor holds a majority today. The cap that would keep it that way is no longer written down, and the stated mission remains a specification “built on the OpenAI Responses API.”

### OpenEnv (Hugging Face): a multi-company committee, a named technical lead and a repository that moved

OpenEnv’s GOVERNANCE.md, last updated June 10, describes a “multi-company Technical Committee” of ten organizations (Meta-PyTorch, Reflection, Nvidia, Microsoft, Prime Intellect, Mercor, Fleet AI, Modal, Unsloth and Hugging Face) and says that “In the early stages of the project, Ben Burtenshaw acts as the BDFL of the project in technical matters.” The foundation’s press release identifies Burtenshaw as a Hugging Face engineer. The application lists nine maintainers (three at Hugging Face, four at Reflection, one at Meta and one at Microsoft) and names Microsoft’s Caitie McCaffrey, a TC member, as sponsor.

The repository is no longer where the governance file says it is. The file still describes the project as “Hosted at huggingface/OpenEnv,” and we found no mention of AAIF or the Linux Foundation in it. GitHub now redirects that name to openenv/OpenEnv, in an organization created on July 20. The documentation and environment hub stay on Hugging Face (the application says environments “are hosted on the Hugging Face Hub today”), the PyPI project `openenv` lists a single maintainer, and the foundation’s OpenEnv page showed a health score of 0 and zero contributors when we checked, which looks like a statistics feed not yet pointed at the new repository.

### OpenSandbox (Alibaba): approved at Sandbox stage, with package names still under Alibaba’s

OpenSandbox is the only one of the four whose tracker entry shows both final approval and a signed agreement. Its application, filed May 5, asked for Growth stage and was candid about the starting point: “Current maintainership and code ownership are primarily Alibaba-affiliated,” and the project saw AAIF participation “as an opportunity to mature toward neutral, multi-stakeholder governance.” The TC set Sandbox stage on July 22, the Approved label followed on October 18, 166 days after the application, and the signed-agreement label on October 21. The repository has already left Alibaba’s GitHub organization (it lives in opensandbox-group, created June 4), and the foundation’s project page reports 90 contributing organizations.

The installable pieces have not moved yet. The README says OpenSandbox is “an open source project under the Linux Foundation, contributed by Alibaba.” But its JavaScript SDK is published as `@alibaba-group/opensandbox`, its C# package as `Alibaba.OpenSandbox` (authors “Alibaba Group”) and its Go module path is `github.com/alibaba/OpenSandbox/sdks/sandbox/go`. The latest npm release, 1.1.1 on October 9, came from one of two individual maintainer accounts, and the Python package lists an alibaba-inc.com contact address. Release images already go to three places: Docker Hub, GitHub Container Registry under opensandbox-group, and an Alibaba Cloud registry. Whether package namespaces count as “other project assets” depends on the signed agreement, which is not public, and under the foundation’s process the technical onboarding is due within four weeks of signing. The unchanged names are a checklist for that window more than evidence of a problem. One written requirement can be checked today: the policy says every Sandbox project’s README must state that Sandbox status is not an endorsement, and the OpenSandbox README does not yet.

## What the commit logs add

Governance seats and code are different things, so we also counted commits. On October 23 we made blobless clones of each repository’s default branch and counted non-merge commits by author email domain, up to 11:00 UTC. The donor domains were anthropic.com (Agent Skills and MCP), openai.com (Open Responses), huggingface.co and hf.co (OpenEnv), and alibaba-inc.com, alibaba.com and alibabacloud.com (OpenSandbox).

| Project | Commits | From donor domains | Last 90 days (since July 25) | Donor domains, last 90 days | Largest committer | 
|---|---|---|---|---|---|
| Agent Skills | 109 | 6 (5.5%) | 7 | 0 | 55 commits from one author whose GitHub profile lists Vercel | 
| Open Responses | 53 | 13 (24.5%) | 7 | 0 | 22 commits, under two personal-address names, from the maintainer the roster lists at OpenAI | 
| OpenEnv | 1,753 | 7 (0.4%) | 455 | 7 | 400 commits, personal address, from a maintainer the application lists at Hugging Face | 
| OpenSandbox | 2,887 | 1,549 (53.7%) | 1,434 | 671 (46.8%) | 863 commits from one alibaba-inc.com address | 
| MCP (donated December 2025, for comparison) | 3,455 | 1,184 (34.3%) | 227 | 92 (40.5%) | 437 commits from a lead maintainer | 

Two readings follow. First, donor share of code and donor share of governance are different quantities. Agent Skills has donor-led governance and a specification repository where 6 of 109 commits carry the donor’s domain, and OpenEnv’s Hugging Face domain appears on only 7 of 1,753 commits even though a Hugging Face maintainer is its largest committer on a personal address, which is why the domain count is a lower bound. Second, donor weight need not fade after a handoff: ten months after Anthropic donated MCP, anthropic.com addresses are on 40.5% of its last-90-day commits, and OpenSandbox sits at 46.8%.

### Reproduce the count

You need Python 3.13 and Git. Clone each repository without file contents, then run the script from the folder that holds the clones. The cutoff is fixed, so the numbers reproduce unless a repository’s history is rewritten.

```
git clone --bare --filter=blob:none https://github.com/opensandbox-group/OpenSandbox.git OpenSandbox.git
python commit_domains.py opensandbox-group/OpenSandbox alibaba-inc.com alibaba.com alibabacloud.com
```
```
import subprocess
import sys
repo, *domains = sys.argv[1:]  # for example: opensandbox-group/OpenSandbox alibaba-inc.com alibaba.com
until, since90 = "2026-10-23T11:00:00Z", "2026-07-25"
log = subprocess.run(
    ["git", "log", "--no-merges", f"--until={until}", "--format=%aI %ae"],
    cwd=repo.split("/")[-1] + ".git", capture_output=True, text=True, check=True,
).stdout.splitlines()
rows = [line.split() for line in log if line]
def donor(email):
    return email.lower().endswith(tuple("@" + d for d in domains))
recent = [r for r in rows if r[0] >= since90]
print(f"{repo}: {len(rows)} commits, {sum(donor(e) for _, e in rows)} from donor domains; "
      f"last 90 days: {len(recent)} commits, {sum(donor(e) for _, e in recent)} from donor domains")
```
The OpenSandbox run prints the following, which matches the table.

`opensandbox-group/OpenSandbox: 2887 commits, 1549 from donor domains; last 90 days: 1434 commits, 671 from donor domains`
## What this record cannot tell you

Four limits apply. The tracker labels are applied by hand and may lag: three projects were announced while still carrying “contribution-agreement/unsigned,” and the agreements themselves are not public. The email-domain count is a lower bound because personal and noreply addresses hide employers; we read GitHub profile company fields only for the people named here, and those fields are self-reported. The foundation’s pages change quickly, and the OpenEnv statistics gap could be fixed within hours. And we tested governance, not engineering: nothing here says anything about the quality of the four specifications.

## What to watch next

- **Agent Skills:** the promised GOVERNANCE.md and MAINTAINERS.md, the named members of its steering committee, and whether the foundation’s page switches to the agentskills repository.
- **Open Responses:** whether the steering committee restores a written limit on any one vendor’s share of seats, and who sits in the nine voting seats after the roster is next revised.
- **OpenEnv:** an updated GOVERNANCE.md that reflects the move to openenv/OpenEnv and the foundation, and who holds ownership of the PyPI project and the GitHub organization.
- **OpenSandbox:** whether the npm scope, NuGet package and Go module path move by the end of the four-week window, and whether the README gains the Sandbox disclaimer.
- **The foundation:** the Approved and signed labels appearing on the other three issues, and how the startup-period approval rule for Platinum members is used on the next batch of proposals.

The same questions apply to other handoffs in this part of the stack. Docker’s Sandbox Kit Specification is moving to the CNCF, and our June report on OpenEnv described the first step of this one, the move to committee coordination at Hugging Face. For readers who use these formats today, our guide to writing an AGENTS.md file covers the one the foundation has hosted since its launch.

## No Comment! Be the first one.
