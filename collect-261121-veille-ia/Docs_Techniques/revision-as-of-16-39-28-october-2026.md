---
id: collect-261121-veille-ia/docs-techniques/revision-as-of-16-39-28-october-2026
title: "Revision as of 16:39, 28 October 2026"
domain: docs-techniques
role: reference
task: article
actors: []
dates: ["2026-10-28"]
keywords: ["change", "wrangler", "issue", "release", "line", "assigned", "changes", "python"]
source: https://fedoraproject.org/w/index.php?title=Changes/Python3.16&diff=793212&oldid=791034
source_anchor: ""
source_lines: [1, 165]
section: "Docs_Techniques"
---
| No edit summary |  |  announcing the change |  | 
| (2 intermediate revisions by one other user not shown) |  |  |  | 
| Line 1: |  | Line 1: |  | 
|  | = Python 3.16 ~~<!-- The name of your change proposal -->~~ = |  | = Python 3.16 = | 
|  |  |  | {{Change_Proposal_Banner}} | 
|  | == Summary == |  | == Summary == | 
| Line 21: |  | Line 22: |  | 
|  | == Current status == |  | == Current status == | 
|  | [[Category:~~ChangePageIncomplete~~ ]] |  | [[Category:ChangeAnnounced]] | 
|  | <!-- When your change proposal page is completed and ready for review and announcement --> |  | <!-- When your change proposal page is completed and ready for review and announcement --> | 
|  | <!-- remove Category:ChangePageIncomplete and change it to Category:ChangeReadyForWrangler --> |  | <!-- remove Category:ChangePageIncomplete and change it to Category:ChangeReadyForWrangler --> | 
| Line 39: |  | Line 40: |  | 
|  | ON_QA -> change is fully code complete |  | ON_QA -> change is fully code complete | 
|  | --> |  | --> | 
|  | * [~~<link to~~ devel-announce~~post will be added by Wrangler>~~ Announced] |  | * [https://lists.fedoraproject.org/archives/list/devel-announce@lists.fedoraproject.org/thread/47DG37JHTXAM5TMPH575QQEM4DF2M4TX/ Announced] | 
|  | * [~~<will be assigned by the Wrangler> Discussion thread~~ ] |  | * [https://discussion.fedoraproject.org/t/f47-change-proposal-python-3-16-system-wide/206307/2 Discourse poll] | 
|  | * FESCo issue: <will be assigned by the Wrangler> |  | * FESCo issue: <will be assigned by the Wrangler> | 
|  | * Tracker bug: <will be assigned by the Wrangler> |  | * Tracker bug: <will be assigned by the Wrangler> | 
| Line 151: |  | Line 152: |  | 
|  | <!-- What work do other developers have to accomplish to complete the feature in time for release?  Is it a large change affecting many parts of the distribution or is it a very isolated change? What are those changes?--> |  | <!-- What work do other developers have to accomplish to complete the feature in time for release?  Is it a large change affecting many parts of the distribution or is it a very isolated change? What are those changes?--> | 
|  | * Release engineering: [https://~~pagure~~ .~~io~~ /releng/~~issue~~ /~~Releng issue number~~ ] <!-- REQUIRED FOR SYSTEM WIDE CHANGES --> |  | * Release engineering: [https://forge.fedoraproject.org/releng/tickets/issues/13552 #13552] <!-- REQUIRED FOR SYSTEM WIDE CHANGES --> | 
|  | <!-- Does this feature require coordination with release engineering (e.g. changes to installer image generation or update package delivery)?  Is a mass rebuild required?  include a link to the releng issue.   |  | <!-- Does this feature require coordination with release engineering (e.g. changes to installer image generation or update package delivery)?  Is a mass rebuild required?  include a link to the releng issue.   | 
|  | The issue is required to be filed prior to feature submission, to ensure that someone is on board to do any process development work and testing and that all changes make it into the pipeline; a bullet point in a change is not sufficient communication --> |  | The issue is required to be filed prior to feature submission, to ensure that someone is on board to do any process development work and testing and that all changes make it into the pipeline; a bullet point in a change is not sufficient communication --> | 

## Revision as of 16:39, 28 October 2026

# Python 3.16

## Summary

Update the Python stack in Fedora from Python 3.15 to Python 3.16, the newest major release of the Python programming language.

## Owner

- Name: Karolina Surma
- Name: Miro Hrončok
- Email: python-maint@redhat.com

## Current status

- Targeted release: Fedora Linux 47
- Last updated: 2026-10-28
- Announced
- Discourse poll
- FESCo issue: <will be assigned by the Wrangler>
- Tracker bug: <will be assigned by the Wrangler>
- Release notes tracker: <will be assigned by the Wrangler>

## Detailed Description

We would like to upgrade Python to 3.16 in Fedora 47 thus we are proposing this plan early.

See the upstream notes at What's new in 3.16.

### Important dates and plan

- 2026-05-07: Python 3.16 development begins
- 2026-10-13: Python 3.16.0 alpha 1
  - Package it as `python3.16` for testing purposes
  - Start the bootstrap procedure in Copr
  - Do a mass rebuild against every future release in Copr
- Package it as 
- 2026-11-10: Python 3.16.0 alpha 2
- 2026-12-15: Python 3.16.0 alpha 3
- 2027-01-12: Python 3.16.0 alpha 4
- 2027-02-09: Python 3.16.0 alpha 5
- 2027-02-09: Branch Fedora 46, Rawhide becomes future Fedora 47
  - The earliest point when we can start rebuilding in Koji side-tag
- 2027-03-09: Python 3.16.0 alpha 6
- 2027-04-13: Python 3.16.0 alpha 7
- 2027-05-04: Python 3.16.0 beta 1
  - No new features beyond this point
- 2027-05-25: Python 3.16.0 beta 2
  - The ideal point when we can start rebuilding in Koji
- 2027-06-01: Expected side tag-merge (optimistic)
- 2027-06-15: Python 3.16.0 beta 3
- 2027-06-15: Expected side tag-merge (realistic)
- 2027-06-29: Expected side tag-merge (pessimistic)
- 2027-07-13: Python 3.16.0 beta 4
- 2027-07-21: Fedora 45 Mass Rebuild
  - The mass rebuild happens with the fourth or third beta. We might need to rebuild Python packages later in exceptional case.
  - If the Koji side-tag is not merged yet at this point, we defer the change to Fedora 48.
- 2027-07-27: Python 3.16.0 candidate 1
  - This serves as "final" for our purposes.
- 2027-08-10: Branch Fedora 47, Rawhide becomes future Fedora 48
- 2027-08-10: Fedora 47 Change Checkpoint: Completion deadline (testable)
- 2027-08-24: Fedora Beta Freeze
  - If rebuild with 3.16.0rc1 is needed, we should strive to do it before the freeze - there is a window of 4 weeks.
- 2027-08-31: Python 3.16.0 candidate 2
- 2027-09-14: Fedora 47 Beta Release (Preferred Target)
  - Beta will likely be released with 3.16.0rc2.
- 2027-10-05: Python 3.16.0 final
- 2027-10-05: Fedora 47 Final Freeze
  - If the 3.16.0 final release is delayed, we'll update it using a freeze exception.
- 2027-10-19: Fedora 47 Final Target date #1
- 2027-10-26: Fedora 47 Final Target date #2

(From Python 3.16 Release Schedule and Fedora 47 Release Schedule.)

The schedule might appear somewhat tight for Fedora 47, but Python's annual release cycle was adapted for Fedora and this worked fine since Python 3.9 and Fedora 33. It is now common that Python is upgraded on a similar schedule in every odd-numbered Fedora release.

Note that upstream's "release candidates" are frozen except for blocker bugs. Since we can and will backport blocker fixes between Fedora and upstream, we essentially treat the Release Candidate as the final release.

## Feedback

## Benefit to Fedora

Fedora aims to showcase the latest in free and open-source software - we should have the most recent release of Python 3. Packages in Fedora can use the new features from 3.16.

There's also a benefit to the larger Python ecosystem: by building Fedora's packages against 3.16 while it's still in development, we can catch critical bugs before the final 3.16.0 release.

## Scope

We will coordinate the work in a side tag and merge when ready.

- Proposal owners:
  1. Introduce `python3.16` for all Fedoras
  2. Prepare stuff in Copr as explained in description.
  3. Update `python-rpm-macros` so`python3.16` builds`python3`
  4. Build `python3.16` as the main Python
  5. Mass rebuild all the packages that runtime require `python(abi) = 3.15` and/or`libpython3.15.so.1.0` (~3900 known packages in October 2026)
  6. Build `python3.15` as a non-main Python
- Introduce 

- Other developers: Maintainers of packages that fail to rebuild during the rebuilds will be asked, using e-mail and bugzilla, to fix or remove their packages from the distribution. If any issues appear, they should be solvable either by communicating with the respective upstreams first and/or applying downstream patches. Also, the package maintainers should have a look at: Porting to Python 3.16. The python-maint team will be available to help with fixing issues.

- Release engineering: #13552

- Policies and guidelines: N/A (not needed for this Change)

- Trademark approval: N/A (not needed for this Change)

- Alignment with the Fedora Strategy:

## Upgrade/compatibility impact

All the packages that depend on Python 3 must be rebuilt. User written Python 3 scripts/applications may require a small amount of porting, but mostly Python 3.15 is forward compatible with Python 3.16. Packages that fail to install because they failed to rebuild with Python 3.16 will be retired at Final Freeze unless they have a proposed Freeze Exception.

## Early Testing (Optional)

Do you require 'QA Blueprint' support? No

## How To Test

Interested testers do not need special hardware. If you have a favorite Python 3 script, module, or application, please test it with Python 3.16 and verify that it still works as you would expect. If the application you are testing does not require any other modules, you can test it using `python3.16` even before this change is implemented, in Fedora 43, 44, 45 or 46.

In case your application requires other modules, or if you are testing an rpm package, it is necessary to install the 3.16 version of the python3 rpm. That rpm will shortly be available in Copr, along with all other python packages that build successfully with python 3.16. See https://copr.fedorainfracloud.org/coprs/g/python/python3.16/ for detailed instructions on how to enable Python 3.16 Copr for mock.

Once the change is in place, test if your favorite Python apps are working as they were before. File bugs if they don't.



## User Experience

Regular distro users shouldn't notice any change in system behavior other than the Python 3 interpreter will be in version 3.16.

## Dependencies

~4300 packages depend on Python 3 and ~3900 packages need rebuilding when Python is upgraded. See scope section.

## Contingency Plan

- Contingency mechanism: Do not merge the side tag with rawhide. If the side tag has been merged and issues arise, that will justify a downgrade, then use an epoch tag to revert to 3.15 version (never needed before)
- Contingency deadline: Beta Freeze
- Blocks release? Yes, we'd like to block Fedora 47 release on at least 3.16.0rc1
