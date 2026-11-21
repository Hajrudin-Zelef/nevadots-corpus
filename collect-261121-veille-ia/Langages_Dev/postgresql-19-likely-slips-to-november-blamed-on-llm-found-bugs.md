---
id: collect-261121-veille-ia/langages-dev/postgresql-19-likely-slips-to-november-blamed-on-llm-found-bugs
title: "PostgreSQL 19 likely slips to November, blamed on LLM-found bugs"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["postgresql", "author", "more", "bugs", "likely", "llms", "commits", "stable"]
source: https://glonce.com/postgresql-19-likely-slips-to-november-blamed-on/
source_anchor: ""
source_lines: [1, 45]
section: "Langages_Dev"
---
A blog post by a PostgreSQL developer, republished from neon.com, explains why PostgreSQL 19 is late. For the past decade, major releases have landed no earlier than mid-September and no later than mid-October. This year, the author writes, PostgreSQL 19 most likely won't be released until November, and it is not impossible that the project postpones it further.

The release also lost features late in the beta period. UPDATE/DELETE FOR PORTION OF, SQL Property Graph Queries (SQL/PGQ), and ALTER TABLE ... MERGE PARTITION/SPLIT PARTITION were all reverted between August 27 and September 15. The author notes that big reverts right after the annual feature freeze (April 8) are not unusual, but pulling several major features this late is.

The author's short answer to "what happened?" is that LLMs happened. PostgreSQL's code base is large, complex and old: the git history, converted from CVS, goes back more than 30 years, and the first code was written a decade before that. It has always contained a huge number of bugs. What has changed is that finding them has become vastly easier, while fixing them has gotten easier but not to the same degree.

The post backs this with commit counts for the period from April 8 to September 25. In 2025 there were 1037 commits to the master branch and 724 to REL_18_STABLE. In 2026, over the same dates, there were 1443 commits to master and 1169 to REL_19_STABLE. The author says the number of back-patched commits jumped by more than 60%. A second comparison covers older branches: 248 commits to REL_17_STABLE in 2025 against 433 to REL_18_STABLE in 2026, which the author puts at a jump of more than 74% in commits back-patched to the release in progress and at least one prior release. The stated reason is that a huge number of people are now reporting PostgreSQL bugs they found using LLMs, and the committers have been very busy fixing them.

The author allows that some of the rise may come from committers making more mistakes, but doesn't think anyone on the project believes that is the main cause. More likely, committers were about as conservative as in earlier years and LLMs quickly found a lot more of their mistakes.

Two factors link this to the late reverts and the delay. First, more bugs take more time to fix. LLMs can draft fixes, but those aren't always correct or compliant with PostgreSQL standards, so humans still have to review and commit them, and those humans "have not gotten any smarter between 2025 and 2026". Second, time that would have gone into reviewing what went into the release was diverted to security issues and other bugs in the back-branches, so it took longer to work out what had to be reverted before shipping version 19.

The author does not find the delay particularly concerning: it is not long in the grand scheme of things and comes from the project's concern for quality. The long-term picture is less clear. With forty years of bugs that humans couldn't find but LLMs can, the author asks how the project will catch up and keep innovating, and answers, "Right now, I'm not sure," adding that the author and many others are working hard on it. The author still rates the code base as some of the cleanest and highest-quality code they have worked on, and says the AI bug-finding success doesn't change that. The closing point is that LLMs will require every PostgreSQL developer, and to some extent every software developer, to raise their game, since features will be scrutinized by AI and flaws may be found within hours.

## Key facts

- PostgreSQL 19 will most likely ship in November, against a decade of releases landing between mid-September and mid-October; a further postponement is not ruled out.
- Three large features were reverted between August 27 and September 15: UPDATE/DELETE FOR PORTION OF, SQL/PGQ property graph queries, and ALTER TABLE ... MERGE/SPLIT PARTITION.
- From April 8 to September 25, REL_19_STABLE got 1169 commits in 2026 against 724 for REL_18_STABLE in 2025; the author reports back-patched commits up more than 60%.
- Commits to the prior release branch rose from 248 (REL_17_STABLE, 2025) to 433 (REL_18_STABLE, 2026), which the author calls a jump of more than 74%.
- The author attributes the surge to people reporting bugs found with LLMs, and says LLM-drafted fixes still need human review and don't always meet PostgreSQL standards.

## Why it matters

PostgreSQL's release timing has been steady for a decade, so a slip to November is a visible change for a core database project. The post is a first-hand account from a PostgreSQL developer of how LLM-assisted bug hunting is changing the workload of a mature open source project: finding bugs has become much easier, fixing them only somewhat easier. It also raises an open question the author says they can't yet answer, namely how the project will catch up on bugs and keep adding features.

## Who it affects

Teams planning upgrades or releases around PostgreSQL 19 should expect a later date than the usual autumn window. PostgreSQL committers and reviewers carry the extra load of bug reports and back-patching. The author says the effect reaches further: LLMs will require every PostgreSQL developer, and to some extent every software developer, to raise their game.

## How to use it

The post is an explanation, not a tool, so there is nothing to install. For anyone tracking PostgreSQL 19, the practical takeaway is to plan for a November release at the earliest. The three reverted features (FOR PORTION OF, SQL/PGQ, MERGE/SPLIT PARTITION) should not be counted on in version 19. Developers can take the author's closing advice that features will now be scrutinized by AI, and defects may surface within hours.

## How solid is it

This is a single author's interpretation, though a first-hand one from a PostgreSQL developer. The commit counts are concrete and the branch figures are consistent with the stated percentages: 1169 against 724 is about 61%, and 433 against 248 is about 75%. The causal link to LLMs is the author's reasoning, not something the post proves; the author states it flatly, as "simply" the reason for the surge in bug reports. Only the point that committers were about as conservative as before is hedged, with "seems likely". One internal wrinkle: the text says master-branch commits "declined slightly" year over year, yet the figures given are 1037 in 2025 and 1443 in 2026. The 60% figure is stated rather than derived from the listed numbers. The post is a republication from neon.com and does not name its author in its text.

## Risks and caveats

The release date is only "most likely" November, and further delay is not excluded. The post gives no count of bugs, security issues or CVEs, and doesn't say which LLMs or tools were used or who reported the bugs. It does not say whether the reverted features will return in a later release. No official project announcement or reactions from other members are quoted. The author also admits the long-term outlook is uncertain and "in some ways more troubling".

“Every feature that we write from now until the end of our careers will be scrutinized by AI”

— From the post, by a PostgreSQL developer
