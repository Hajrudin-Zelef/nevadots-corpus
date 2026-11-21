---
id: collect-261121-veille-ia/langages-dev/postgresql-19-delayed-to-november-19-as-ai-found-bugs-overwhelm-developers
title: "PostgreSQL 19 Delayed to November 19 as AI-Found Bugs Overwhelm Developers"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-11-19"]
keywords: ["postgresql", "november", "bugs", "release", "developers", "more", "september", "delayed"]
source: https://pbxscience.com/postgresql-19-delayed-to-november-19-as-ai-found-bugs-overwhelm-developers/
source_anchor: ""
source_lines: [1, 59]
section: "Langages_Dev"
---
## PostgreSQL 19 Delayed to November 19 as AI-Found Bugs Overwhelm Developers

For about a decade, PostgreSQL has shipped a new major version between mid-September and mid-October. This year that rhythm broke. PostgreSQL 19, originally expected in the autumn window, is now scheduled for general availability on November 19, 2026, and the project’s own developers say the main reason is a surge of bugs uncovered with large language models (LLMs).

### The Revised Schedule

On October 10, release team member Jonathan Katz told the developer mailing list that the general availability date was moving to November 19. Release Candidate 1 arrived on October 15 as planned. A second release candidate is due on November 12, the same day as the scheduled update releases for older branches, because those updates carry a large batch of fixes the team wants included in version 19 before it is declared final.

The official announcement adds a caveat: the November 19 date holds unless a new issue forces another delay or an additional release candidate.

### Late Reversals of Major Features

The delay came alongside an unusual number of late feature removals. Between August 27 and September 15, three large features were reverted from the release:

- **ALTER TABLE … MERGE PARTITIONS / SPLIT PARTITIONS** (August 27)
- **SQL Property Graph Queries (SQL/PGQ)** (September 7)
- **UPDATE/DELETE … FOR PORTION OF** , the temporal update and delete clause (September 15)

The Beta 4 announcement of September 24 also listed the reversal of online enabling and disabling of data checksums. Commentators have noted that several of the removed features are likely candidates to return in PostgreSQL 20.

The release announcement framed the decision as a matter of principle: PostgreSQL must be reliable first, and the project also values a predictable schedule.

### What the Numbers Show

Robert Haas, a long-time PostgreSQL committer, laid out the case in a blog post published on October 21. Comparing the period from feature freeze on April 8 to September 25 across two years, he found:

- Commits to the release branch under preparation rose from 724 in 2025 to 1,169 in 2026, an increase of more than 60%.
- Commits that also had to be back-patched to the previous stable branch rose from 248 to 433, an increase of more than 74%.

Haas argued that committers were about as careful as in earlier years, but that far more of their mistakes were now being found quickly by people running LLMs against the code and reporting the results.

### Why Human Reviewers Became the Bottleneck

Haas described two effects. First, with so many more bugs being found, fixing them simply takes longer. LLMs can draft fixes, but those fixes are not always correct or consistent with PostgreSQL’s standards, so humans still have to review and commit them.

    Time that would have been spent reviewing what went into the release was diverted to security issues and other bugs in the back branches, so deciding what to revert before shipping version 19 took longer.


That paraphrases his second point. He added that he does not consider the delay especially worrying, since it is short and stems from the project’s focus on quality. He was more concerned about the long-term picture: with forty years of bugs that humans failed to find but LLMs can surface in hours, it is unclear how projects will catch up and still innovate.

### A More Complicated Picture

AI-driven bug discovery is not the only explanation offered within the community. Reporting from InfoQ cited developer Peter Eisentraut, who said part of the problem was code quality and architecture in the reverted features themselves, and that a dense wave of security work from April to August pulled senior developers away from the extra review and testing normally done early in the beta period. He also pointed to PostgreSQL’s growing complexity.

Earlier in the cycle, Haas had posted a “scary patch contest” message highlighting patches that had needed an unusually large number of post-freeze fixes, including some involving data loss and access-control problems. The thread was an early sign that several features were in doubt.

Developers outside the project have also raised a practical concern: AI tools can generate plausible-looking reports that fall apart in the context of the whole system, so triaging real defects from false alarms has become a significant cost of its own.

### What It Means for Users

Anyone planning around PostgreSQL 19 should confirm that the specific features they want are still in the release notes, and many practitioners suggest testing on a release candidate and waiting for the first minor release before moving production systems. PostgreSQL 18 remains the established upgrade target in the meantime.

The larger lesson is about process rather than blame. When tools can find defects far faster than people can verify and fix them, projects will need to rethink how they triage reports, allocate reviewer time and schedule releases.

**Sources**

- Robert Haas, “Postgres 19 is late, and it’s AI’s fault,” Neon blog, October 21, 2026
- PostgreSQL Global Development Group: Beta 4 and RC 1 announcements; release team message “Updated PostgreSQL RC / GA dates,” October 10, 2026
- LWN.net, “PostgreSQL 19’s ‘scary patch contest’,” September 10, 2026
