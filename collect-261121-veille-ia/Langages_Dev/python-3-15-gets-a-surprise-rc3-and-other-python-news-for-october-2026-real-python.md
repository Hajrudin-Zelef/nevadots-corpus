---
id: collect-261121-veille-ia/langages-dev/python-3-15-gets-a-surprise-rc3-and-other-python-news-for-october-2026-real-python
title: "Python 3.15 Gets a Surprise RC3 and Other Python News for October 2026 – Real Python"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["python", "release", "lazy", "october", "news", "feature", "been", "surprise"]
source: https://realpython.com/python-news-october-2026/
source_anchor: ""
source_lines: [1, 302]
section: "Langages_Dev"
---
**Python 3.15** was supposed to be the least surprising news of the month. The feature set has been locked since May, and our preview series has been walking through it all summer. Then a handful of last-minute bugs in lazy imports forced a surprise third release candidate, and the final release moved to October 9, later this week.

The first-ever **Python Packaging Council** was elected and immediately handed a PEP written by one of its own members. PyPI changed how it counts downloads, so if your package’s chart just fell off a cliff, that’s the likely reason.

**Polars 2.0** reached the release candidate stage with a new default that can reorder your rows, and **SQLAlchemy 2.1** shipped in late September. Meanwhile, the AI providers broke Python code again, this time from the API side, and EVE Online announced that its 2.4 million lines of Python 2 are finally moving to Python 3.

Grab a coffee, because there’s a lot of Python news to get through!

**Join Now:** Click here to join the Real Python Newsletter and you’ll never miss another Python tutorial, course, or news update.

Python 3.15.0 was due on October 1, the date that PEP 790 set for it more than a year ago. Instead, release manager Hugo van Kemenade announced a third release candidate, explaining that “we got some last-minute lazy-import release blockers, and it makes sense to include them in 3.15.0 final.”

Python 3.15.0rc3 came out on October 2 with around 156 fixes from 82 contributors since rc2. The final release is now scheduled for **Friday, October 9**.

The blockers were the kind of edge cases you only find once people start using a feature for real. In one bug, `lazy import a.b as c` treated `b` as an attribute of `a` instead of importing the `a.b` submodule. In another, touching one lazily imported submodule of a package also imported a sibling submodule that should have stayed lazy. Shipping a headline feature with known bugs like these would’ve been worse than a one-week delay.

Nothing else changes. The feature set has been frozen since May. The application binary interface (ABI), which compiled extensions rely on to work with the interpreter, has been frozen since August and stays frozen through rc3. Between now and Friday, only reviewed bug fixes can land on the 3.15 branch. If you followed the release candidates over the summer, then nothing in the final build will surprise you.

You can try the release candidate in a REPL today without installing anything by hand, as long as your copy of `uv` is recent enough to know about rc3. Once the final release is out, an updated `uv` will pick up 3.15.0 with the same command:

```
$ uv self update
$ uv run --python 3.15 python
```
If you installed `uv` through a package manager like Homebrew or `pipx`, then `uv self update` won’t work, so upgrade `uv` with that package manager instead.

Once you’re in, most of the headline features fit on a single screen. Here’s a lazy import, a frozen dictionary, and a sentinel working together:

```
>>> lazy import json
>>> import sys
>>> "json" in sys.modules
False
>>> defaults = frozendict({"theme": "light", "autosave": True})
>>> MISSING = sentinel("MISSING")
>>> defaults.get("font", MISSING)
MISSING
>>> json.dumps(defaults | {"theme": "dark"})
'{"theme": "dark", "autosave": true}'
>>> "json" in sys.modules
True
```
The `lazy` soft keyword from PEP 810 defers the import until you first access `json`, which is why the module shows up in `sys.modules` only after the `.dumps()` call. The `frozendict` and `sentinel` built-ins are the two new types: one for mappings that can’t change and one for marking missing values when `None` is a valid value.

Lazy imports are also the feature that rc3 fixed, so if you tried them on rc2 and saw a submodule load too early or not at all, then try again on rc3.

Beyond those three, the release has these additions:

- **UTF-8 by default** now applies everywhere, so`open()` stops guessing your encoding from the system locale.
- **A sampling profiler** in the standard library can attach to a running process.
- **An upgraded just-in-time (JIT) compiler** adds a tracing frontend. It’s still experimental and runs only in builds that support it and have it enabled. In the core team’s benchmarks, it’s 7 to 8 percent faster than the standard interpreter on x86-64 Linux and 11 to 12 percent faster than the tail-calling interpreter on AArch64 macOS.
- **A tail-calling interpreter in the official 64-bit Windows binaries** is built with Visual Studio 2026. In pyperformance runs on x86-64, it measured 15 to 20 percent faster than the switch-case interpreter.
- **`abi3t`** is a stable ABI for the free-threaded build. A C extension that opts in can ship one wheel for free-threaded Python 3.15 and later, though common build tools can’t produce these wheels yet.
- **`TypeForm` and closed `TypedDict` types** arrive for anyone who argues with their type checker for fun.
- **Unpacking in comprehensions** now works, as in`{**profile for profile in profiles}` .

One more change is a retreat rather than a feature. The incremental garbage collector that debuted in 3.14.0 was rolled back in 3.14.5 after reports of memory pressure, and 3.15 keeps the older generational collector. It’s the second time that design has been pulled, and it’s a useful reminder that a good benchmark isn’t the same as a good production week.

**Note:** We’ve been previewing this release all year. For the full tour, with runnable examples and a list of porting gotchas, read Python 3.15: Cool New Features for You to Try or watch the What’s New in Python 3.15 video course. Then go deeper with the individual previews on lazy imports, `frozendict`, sentinel values, the sampling profiler, UTF-8 by default, and the upgraded JIT.

Should you upgrade this week? Test on rc3 now, and switch your own scripts and command-line tools once 3.15.0 lands on Friday. Lazy imports alone can make a sluggish `--help` feel snappy again. If you maintain a package with compiled extensions, then you don’t have to wait. Wheels built against the release candidates are compatible with the final release, so you can publish 3.15 wheels today.

For anything you deploy, the usual advice still applies: wait until your compiled dependencies publish 3.15 wheels, and let production servers sit tight until 3.15.1. The one change most likely to bite is the switch to UTF-8 by default, which matters only if your code implicitly relied on a legacy locale. Your test suite will tell you in an afternoon, and you don’t need to wait for Friday to run it:

```
$ uv run --isolated --python 3.15 --with pytest==9.1.1 \
  python -X warn_default_encoding -m \
  pytest -W error::EncodingWarning
```
In a `uv` project, that command builds a throwaway 3.15 environment from your `pyproject.toml` and adds `pytest` in case it isn’t already a dependency.

If you keep your dependencies in `requirements.txt`, then add `--with-requirements requirements.txt`. The `-X warn_default_encoding` flag enables `EncodingWarning`, which is off by default, and `-W error::EncodingWarning` turns every `open()` call without an explicit encoding into a test failure. Turning deprecation warnings into errors won’t catch these calls, because `EncodingWarning` isn’t a `DeprecationWarning`.

Fix each failure by passing the encoding you intend to use, and add a test with a real legacy-encoded file wherever your code reads one. PEP 686 walks through the full migration.

Real Python’s new **Working With AI Agents** live courses help you get more out of the AI agent you already use.

Stop guessing why your agent did something, and start checking, directing, and trusting its work. The five focused courses teach the skills that carry over from tool to tool. **Explore the Courses →**

The only PEP decision this month was a provisional one. On September 9, PEP 825, which defines the package format for wheel variants, was provisionally accepted. This lull is typical of a release month: the core team was busy shipping. Two new drafts are worth bookmarking because both address small annoyances you’ve probably run into.

PEP 845, from Thomas Kehrenberg and Marc Mueller, proposes **leading-dot value patterns** for structural pattern matching. Today, if you want a `case` clause to compare against a local variable, then you have to capture the value under a new name and check it in a guard. Under the proposal, a leading dot would tell Python to match the variable’s value instead of binding a new name:

```
book = {"title": "Dune", "author": "Frank Herbert"}
author = "Frank Herbert"
# Today: capture, then compare in a guard
match book:
    case {"title": title, "author": book_author} if book_author == author:
        print(f"Found {title}")
```
Under PEP 845, you could make the same check without the guard. This is proposed syntax, so it won’t run on any released Python version:

```
# PEP 845: match against the existing value of author
match book:
    case {"title": title, "author": .author}:
        print(f"Found {title}")
```
Dotted names like `Color.RED` already work as value patterns, so the proposal extends a rule you know rather than inventing a new one. Whether a single dot is too easy to miss when you’re skimming code is exactly the kind of question participants will debate in the discussion thread.

PEP 846, from Bartosz Sławecki, would let you document a `type` alias with a string literal right below it, the way you document a function. The string would land in the alias’s `__doc__` attribute, so `help()` could show it at runtime. Right now, only some type checkers and Sphinx pick those strings up.

Both PEPs target Python 3.16 and remain in draft form, so treat them as proposals for now.

In March, OpenAI announced its agreement to acquire Astral, as we covered in May, and the community openly asked what would happen if a company with other priorities ended up owning the most popular Python tooling. September brought the first real answers, and Python’s own institutions supplied most of them.

Voting in the elections we flagged last month closed on September 15, and on September 17 the PSF announced the first-ever Python Packaging Council. Brett Cannon and Pradyun Gedam won two-year terms. Donald Stufft, Henry Schreiner, and Ralf Gommers won one-year terms. Voters cast 541 ballots. The PSF Board election finished the same day, seating Elaine Wong, Laís Carvalho, Ee Durbin, and Georgi Ker. It drew 670 ballots.

The new council already has two items on its desk. Both are draft PEPs that deal with virtual environments:

- PEP 832 standardizes how tools discover a project’s environment.
- PEP 838 standardizes a `python-version` field in`pyvenv.cfg` . Tools already record the version there using different keys and formats. The new field holds only the major and minor version numbers, so it doesn’t go stale after a patch upgrade.

Both sit between the core interpreter and the packaging tools, so who gets to decide? Brett Cannon put that question to the Steering Council in August. On September 10, the Steering Council answered that the two PEPs fall within the responsibility it shares with the Packaging Council under PEP 772. The two councils would reach a joint decision once the Packaging Council was seated, which happened a week later.

PEP 832 itself changed in the meantime. On September 8, Cannon reverted it to its earlier design with a `.venv` redirect file. That leaves a nice bit of irony: Cannon, the PEP’s author, now sits on one of the two councils that will rule on it. Overlapping roles like this are normal in a small community, and the recusal conversation will be worth watching.

**Note:** If `.venv` redirect files sound like an abstraction too far, then Python Virtual Environments: A Primer covers what a virtual environment actually is, and Managing Python Projects With uv shows how modern tools create and find them for you.

The other institutional thread runs through the `python/prebuilt-cpython` repository, which has hosted planning for official prebuilt, relocatable CPython builds since October 2025. Today, the builds that `uv` downloads when you ask for a Python version come from Astral’s `python-build-standalone` project.

An official alternative under the `python` organization would make the ecosystem less dependent on any single company, a comforting prospect after the acquisition news.

Astral, for its part, kept shipping. A `uv` pull request merged on August 31 deduplicates identical files across the wheel cache by hard-linking them, which reduced the PR author’s cache size by about 10 percent. The feature is hardly glamorous, but your disk will appreciate it.

PyPI had three stories this month, and all of them are about knowing what the index is actually doing.

First, download counting got stricter. Since August 24, a request counts as a download only if it fetches a `.whl`, `.tar.gz`, or `.zip` file. Metadata-only requests from installers, which made up about 40 percent of the old traffic, no longer count. If you maintain a package and your downloads dropped sharply around August 24, then the new counting rule is the probable cause.

Older numbers weren’t filtered after the fact, so filter both periods the same way before you compare them. PyPI itself warns that download counts don’t measure users or popularity.

Second, PyPI published a postmortem for intermittent file-hosting errors that ran from August 15 to 28. The mix of 502 and 503 errors on `files.pythonhosted.org` came from a partially rolled-back canary deployment at Fastly combined with several configuration bugs on PyPI’s side. If your CI pipeline failed randomly in the second half of August, then you now have your explanation.

Third, Brett Cannon wrote up what’s missing for reproducible builds on PyPI. Wheels can already record their build tools in an SBOM under PEP 770.

What’s still missing is a standard way for a source distribution to say where it came from, a way for it to carry an SBOM, and a way to use PyPI to publish independent verification that a wheel matches its source. He sketches fixes for all three, including a new source distribution format and trusted verifiers that would vouch for reproducibility through the index API.

Put together, these stories show where packaging governance is heading. The new council inherits an index that’s getting better at measuring itself, which will help the council make decisions based on those numbers.

The community stories this month range from a two-decade-old spaceship game to a single innocent-looking string method. The spaceship game comes first.

Fenris Creations, formerly CCP Games, announced that EVE Online has begun migrating to Python 3. In case you’ve never heard of it, EVE is a spaceship MMO that launched in 2003 and has been running on Stackless Python ever since. It moved to Stackless 2.7 in 2010 and stayed there, which means the game has outlived Python 2 itself by several years.

The codebase is 2.4 million lines of Python. So far, only the first stage has shipped: code that’s Python 3–ready but still runs on 2.7. Fenris gives no completion date and calls this the beginning of a long road, which is probably the most accurate migration estimate anyone has ever published. If you put off your own Python 3 migration until 2019, then you can feel a little better now.

EVE’s migration is also a reminder of how far Python reaches. A game with hundreds of thousands of players has been running its logic in Python for over two decades, and its developers are still choosing to invest in the language rather than leave it.

The State of Django 2026 survey from JetBrains and the Django Software Foundation collected nearly 3,500 responses from more than 40 countries. Its subtitle, *Boring Is So Back*, sums up the results. PostgreSQL has held steady at 76 to 79 percent of respondents for five years, and 43 percent are already on Django 6.0.

That second number provides context for Django’s move to annual releases last month. Starting in January 2028, every feature release will get three years of support, so the long-term support (LTS) label goes away. Django 6.2 LTS is still scheduled for April 2027. Given that 43 percent of survey respondents had already moved to Django 6.0 before 6.1 came out, three years of support for every release matches how people actually upgrade.

The survey also shows that 58 percent of respondents who use AI coding tools rely on them daily, that `uv` has reached 43 percent adoption, and that Ruff is the most popular code-quality tool. Christopher Bailey and Christopher Trudeau dug into the results on episode 311 of the podcast. If you’re starting a Django project today, then the survey gives a good picture of what everyone else is using.

One of the most-read links in PyCoder’s Weekly this month was Daniel Lemire’s demonstration that Python sets and dictionaries can have quadratic-time performance. The trick relies on how CPython hashes integers, using the Mersenne prime 2<sup>61</sup> − 1 as the modulus on 64-bit builds:

```
>>> M = 2**61 - 1
>>> hash(M), hash(2 * M), hash(3 * M)
(0, 0, 0)
```
Every multiple of that prime hashes to zero, so they all start at the same slot in the hash table. CPython resolves collisions by probing other slots, and keys with equal hashes follow the same probe sequence, so each new key has to step past all the ones inserted before it.

In Lemire’s benchmark, inserting a few thousand such multiples into a set turns a constant-time operation into a linear one, and filling the whole set becomes quadratic. The elapsed time roughly quadruples whenever the input doubles, reaching around 45 seconds for 100,000 keys.

You won’t hit this with ordinary data, but you can when an attacker picks your dictionary keys, which is why string hashing has been randomized since Python 3.3. Integers aren’t randomized.

In a nice coincidence, the Python 3.16 docs picked up a new time-complexity reference page covering the Big O costs of `list`, `deque`, `set`, and `dict` operations. It gives you an official answer to how fast each operation is, including the fine print about average and worst cases that Lemire just demonstrated. If you’d rather see collisions happen up close, then build a hash table yourself or brush up on how dictionaries work.

Seth Larson, the PSF’s Security Developer-in-Residence, explained how `str.lower()` became a vulnerability behind CVE-2026-17084. The old Internationalizing Domain Names in Applications (IDNA) 2003 standard pins case folding to Unicode 3.2.0, but `str.lower()` uses whatever Unicode version your interpreter ships with.

For some characters, like the Cherokee letter Ꭰ, the two disagree, so the same domain name can encode to two different Punycode strings.

When two parts of a system disagree about which domain they’re talking about, attackers get a way in. The vulnerability lives in the `stringprep` module and the standard library’s IDNA 2003 codec, not in everyday `str.lower()` calls. The CPython fix restores the Unicode 3.2.0 mappings, so upgrade to a Python release that includes it. If you need the newer IDNA 2008 rules, then use the third-party `idna` package instead of the standard-library codec.

Two data libraries set the tone this month. One is moving to a new major version, and the other just arrived at a long-awaited minor one. Both changed defaults in ways that can break working code, so read the notes before you bump your pins.

Polars announced 2.0 and published its first release candidate on September 2, and 2.0.0rc2 followed on September 20. The final release is expected in the coming weeks, so this is the time to run your test suite against the release candidate and leave production alone.

The main change is that the streaming engine becomes the default for every `LazyFrame` query. Polars claims roughly five times the speed, but the new default comes with a catch that will bite somebody. The streaming engine doesn’t keep row order the way the in-memory engine happened to, so code that implicitly relied on the old order can break.

Group-bys are the easiest place to trip up. Polars 1.x never promised the order of `.group_by()` output unless you passed `maintain_order=True`, and the new engine makes that caveat much harder to ignore. To try it yourself, install the release candidate into a throwaway environment:

```
$ uv run --with polars==2.0.0rc2 --prerelease=allow python
```
Then ask for the order explicitly:

```
import polars as pl
orders = pl.DataFrame({"customer_id": [2, 1, 2], "amount": [10, 20, 30]})
result = (
    orders.lazy()
    .group_by("customer_id", maintain_order=True)
    .agg(pl.col("amount").sum())
    .collect()
)
```
If any of your code or tests depend on the order of a `.group_by()` result, then add `maintain_order=True` or an explicit sort now. Do the same for joins and unpivots, which 2.0 also no longer keeps in order unless you ask. The release also removes the long-deprecated `melt()` in favor of `unpivot()`, rejects lossy type coercions in `is_in()`, and raises new `AttributeRemovedError` and `ArgumentRemovedError` exceptions to point you at removed APIs.

Around the same time, the most-clicked link of the month in PyCoder’s Weekly was Polars’ own guide to pandas-to-Polars migration strategies. It lays out three options: migrate only the slow part, migrate everything by hand, or let an LLM translate your code and repair it against test fixtures until the checks pass. The fact that the third option is presented with a straight face is itself a sign of the times.

For a more hands-on take, Marco Gorelli’s Polars vs SQL differences nobody is talking about explains where the SQL mental model breaks. SQL tables are unordered bags of rows, while Polars DataFrames keep their order.

SQL databases don’t even agree with each other on where nulls go. PostgreSQL puts them last in ascending order and first in descending order, while SQLite puts them first in ascending order. Polars puts them first unless you pass `nulls_last=True`. None of them is wrong, but if you move queries between SQL and Polars, then you’ll want to know which convention each one follows.

SQLAlchemy 2.1.0 shipped on September 24, more than three and a half years after 2.0, following release candidates on August 31 and September 8. Mike Bayer describes it as “decidedly not as dramatic” as 2.0, but a few hard changes can still break your setup:

- The `greenlet` dependency no longer installs by default, so if you use SQLAlchemy’s`asyncio` support, then install`sqlalchemy[asyncio]` .
- A bare `postgresql://` URL now picks the`psycopg` 3 driver, and`oracle://` picks`oracledb` .
- Python 3.11 is the new minimum version.

On the feature side, a new `tstring()` construct brings template strings to raw SQL on Python 3.14 and later, binding interpolated values as parameters instead of concatenating them into the SQL string. There’s also long-awaited `CREATE VIEW` support and a new driver for Microsoft SQL Server.

If your `requirements.txt` says `sqlalchemy` with no upper bound, then your next fresh install or upgrade on Python 3.11 or later will pick up 2.1 unless another dependency holds it back. If you use the asyncio support, then change that line to `sqlalchemy[asyncio]` so that `greenlet` still gets installed.

A few more releases caught our eye:

- **Wagtail 8.0** adds a v3 REST API with read and write support for CMS operations and fixes five security issues in its admin and content APIs. Two members of the Wagtail team are on episode 312 of the podcast.
- **Flet 1.0** reached a stable release for building cross-platform desktop, web, and mobile apps in pure Python.
- **Poetry 2.5.0** and**Flit 4.1.0** landed within days of each other, which fits a month when packaging was the main topic.
- **Click 8.5.0** ,**IPython 9.17.0** , and a second beta of**Pydantic 2.14** rounded out the list.

Last month, we covered a cascade of breaking SDK releases that knocked over downstream projects. This month brought a second wave, and this time the breakage came from the APIs themselves. On top of that, PyTorch changed some gradients without raising a single error, and a security researcher showed what an agent in auto mode will run if you let it.

Here are the changes most likely to turn up as errors in your logs:

1. **The OpenAI Assistants API is gone.** It shut down on August 26, after we flagged the planned sunset back in March. Any code still calling`client.beta.assistants` needs to move to the Responses and Conversations APIs.
2. **GPT-6 Astra requires the Responses API for tools.** The model became generally available on September 3, and tool calling through Chat Completions isn’t supported. It also rejects custom`temperature` and`top_p` values and doesn’t return log probabilities.
3. **Claude Fable 5.1 and Mythos 5.1 reject some `tool_choice` values.** Both models shipped on September 1 and return a 400 error for`tool_choice` set to`any` or`tool` . The release notes suggest`auto` combined with strict tool use instead. Strict tool use validates the tool calls the model makes, but`auto` still lets it skip tools entirely, so if your code depends on a tool call, then check whether the response contains one and handle the plain-text case. Separately, for accounts created on or after August 31, 2026, Claude Fable 5.1 now rejects a replayed thinking block by default if you’ve changed the system prompt, tools, or an earlier message. A beta`prefix_mismatch_behavior` setting lets you drop the mismatched blocks instead.

None of these changes are unreasonable individually. Taken together, they mean that code you wrote against last year’s APIs may stop working with this year’s models. Keeping model names in configuration and running a small smoke test against every model you plan to switch to is cheaper than debugging in production.

Not everyone is excited about where the new models are heading. Armin Ronacher’s Astra for Coding: Why Are We Doing This Again? argues that GPT-6 Astra is great at finishing tasks and poor at writing code that humans can read, with hard-coded constants and odd workarounds everywhere. He asks whether the models are now being optimized for the wrong goal.

It’s the sharpest counterpoint to the benchmark charts this month, which makes our guide to reviewing AI-generated Python code especially timely.

**Note:** If you’re updating Claude integrations, then the Using the Claude API in Python video course covers the basics of messages, system instructions, and structured output. To see how the two new model families compare on the same creative task, check out our benchmarks of Claude Fable 5.1 and GPT-6 Astra.

PyTorch 2.14.0 arrived on September 2 with a numerical change that won’t show up as an error. When the input to `torch.clamp()` or `torch.clamp_min()` sits exactly on a scalar bound, the gradient is now zero instead of one. You can check it in a throwaway environment on Python 3.14:

```
$ uv run --python 3.14 --with torch==2.14.0 python
```
```
>>> import torch
>>> x = torch.tensor(0.0, requires_grad=True)
>>> torch.clamp_min(x, 0.0).backward()
>>> x.grad
tensor(0.)
```
In PyTorch 2.13, that last line printed `tensor(1.)`. When the bound is itself a tensor that requires gradients, a tie now splits the gradient evenly between the input and the bound, and `fmin()` and `fmax()` follow the same rule. The new behavior is mathematically cleaner, but if you run a training job that relies on exact reproducibility, then pin your version or compare a few runs before you upgrade.

The rest of the ML stack kept moving, too. vLLM 0.29.0 made Model Runner V2 the default and plans to remove V1 in 0.32. Transformers 5.16 replaced its legacy tensor-parallel API with a DTensor-based one, which is a breaking change if you shard models yourself. Then 5.17 added Tencent’s 780-billion-parameter Hy4 preview.

LangChain 1.4.0 added a native `langchain.mcp` namespace for the Model Context Protocol (MCP), the standard way to connect models and agents to external tools and data. The new namespace builds on August’s MCP rewrite. On OpenAI’s side, the new Agents API entered public beta on September 10, offering a hosted agent harness that handles sessions, context compaction, and recovery for you.

Johann Rehberger showed how to break Claude Code’s auto mode with a poisoned ZIP file. Rehberger succeeded in roughly 60 to 80 percent of the attempts, though the sample was small.

The chain is clever. The attacker’s server pushes Claude to download an archive with `curl`, and Claude, sensibly distrusting the bundled decoder, writes its own Python decoder instead. The trouble is that it runs that script from inside the extracted directory, where a malicious `struct.py` shadows the standard-library module. As Rehberger puts it, “Claude does not trust the supplied binary decoder, but it trusts the one it wrote itself.”

Anthropic closed the report as informative, saying that auto mode relies on a best-effort classifier and isn’t a security boundary. That’s a fair answer, and it’s also the lesson. If you let an agent run commands on its own, then isolation and network egress controls are your real protection. The classifier is a convenience. It’s the same moral as last month’s local agent UI bug, one layer further down.

Here’s where October’s Python conference calendar stands:

- **PyBay** took place on October 3 in San Francisco, just before this article went out.
- **PyCon Africa** runs from October 7 to 11 in Kampala, Uganda.
- **PyCon Estonia** won’t take place this year. The organizers canceled the 2026 edition and plan to return in 2027.
- **PyCon Greece** runs on October 12 and 13 in Athens.
- **PyCon NL** meets on October 15 in Utrecht. It’s the first edition run by the newly formed PyNetherlands foundation.

If you had **PyCon Ireland** on your calendar for October 17, then move it. The event has been rescheduled to November 21 at a new location in Dublin’s city center after the original venue fell through. Further out, **PyData Global** runs online from December 8 to 10.

The 3.15 preview series wrapped up this month, and the rest of the site leaned hard into AI-assisted development, with a detour into timing your code and profiling your data. Here’s what’s new.

The written **tutorials** range from the last few 3.15 previews to the full release guide, with several AI pieces in between:

If you prefer learning by watching, then check out these new **video courses**:

The **quiz** list is long this month because the team caught up on quizzes for a big batch of classic tutorials along with the new ones:

On **The Real Python Podcast**, Christopher Bailey went from performance engineering to Django survey data to AI in open source:

If you only have time for one episode, then make it 312. Meagen Voss, Wagtail’s community manager, and Thibaud Colas from its core team talk frankly about what happens to an open-source project when AI-generated pull requests start arriving faster than humans can review them. They also get into how Wagtail picks open-weight models and inference providers for its own AI features.

It pairs well with the tutorial on reviewing AI-generated code above, since it shows the same problem from the maintainer’s side of the pull request.

Python 3.16 development has been underway since May, and its first alpha is close. PEP 826 schedules that alpha for October 13, with the feature freeze on May 4, 2027, and the final release on October 5, 2027.

PEP 828’s `yield from` for async generators was already accepted for 3.16 in August, while drafts like PEP 845 and PEP 846 will compete for a spot. Python 3.10 also reaches end of life in October, so if you still run it anywhere, then this is your nudge.

A few other things are worth watching:

- The final release of Python 3.15.0, now scheduled for October 9
- The Packaging Council’s first meetings and whether PEP 832 and PEP 838 get a joint ruling with the Steering Council
- The final release of Polars 2.0, expected within weeks of the second release candidate
- Pydantic 2.14, which is on its second beta
- The first 3.15 wheels for the compiled libraries you depend on, which will determine when you can upgrade

The common theme this month came from Python’s institutions, which spent September catching up with the size of the ecosystem they look after.

Packaging got an elected council, PyPI started counting only real downloads, and even a two-decade-old spaceship game decided that Python 2 was no longer a sustainable home. Python 3.15, the one release that was supposed to arrive without drama, had one last surprise in store. Still, a one-week slip to get lazy imports right is a price worth paying, and it’s a good sign of a release team that would rather ship late than ship broken.
