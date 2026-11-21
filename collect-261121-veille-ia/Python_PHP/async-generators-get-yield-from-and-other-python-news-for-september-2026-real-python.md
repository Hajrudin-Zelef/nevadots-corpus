---
id: collect-261121-veille-ia/python-php/async-generators-get-yield-from-and-other-python-news-for-september-2026-real-python
title: "Async Generators Get yield from and Other Python News for September 2026 – Real Python"
domain: python-php
role: reference
task: article
actors: []
dates: []
keywords: ["python", "async", "yield", "generators", "then", "news", "real", "them"]
source: https://realpython.com/python-news-september-2026/
source_anchor: ""
source_lines: [1, 233]
section: "Python_PHP"
---
August was a month for reading release notes rather than writing new code. Python 3.15 stopped accepting changes to its application binary interface, and three older branches got security patches with real CVEs behind them. Four separate AI libraries shipped breaking releases in ten days, and the last of them knocked over a downstream framework, which then took about nine hours to ship a fix.

If you maintain anything that pins a dependency, then this was less a month of new toys and more a month of checking what you’re standing on.

There was one new piece of syntax to look forward to, though, and it closes a gap that has irritated everyone writing asynchronous code for years. On August 3, Yury Selivanov, the Steering Council’s appointed delegate for the PEP, accepted **PEP 828**.

**Join Now:** Click here to join the Real Python Newsletter and you’ll never miss another Python tutorial, course, or news update.

Sync generators have had `yield from` since Python 3.3. Async generators never got an equivalent, so delegating to another async generator has always meant writing the loop by hand:

```
async def wrapper():
    async for item in numbers():
        yield item
```
That works right up until it doesn’t. The manual loop only forwards values in one direction. If a caller uses `.asend()` to push a value back into `wrapper()`, or `.athrow()` to raise inside it, or `.aclose()` to shut it down, then none of that reaches `numbers()`. The loop swallows it. Worse, there’s no way to capture what the inner generator returned, because `StopAsyncIteration` has never carried a value.

PEP 828 fixes both problems by allowing `yield from` inside async generators, starting in Python 3.16:

```
async def numbers():
    yield 1
    yield 2
    return "done"
async def wrapper():
    result = yield from numbers()
    print(result)
```
The delegation becomes the interpreter’s job. Values, exceptions, and close requests all propagate through to the inner generator the way they do in sync code, and the PEP adds a `.value` attribute to `StopAsyncIteration` so the `return` statement finally means something.

**Note:** If the difference between `.asend()` and a plain `async for` is new to you, then Python’s asyncio: A Hands-On Walkthrough and How to Use Generators and yield in Python cover both halves of what PEP 828 is joining together.

The PEP’s tracking issue closed on August 7, targeting **Python 3.16**. That’s a wait, but this is the kind of change that deletes a category of bug rather than adding a feature you have to learn.

Three releases, seven new PEP drafts, a rejection, and a withdrawal. The releases matter more than usual this month because two of them close doors rather than open them: one freezes an interface for the rest of the 3.15 series, and the other patches holes that have gone unfixed considerably longer than anyone would like. The PEPs are the opposite, all possibility and no obligation, which makes them the fun part.

Python 3.15.0rc1 arrived on August 4, exactly on the schedule August’s Python news laid out. The headline isn’t a feature, because the feature set has been settled since the freeze back in May. It’s that the application binary interface, or ABI, is now locked for the entire 3.15 series. From here, only reviewed bug fixes land.

For most people, that’s trivia. If you ship a C extension module, then it’s the moment your build either works or doesn’t, and you have until October to find out.

That contract between compiled extensions and the interpreter puts a lot of weight on three letters. If you want it unpacked, then Quansight’s tour of the CPython ABI remains the clearest walkthrough of what’s being promised, and we covered the free-threaded stable ABI in May’s Python news.

The release candidate bundles what 3.15 has been assembling all year: `frozendict`, `sentinel`, explicit lazy imports, UTF-8 by default, unpacking in comprehensions, a JIT worth roughly 8 to 9 percent on x86-64 Linux, and official macOS installers that now ship the free-threaded build by default.

Release candidate 2 followed on September 1, and PEP 790 schedules the final release for October 1. That last date is a target rather than a promise, but the direction is set. If you’ve been meaning to test against 3.15, then the excuse that it’s still changing expired on August 4.

On August 12, Python 3.12.14, 3.11.16, and 3.10.21 shipped together, and this batch has teeth. CVE-2026-4224 covers unbounded recursion in XML parsing, and CVE-2026-3644 covers control characters slipping into HTTP cookies.

Alongside those sit a symlink bypass in the `tarfile` extraction filter, a Windows-only path bug in `shutil.unpack_archive()` for ZIP files, buffer issues in `bz2` and `lzma`, and an FTP `PASV` validation flaw that completes a fix first issued back in 2021.

The `tarfile` one is worth pausing on because it’s a hole in the safety net itself, not in the risky path you’d expect. Here’s the call that’s supposed to be safe:

```
import tarfile
with tarfile.open("archive.tar.gz") as tar:
    tar.extractall("output/", filter="data")
```
The `data` filter exists precisely so that extracting an untrusted archive is the safe thing to do. A crafted archive could slip a symlink pointing outside the destination directory past the filter, bypassing a fix that had already been issued once as CVE-2025-4330. This release closes that and hardens the filter’s own validation.

So if you’re working with files or ZIP archives from anywhere you don’t control, then patch rather than reason about whether you’re exposed.

**Note:** Not sure whether you’re on the current patch release? Should You Update to the Latest Python Bugfix Version? explains why the last number in the version matters more than most people assume.

A week earlier, 3.14.7 and 3.13.15 landed as routine bugfix releases, carrying roughly 499 and 400 fixes, respectively. Boring, and that’s the point.

The month’s other memorable bug didn’t get a CVE, but it has the better reproducer, one that fits in two lines:

```
__conditional_annotations__ = 0
a: 1
```
That’s the whole program. On any Python with lazy annotation evaluation, which means 3.14, 3.15, and the 3.16 development branch, it segfaulted the interpreter until the type check landed.

It still does on the current 3.14 because the type check that prevents the crash reached that branch about four hours after 3.14.7 was tagged, so the fix won’t ship until 3.14.8. Python 3.15.0rc1 and the main branch already have it. We watched those two lines take down a 3.14 interpreter while we were checking.

The write-up of the crash traces it back to PEP 649, which made annotations lazy, and to its implementation companion PEP 749. Part of the bookkeeping for lazy annotations involves a module-level set named `__conditional_annotations__` that records which annotated assignments actually ran. To add to that set, the compiler reuses the `SET_ADD` opcode, which was written for set comprehensions.

Inside a comprehension, the target is a real set that was built on the stack moments earlier and can’t be touched by user code, so `SET_ADD` skips the type check and casts the pointer straight to a set. PEP 749 gave the opcode a second caller that loads its target by name, and names can be rebound. Assign an integer to `__conditional_annotations__`, and the opcode starts dereferencing that integer’s fields as hash table pointers.

Lydxn reported the crash on July 30, and the fixes landed in early August: a dedicated intrinsic on the main branch and a type check added to `SET_ADD` itself for 3.14 and 3.15, wrapping up the report on August 7.

The moral is tidier than the crash. That unchecked cast wasn’t a mistake when it was written, because the invariant behind it was real. The invariant stopped being true the moment the opcode picked up a second caller, and nothing in the language forces anyone to notice.

PEP authors had a productive summer, and seven new drafts have appeared since we last checked in:

- **PEP 841** proposes`f{...}` syntax for frozen collections, echoing f-strings. Instead of`frozenset({1, 2, 3})` building a set and then freezing it at runtime,`f{1, 2, 3}` collapses to a single constant at compile time whenever its elements are all constants themselves. It pairs naturally with`frozendict` , which was covered in March’s Python news, and with immutable-by-default habits in free-threaded code.
- **PEP 837** standardizes custom JSON serialization through a`__json__()` method, a`copyreg.json()` registry, and a per-encoder dispatch table, replacing the usual dance of writing a`default=` function or subclassing`JSONEncoder` .
- **PEP 843** adds an`export` soft keyword so that a library can re-export a name without typing it twice, once in the import and again as a string in`__all__` . Writing`from ._internal.core export PublicAPI` does both jobs at once. Neil Girdhar is the author, and Peter Bierma is the sponsor. Bierma turns up again a few paragraphs down under less happy circumstances.
- **PEP 844** approaches the same problem from the other end, adding`public` and`private` builtins.
- **PEP 840** tackles name resolution in class namespaces,**PEP 838** records`python-version` in`pyvenv.cfg` , and**PEP 839** adds`PyFrozenSetWriter` and`PyFrozenDictWriter` so that C extension code can build a frozen collection without a mutable intermediate.

Two of those seven are circling the same question: how a module should declare which of its names are public. An eighth draft tried and gave up.

Being a draft is no promise of anything, and this batch proved it within a month. PEP 842 proposed a way for modules to declare their public API surface, and it was the one readers passed around most. Its author, Peter Bierma, withdrew it on August 15 after concluding that the design didn’t fit the needs of third-party packages. Withdrawing your own proposal three weeks after writing it is an underrated move.

Another PEP didn’t make it either. PEP 797 proposed `SharedObjectProxy` so arbitrary objects could cross subinterpreter boundaries, and the Council rejected it on July 8. An earlier immortal-proxy design had already been abandoned before the PEP was even submitted, defeated by implementation complexity and edge cases.

Sharing arbitrary objects between subinterpreters remains hard, and the Council decided that saying so beats pretending otherwise.

In quieter platform news, the Council approved amending PEP 11 on July 23 to promote Windows on ARM64 from Tier 3 to Tier 2 support. That means a required buildbot, two core developers on the hook, and test failures that block releases. If you’ve been running Python on an ARM Windows laptop and treating it as slightly experimental, then you can stop.

On August 20, the Council also approved Tier 3 support for 64-bit RISC-V Linux. That platform already runs three stable buildbots and a free-threading one, and the RISE project is backing the work. Tier 3 carries no release-blocking guarantee, so read it as a statement of direction rather than a promise.

Last month, the story was that the ecosystem had settled on 3.14t as its free-threaded target. This month brought the follow-up question. The GIL used to funnel Python’s number-crunching through a single core, no matter how many threads you started. Does removing it make numerical code faster?

Quansight published a detailed account of scaling NumPy on free-threaded Python, walking through the work needed in both NumPy and CPython to get multithreaded workloads to scale rather than merely run. It’s the most concrete engineering write-up on the topic so far, and it became the subject of episode 307 of the podcast.

The counterweight landed the same month. Timofei Ivankov documented that `asyncio.all_tasks()` can silently drop tasks on the free-threaded build, which is exactly the failure mode that makes free threading nerve-racking because nothing crashes and nothing raises.

**Note:** New to all this? Python 3.13: Free Threading and a JIT Compiler explains what changes when the GIL goes away, and the Thread Safety in Python: Locks and Other Techniques video course covers what you have to do about it.

Both stories point the same way. Free threading has moved past the question of whether it works and into the much longer stretch of finding the places where it doesn’t, one silent bug at a time.

Voting season arrived, and so did an argument about whether any of us understand the code we work on. The two are more closely related than they look. Packaging governance and codebase comprehension are both problems of scale, where nobody holds the whole thing in their head, and the systems we build have to assume as much.

We noted in August that nominations had opened for the first-ever Python Packaging Council. The nominations are in, and the community didn’t respond with a shrug: seventeen nominees are standing for five seats. The top two vote-getters take two-year terms, the next three take one-year terms, and future cycles alternate so the whole council never turns over at once.

The PSF Board election runs on the same calendar, with seventeen candidates standing for four seats as Cheuk Ting Ho, Christopher Neugebauer, Denny Perez, and Georgi Ker reach the end of their terms. The PSF asks voters to read the nomination statements rather than vote for the names they recognize, which is a polite way of acknowledging how these things usually go.

Both ballots run from September 1 to 15, so they’re open right now, though PSF voting members had to affirm their membership by August 25 to take part. That’s thirty-four candidates for nine seats across two bodies, one of which is brand new. Packaging, in particular, is the part of Python that everybody complains about and comparatively few people vote on.

The PSF also announced its Q2 2026 Fellow members, recognizing contributors who tend to do the sort of work that never shows up in a release note.

The Python Type System and Tooling Survey 2026 ran for most of August, collecting opinions about type hints from anyone who had one.

Sean Goedecke argued that you don’t need to understand your codebase, and his post was still the most-discussed piece going into August. His claim is that engineering advice online skews heavily toward small projects, where holding the whole system in your head is realistic, and that this advice misleads people working on large ones, where it isn’t.

It’s a good argument to have in a year when agents are generating code faster than anyone reads it. Christopher Bailey and Christopher Trudeau took it apart on episode 305, and it’s worth an argument with yourself.

The most-clicked link in PyCoder’s Weekly last month was James Bennett’s explanation of how many characters count as a line break in Python. It beat every release and every PEP.

The answer is eleven: ten code points, plus the two-character `\r\n` sequence. You can watch three of the surprising ones in action:

```
>>> text = "one\vtwo\fthree\u2028four"
>>> text.splitlines()
['one', 'two', 'three', 'four']
>>> text.split("\n")
['one\x0btwo\x0cthree\u2028four']
```
Vertical tab, form feed, and Unicode’s LINE SEPARATOR all end a line as far as `.splitlines()` is concerned, and none of them look like anything at all to `.split("\n")`.

Bennett traces where each one came from. There are the DOS, Unix, and classic Mac conventions that Python papered over with universal newlines back in 2.3, plus the ASCII control characters that predate all of them. Three of those control codes qualify only because Unicode’s bidirectional algorithm treats them as paragraph separators.

It’s a piece of Python trivia that turns into a history of text encoding if you pull on it. In a month this full of migration notes, it was a pleasure to read something with nothing to upgrade.

Three projects spent last month rearranging the ground the rest of us build on. Django rewrote its release calendar, PyPI closed off two long-standing surfaces, and Modular opened up one it had been holding back. Adam Johnson also shipped a pair of Django packages, and one of them has the best origin story in this issue.

Django is changing shape. Django’s Steering Council accepted DEP 20, and from 2028 the framework moves to one feature release each January, with version numbers based on the year. The first release under the new scheme is Django 2028.0, which follows Django 6.2 and replaces what would have been Django 7.0. Its first patch release is 2028.1.

The numbering is the least of it. The real change is that *the LTS designation goes away*. Every release gets three years of support: one year of bug fixes, followed by two years of security and data-loss fixes, with three versions supported at any time. Instead of choosing between the fast track and the long-term one, you get a rolling target.

Each Django release will support the three newest Python versions when it ships and pick up the next one during its first year. That lines Django up with Python’s own October cadence.

Nothing changes yet. Django 6.1 shipped on August 5, following the alpha we mentioned back in June. Django 6.2 LTS still arrives in April 2027 on the old schedule, and existing support commitments stand. But if your upgrade planning assumes there will always be an LTS to sit on, then that assumption now has an expiration date.

Adam Johnson also had a productive summer, releasing django-crawl and django-orjson in July.

Of the two, `django-crawl` has the better origin story. While moving a client project from the `django-csp` package to Django 6.0’s built-in CSP support, Johnson used the test harness to crawl the site and confirm the header value hadn’t changed. The crawl turned up seven unrelated bugs in a project that already had 100 percent test coverage.

Coverage tells you which lines ran, not whether the pages work. If you’re running a Django project, then that crawl is a cheap check to steal.

PyPI spent the month closing things off, with one exception that’s purely cosmetic. First, the exception, which is a phased rollout of a new user interface, staged so feedback can shape it. Worth knowing about, mainly so a redesigned pypi.org doesn’t catch you off guard in a few weeks.

More consequential, and much less visible, is that PyPI now rejects new files uploaded to releases older than fourteen days. The reasoning is supply-chain hygiene: a long-stable release shouldn’t suddenly grow a new artifact, because that’s a tidy way to poison something thousands of projects already trust. If your release process involves adding a wheel for a straggling platform weeks after the fact, then that workflow needs to change.

On August 11, PyPI froze the HTML version of the Simple Index API. PyPI has adopted PEP 833, which means the HTML index keeps serving current data but will never see another metadata field or structural change. Nothing is deprecated, and nothing breaks.

If you install packages with `pip` 22.2 or later, or with `uv`, then your installer already prefers the JSON representation from PEP 691, and you’ll never notice. If you maintain a tool that parses the HTML index, then consider this your heads-up that everything new happens in JSON from here.

Something did open up rather than close down. On August 18, Modular open-sourced the Mojo compiler and its tooling under Apache 2.0 with LLVM exceptions. The standard library had been open since 2024, so this is the rest of it. The move follows Mojo reaching 1.0 and source stability the week before.

Read the caveat before you get too excited. Modular isn’t accepting outside contributions to the compiler yet and says it aims to accept them by the end of the year, so the code is readable now and writable later. If you’ve followed Mojo as a thought experiment about how far a compiled Python-shaped language can go, then you can finally see the answer instead of guessing at it.

No new frontier model stole the month, which left room to notice the maintenance burden that a year of model launches has built up. After July’s flood of releases, August was the cleanup shift: breaking client libraries, a set of agent tools reaching GA, a security advisory in a tool many developers run locally, and not a single benchmark chart worth arguing about.

This month, the bill came due on the Python packages that talk to the models.

The openai-python 3.0.0 release landed on August 12, made `httpx2` the default HTTP client, and stopped pulling in `httpx` for you. If you pass a custom client, transport, or timeout config, then that’s a migration, not a version bump. There’s a temporary escape hatch for the legacy behavior, which is the sort of thing you should use to buy a week rather than a year.

Two days earlier, vLLM 0.27.0 added Kimi K3 support and moved to PyTorch 2.13, which the release notes themselves label a breaking environment change. Transformers 5.15 landed on August 10 too, with new open-weight architectures, following the GPTNeoX weight rename and the GPTBigCode attention-backend change that shipped in 5.14 back in July.

Then on August 20, the Anthropic SDK went to 1.0.0 and repeated exactly the jump OpenAI had made eight days earlier: rebuilt on `httpx2`, with legacy `httpx` support removed. A custom `http_client` now has to be an `httpx2.AsyncClient`, and the 1.x SDK rejects the old kind at construction.

What happened next is the useful part. Every release of Pydantic AI up to that moment allowed `anthropic` without an upper version bound, and so did 2.32.2, which shipped about seven hours after the SDK went 1.0 and still carried no bound. So a fresh or unpinned install of `pydantic-ai[anthropic]` would cheerfully resolve to a major version the framework had never been tested against, and then fail at runtime.

Roughly nine hours after the SDK landed, Pydantic AI 2.33.0 arrived with release notes that open “If Anthropic stopped working for you in the last day” and an apology for the breakage window. The fix requires `anthropic` 1.0.0 or later, and anyone who needs to stay on an older Pydantic AI has to pin `anthropic<1`.

The Pydantic AI framework wasn’t the only thing caught out. Simon Willison’s `llm` tool picked up `httpx` transitively through `openai`, so the 3.0.0 release broke fresh installs there too. He shipped 0.32.1 on August 21 as an emergency pin to `openai<3`, then llm 0.33 the next day with the real fix, moving the tool onto `httpx2` properly. Pinning first and migrating second is the right order, and it’s useful to watch someone do it in public.

Four unrelated projects, ten days, all breaking, and two of them knocking over downstream projects, one patched in under a day. The argument for lock files makes itself here, without anyone having to write a think piece about supply-chain discipline. An upper bound on a dependency you don’t control is a seatbelt.

While the client libraries were churning, the Claude API moved a batch of agent features out of beta on August 19. Computer use is now generally available as the `computer_toolset_20260801` toolset, dropping the beta header, adding batched actions, and turning zoom on by default. Upgrading from the older beta changes the shape of your requests, so it takes real rework, not a flag flip.

Alongside it came a browser-use tool that drives a browser your own application hosts. It reads the page through the accessibility tree and interacts with elements, forms, and tabs, building on the older screenshot-and-click control. The Files API and Agent Skills both went GA in the same batch, and the day before, the Workbench was renamed the Playground, which now shows the full SDK request and response for every run.

None of that is thrilling on its own. What’s worth noticing is the direction. A year ago, these were demos, and now they’re part of a stable surface with version-pinned tool names and migration guides attached. Agent tooling is growing the same unglamorous machinery as the rest of the ecosystem, which is usually the sign that something has stopped being a toy.

The security news to know about is an advisory in Pydantic AI. The local development UI exposed by `Agent.to_web()` and `clai web` didn’t validate the `Host` header, which opened it to DNS rebinding. A malicious website you happened to visit could reach the agent running on your own machine, with your tools and your credentials attached.

Pydantic AI 2.30.0 restricts the UI to localhost and LAN by default and adds an `allowed_hosts` option for anyone deliberately serving it on a real hostname.

**Note:** This is a good moment to audit what else is listening on your machine. If you’re wiring up agents of your own, then How to Write a CLAUDE.md File for Claude Code and CrewAI in Python: Coordinating Teams of AI Agents both cover setups worth auditing the same way.

It’s a small bug with a large lesson. We’ve all adopted local agent tooling over the last eighteen months, and much of it assumes that “it’s only on my laptop” is the same thing as “it’s private.” A browser tab is enough to prove otherwise.

Late August was crowded. PyCon JP opened the run in Hiroshima on August 21 and 22. DjangoCon US followed from August 24 to 28 at voco Chicago Downtown. Talks filled the first half, and contribution sprints closed it out. PyCon AU overlapped the end of DjangoCon US, running in Brisbane from August 26 to 30.

If you missed all three, which most people did, then the talks are the consolation prize. Recordings from these conferences usually appear within a few weeks of the closing keynote. The sprint days don’t record well, and they’re what’s most likely to turn a first-time contributor into a regular one, so they’re worth budgeting for next year.

The Real Python team spent the month on observability, AI-assisted debugging, and a preview of what 3.15 does to profiling. Here’s what’s new on the site.

You can start with these new **tutorials**:

If you prefer learning by watching, then check out these new **video courses**:

Test your understanding with these new **quizzes**:

On **The Real Python Podcast**, host Christopher Bailey covered prompts, performance, complexity, and the ways AI fails without telling you:

Episode 307 is the one to pick if you’re short on time. It’s the conversational version of the Quansight write-up above, and hearing someone explain where the scaling actually comes from gets you up to speed faster than the benchmarks alone.

Episode 308 pairs well with episode 307 for a different reason: Calvin Hendryx-Parker on why AI systems fail silently, which is the same failure mode as that `asyncio.all_tasks()` bug, one layer up the stack. Episode 309 then draws the line between complicated and complex problems, which is the same distinction Sean Goedecke was reaching for when he argued you can’t hold a large codebase in your head.

September is a short runway. Release candidate 2 arrived on September 1, both election ballots close on September 15, and 3.15 is scheduled to go final on October 1. If you’ve been treating the 3.15 cycle as somebody else’s problem, then this is the last month where finding a bug is cheap.

On our side, Python 3.15 Preview: UTF-8 by Default is already out, with Agentic Engineering in Python: From Vibes to Evidence coming mid-month, a title that suggests where the conversation about AI tooling is finally heading.

On the same theme, Philipp Acsany is running AI Coding Tools for Python Developers: What’s Actually Worth Using Right Now on **Saturday, September 12**, a one-day live course on Zoom. It walks you through the whole landscape, category by category, and ends each one with a verdict: worth it, worth knowing about, not yet, or skip.

If there’s a thread running through all of this, it’s that last month was about the layer underneath your code rather than the code itself. An ABI locked down, an index format closed off, a release calendar redrawn, and a dependency graph left one missing upper bound away from a bad morning.

None of it is glamorous, and all of it is the sort of thing you only notice when it goes wrong. The one bright spot lands in 3.16. See you next month!
