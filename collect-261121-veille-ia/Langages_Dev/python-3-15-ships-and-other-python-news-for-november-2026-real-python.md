---
id: collect-261121-veille-ia/langages-dev/python-3-15-ships-and-other-python-news-for-november-2026-real-python
title: "Python 3.15 Ships and Other Python News for November 2026 – Real Python"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["python", "real", "news", "samples", "release", "lists", "tachyon", "sampling"]
source: https://realpython.com/python-news-november-2026/
source_anchor: ""
source_lines: [1, 391]
section: "Langages_Dev"
---
After an eight-day delay, **Python 3.15** finally shipped on October 9, and Python 3.16’s first alpha followed only four days later. The alpha arrived alongside two new PEPs that propose bringing `?.` and `??` to Python, more than ten years after the last attempt stalled.

You’ll also read about eight intermediate certificate authorities whose private keys turned out to be recoverable by anyone, which sent `pip`, `certifi`, `uv`, and CPython itself scrambling to ship fixes. Meanwhile, **pandas 3.1** and **Polars 2.0** both landed, Ruff stopped nagging you about most naive datetimes, and Anthropic’s new API version dropped `temperature`.

That’s plenty to cover, so dive into this month’s **Python news**!

**Join Now:** Click here to join the Real Python Newsletter and you’ll never miss another Python tutorial, course, or news update.

After last month’s surprise third release candidate, Python 3.15.0 landed on October 9, eight days late and without further drama. Release manager Hugo van Kemenade’s announcement counts **5,643 commits from 1,012 contributors** since Python 3.14.

You already know the headliners if you’ve followed Real Python’s coverage this year: lazy imports, `frozendict`, the new `sentinel` built-in, and UTF-8 as the default encoding. For a guided tour, Python 3.15: Cool New Features for You to Try and the What’s New in Python 3.15 video course walk through them with examples.

However, two of the smaller changes may well be the ones you end up using most often. The first is PEP 798, which lets you unpack inside a comprehension. Flattening a list of lists no longer needs a nested loop or `itertools.chain()`:

```
>>> lists = [[1, 2], [3], [4, 5]]
>>> [*x for x in lists]
[1, 2, 3, 4, 5]
>>> {*x for x in lists}
{1, 2, 3, 4, 5}
>>> profiles = [{"a": 1}, {"b": 2}, {"a": 3}]
>>> {**p for p in profiles}
{'a': 3, 'b': 2}
```
The star unpacks one level per item. Dictionary unpacking follows the same rules as `{**a, **b}`, so later keys win, which is why `"a"` ends up with the value `3`.

The second is **Tachyon**, the sampling profiler that ships in the new `profiling` package from PEP 799. It samples your program’s stack from the outside, so it adds very little overhead and can even attach to a process that’s already running. You don’t need to change any code to point it at a script. Take `slow.py`, which computes a Fibonacci number recursively:

```
def fib(n):
    return n if n < 2 else fib(n - 1) + fib(n - 2)
print(fib(32))
```
Running it under Tachyon prints the script’s output, followed by the profile. Your timings will differ:

```
$ python -m profiling.sampling run slow.py
2178309
Captured 298 samples in 0.30 seconds
Sample rate: 1000.00 samples/sec
Error rate: 36.58
Profile Stats:
       nsamples   sample%  tottime (ms)    cumul%  cumtime (ms)  filename:lineno(function)
        187/189      98.9       187.000     100.0       189.000  slow.py:2(fib)
```
Tachyon correctly blames nearly every sample on `fib()`, whose return statement is on line 2 of the script. The percentages only count the samples Tachyon managed to read, and the error rate shows how many it missed, which is why the table adds up to 189 samples rather than 298.

Add `--flamegraph -o profile.html` and you get an interactive flame graph instead. Real Python’s guide to `profiling.sampling` covers attaching to live processes and the different sampling modes.

**Note:** If you’re on a Mac, check out the release announcement before upgrading to macOS 27.0. An OS-level change can make IDLE and other Tkinter apps hang when a menu command opens a dialog, and the release team believes it affects all current versions of Tk.

If you install Python from python.org, then you’ll notice some changes there, too. The Windows 64-bit binaries now use the tail-calling interpreter, which the What’s New document credits with a speedup of fifteen to twenty percent on pyperformance over the older switch-case interpreter on its test machine. The macOS installer now installs free-threading support by default, so the GIL-free build is already there when you want to try it.

Miguel Grinberg ran his yearly benchmark, How Fast Is Python 3.15?, against the third release candidate, a few days before the final release. He concludes:

Python 3.15 is at best a fairly minor improvement over 3.14 in terms of performance. (Source)


The runtime of his recursive `fib(40)` benchmark only dropped from 7.17 seconds on 3.14 to 6.94 seconds on 3.15, about a three-percent gain. Turning on the experimental JIT shaved that down to 5.76 seconds, but Grinberg still advises against using it in production.

Free threading is where the numbers look most impressive. Grinberg also ran `fib(40)` in four threads at once, so the test does four times the work of the single-threaded run.

With the GIL, the threads took turns, and the standard build needed 32.5 seconds. The free-threaded build ran them in parallel and finished in 7.2 seconds, about four and a half times faster and roughly the time that one `fib(40)` takes on its own.

He measured roughly the same ratio on 3.14, so the gain comes from free threading itself rather than from this release. Results like this make free threading’s first real workloads, covered in September’s roundup, feel a lot less theoretical.

Keep in mind that PyPy still finishes the single-threaded run in 1.25 seconds, so CPython has a way to go before it wins any races on pure-Python loops.

If you’re curious about the JIT’s progress, the tutorial on Python’s JIT compiler in 3.15 explains what it does and doesn’t speed up.

Python 3.16.0a1 arrived right on schedule on October 13, four days after 3.15 shipped. Savannah Ostrowski is the release manager for 3.16, and the next alpha is due on November 10. As usual, a first alpha doesn’t give you much to play with yet: the draft What’s New in Python 3.16 has no headline features, though small changes are trickling in, like a new `stop_exception` parameter for `iter()`.

To see where 3.16 might be headed, you’ll need to look at the PEPs instead. One pair in particular has reopened a debate that’s older than some Python developers.

PEP 823 and PEP 824, both by Marc Mueller and sponsored by Guido van Rossum, propose four operators for working with values that might be `None`. If you’ve written C#, they’ll look familiar, and TypeScript and Kotlin have similar operators:

- **`?.` and `?[ ]`** (PEP 823) access an attribute or an item by index only if the object isn’t`None` . Otherwise, the whole chain short-circuits to`None` .
- **`??`** (PEP 824) returns its left side unless it’s`None` , in which case it returns the right side.
- **`??=`** (PEP 824) assigns a value only if the target is currently`None` .

Here’s the kind of code PEP 823 wants to replace. In this function, `data.customer` might be `None`, and so might the customer’s `user`. If a user exists, then it always has a `name` string:

```
def get_customer_name(data):
    if data.customer is not None:
        if data.customer.user is not None:
            return data.customer.user.name.lower()
    return None
```
Under PEP 823, the nested checks would collapse into a single expression. The `?.` short-circuits to `None` as soon as `customer` or `user` is `None`, while the plain `.lower()` at the end relies on the name being there. This is proposed syntax, so it raises a `SyntaxError` on every released version of Python, including the new 3.16 alpha:

```
def get_customer_name(data):
    return data.customer?.user?.name.lower()
```
PEP 824’s `??` would also help you avoid a subtle bug in a common idiom. Writing `age = user_age or "unknown"` replaces `None`, but it also replaces `0`, `""`, and anything else that’s falsy. In contrast, `user_age ?? "unknown"` would only kick in for `None`.

You may have heard of these operators before, because PEP 505 proposed them back in 2015, and it’s been deferred for years. The new PEPs split the idea in half, and Mueller has confirmed that either one could be accepted without the other.

The discussion threads for PEP 823 and PEP 824 have collected nearly two hundred posts between them, and opinions are split. Supporters of `??` like that it only ever reacts to `None`, while Paul Moore wrote that the semantics of `?.` weren’t obvious to him. Others find the new operators un-Pythonic.

Both PEPs are still drafts, so feel free to weigh in if you have an opinion about the new syntax.

There are two other new proposals for 3.16:

- **PEP 851** by Stan Ulbrych would add`list.discard()` . It removes the first matching item and returns`True` , or returns`False` if there’s nothing to remove, instead of raising a`ValueError` like`.remove()` does. The PEP found more than a thousand`if x in lst: lst.remove(x)` patterns in the top 5,000 PyPI projects. Until the method is available, How to Remove Items From Lists in Python covers your options.
- **PEP 848** by Mark Shannon is the newest attempt at an incremental garbage collector, a design that has been pulled twice already. The 3.14 version was rolled back in 3.14.5 because garbage piled up and caused memory pressure in production. This version adds generations back in, and it claims pause times up to 100 times shorter on some programs plus a speedup of 3 to 5 percent on pyperformance.

Last month’s roundup covered PEP 845’s leading-dot value patterns. Its authors submitted it to the Steering Council on October 5, and three days later, Thomas Wouters sent it back, writing that the council considered the submission severely premature.

However, the council didn’t reject the PEP. It wants more discussion and more core developers involved before it makes a decision.

While 3.16 is just getting started, Python 3.10 is officially done. Python 3.10.22, released on October 1, was the series’ final release, and 3.10 will get no more security fixes. If anything you maintain still runs on it, then it’s time to plan the upgrade. Libraries are already moving on, as you’ll see in the Library and Tooling Updates section below.

On October 20, Dennis Jackson went public with the news that eight intermediate certificate authorities run by **eMudhra**, an Indian CA whose roots are trusted by Mozilla, Microsoft, Apple, and Google, used badly generated private keys.

Some quick background helps here. When your code opens an HTTPS connection, the server presents a certificate signed by an intermediate certificate authority (CA), whose certificate is in turn signed by a root CA. Your client trusts the connection only if that chain ends at a root in its **CA bundle**, the list of root certificates it trusts.

So if an intermediate’s private key leaks, then whoever has the key can sign certificates for any site, and every client that trusts the root will accept them.

In a reply to the Python-specific advisory, core developer Christian Heimes explained just how weak the keys were: in all eight of them, the private key value `d` was smaller than 256. That meant anyone could recover the keys from the public certificates and impersonate any website.

Firefox and Chrome had blocked the certificates three days before the disclosure, and Microsoft and Apple followed by October 22.

However, fixing Python took more work, because much of the ecosystem doesn’t use your operating system’s trust store, so updating your browser or OS doesn’t fix it. requests and HTTPX both default to the CA bundle from `certifi`, and `pip` vendors `certifi`, which means `pip` ships and always trusts its own copy.

Seth Larson, the PSF’s Security Developer-in-Residence, warned that unpatched tools were open to machine-in-the-middle attacks, including on connections to python.org and pypi.org.

The fixes rolled out in layers over the following week:

- **`certifi` 2026.10.18** removed the root certificate that the compromised intermediates chain to, the next release added safe ones back in, and`pip` 26.2.2 followed on October 20 with that bundle vendored inside.
- **`uv` 0.13.4** dropped the same root from its built-in certificate list on October 20.
- **Python 3.15.1, 3.14.9, and 3.13.17** arrived on October 21 as fast-tracked security releases that bundle the fixed`pip` .
- **Python 3.15.2, 3.14.10, 3.13.18, 3.12.16, and 3.11.18** followed on October 26 with an additional safeguard. The`ssl` module now carries a small blocklist of the compromised public keys and rejects them during verification, no matter which CA bundle you load, addressing CVE-2026-115557.

Only 3.15.2 and 3.14.10 come with binary installers, while the older branches are source-only security releases. The October 26 batch also fixes a `tarfile` extraction filter bypass that could write files outside the destination directory, so it’s worth installing even if certificates aren’t keeping you up at night.

Patching takes three separate steps, because the interpreter, the packages in each environment, and `uv` all carry their own copy of the fix.

First, upgrade Python itself with the tool you used to install it, such as the python.org installer, your system package manager, or `uv python install`. Then check the version:

```
$ python --version
Python 3.14.10
```
You’re looking for 3.15.2, 3.14.10, 3.13.18, 3.12.16, or 3.11.18, depending on your branch. Virtual environments keep pointing at the interpreter they were created with, so re-create any environment whose interpreter you replaced.

Second, update `pip` and `certifi`. You need to repeat this in every virtual environment and container on your machine:

```
$ python -m pip install --upgrade pip certifi
$ python -m pip --version
pip 26.2.2 from .../site-packages/pip (python 3.14)
$ python -c "import certifi; print(certifi.__version__)"
2026.10.23
```
You’re looking for `pip` 26.2.2 or later and `certifi` 2026.10.19 or later. Running `python -m certifi` prints the path to the bundle rather than its version, so it won’t tell you whether you’re patched.

Third, if you use `uv`, then update it once per machine. Standalone installs can update themselves:

```
$ uv self update
$ uv --version
```
You’re looking for `uv` 0.13.6 or later. If you installed `uv` through `pip`, pipx, Homebrew, or another package manager, then `uv self update` won’t work, so upgrade it the same way you installed it. If you build Docker images, then rebuild them without the layer cache so you don’t keep an old `pip` around.

**Note:** The certificate scramble landed in the same week as Real Python Podcast Episode 316, where Seth Larson talks about how the Python ecosystem is adapting to supply chain threats. For a hands-on take, Supply Chain Security for the Age of AI-Speed Exploits covers the habits that keep your dependencies safe.

It’s the second month in a row that a security story has turned on a small detail, after `str.lower()` turned out to be a vulnerability last month. Larson has also written up the pain points of the current setup, and he comes to a bigger conclusion:

An over-arching opinion that I have (and I am willing to be wrong) is that PyPI should no longer be a certificate distribution channel. (Source)


Chances are, the question of how Python tools should find trusted certificates will come up again in next year’s packaging discussions.

Outside of the release notes, a browser project wound down a while ago, PyPI let Outlook users back in, and the PSF welcomed four new Fellows and relaunched its wikis. You’ll also meet a `+` operator that reads the call stack.

Back in August, maintainer Andrea Giammarchi quietly posted a short announcement that PyScript is in maintenance mode. It’s older news, but it’s easy to have missed, and it matters if you’ve built anything on PyScript. The PyScript API is frozen, the project accepts no new issues or pull requests, and only Pyodide-related updates will continue at a much slower pace.

As for the reason, Giammarchi writes that PyScript simply remained a niche project. Existing projects keep working, and the code is Apache-licensed, so anyone can fork it.

When Real Python took a first look at PyScript in 2022, PyScript promised to put Python next to JavaScript in every browser. That vision didn’t quite pan out. However, Pyodide, the CPython-on-WebAssembly project that PyScript builds on, is still under active development, and it’s one of the few ways to run NumPy or pandas in a browser tab.

Are there any Python-in-the-browser projects that you currently use and recommend? Or do you think there’s no need for these types of ports, with coding agents capable of translating code into JavaScript or WebAssembly?

**Tip:** If you’re still struggling to get reliable output from coding agents and want to fast-track and future-proof your skills, then join the upcoming Codex for Python Developers live course that runs on November 14 and 15. You’ll use Codex in the terminal to develop, test, and release a Python project, creating reusable skills along the way.

In June 2024, a malware campaign that registered accounts in bulk pushed PyPI to block Outlook email domains entirely. On October 16, Mike Fiedler announced that the ban is over. PyPI now checks new addresses with an email reputation service donated by UserCheck and automatically blocks disposable domains, so you can once again register a PyPI account with an outlook.com or hotmail.com address.

A week later, PyPI also capped version strings at 128 characters. Before you worry, the post notes that this affects almost no one. Exactly two of the roughly 9.9 million releases on PyPI had longer version strings, and the record holder was 217 characters long.

The PSF named its Q3 2026 Fellows: **Aldo Caneo**, **Donghee Na**, **Paul Moore**, and **Takayuki Shimizukawa**. Donghee Na is a CPython core developer and current Steering Council member, Paul Moore is a longtime `pip` maintainer, and Takayuki Shimizukawa co-maintains Sphinx and serves on the PyCon JP Association’s accounting board. Congratulations to all four! If you know someone who deserves the honor, then nominations for the next round close on November 20.

The PSF also relaunched its wikis, which had been read-only since February because of scraping traffic, almost entirely from AI bots, and security concerns about the outdated infrastructure they ran on. The new wikis are static sites on GitHub Pages, and you can now edit them through pull requests. The PSF acknowledged that retiring the old wikis without notice had been frustrating for contributors.

Arie Bovenberg dug into Pendulum’s `+` operator and found that its result depended on who called it. In Pendulum 3.2, adding a `timedelta` to a `DateTime` inspected the call stack. If the caller was a function named `astimezone`, then `+` did wall clock arithmetic like the standard library, ignoring daylight saving changes. Otherwise, it did Pendulum’s own daylight saving time-aware math.

The hack existed because Pendulum’s `DateTime` subclasses the standard library’s `datetime`, and `zoneinfo` calls `+` internally while expecting standard behavior. Pendulum 3.3, released on October 12, removed the stack inspection, and `+` got about five times faster as a bonus. Keep in mind that the problem goes beyond Pendulum, because a subclass that changes how an operator behaves will eventually meet code that expects the original behavior.

With 3.15 out the door, the big libraries spent October catching up, and several of them used the occasion to raise their minimum Python version. If you still support Python 3.10 anywhere, then note that Pillow 13, Polars 2.1, and PyTorch 2.15 just dropped it, joining pandas 3, so expect your dependency resolver to start pinning older versions.

pandas 3.1.0 arrived on October 22, and its headline feature will look familiar if you’ve used Polars. The new `DataFrame.select()` method picks existing columns and computes new ones in a single call, using the `pd.col()` expressions that debuted in pandas 3.0. Later arguments can even refer to columns that earlier arguments created:

```
>>> import pandas as pd
>>> df = pd.DataFrame({
...     "first_name": ["John", "Alice", "Bob"],
...     "last_name": ["Smith", "Cooper", "Marley"],
...     "age": [61, 22, 35],
... })
>>> df.select(
...     "first_name",
...     age_months=pd.col("age") * 12,
...     age_days=pd.col("age_months") * 30,
... )
  first_name  age_months  age_days
0       John         732     21960
1      Alice         264      7920
2        Bob         420     12600
```
Plain `df[["first_name", "age"]]` still works for picking existing labels, but it doesn’t accept expressions. `.filter()` also gained a `cond` argument for row filtering, so `df.filter(cond=pd.col("age") > 30)` keeps John and Bob. The old `items`, `like`, and `regex` arguments, which filter columns rather than rows, are on their way to deprecation.

The release also promises single-threaded `read_csv()` calls that are typically two to three times faster than in 3.0, and it deprecates `inplace=True` on methods like `.drop()`, `.rename()`, and `.sort_values()`. pandas 3.1 requires Python 3.11 or later. If you’re weighing the two libraries, Polars vs pandas is a good place to start.

Polars 2.0 went final on October 6 after the release candidates covered in last month’s roundup, so the row order caveat now applies to everyone who upgrades.

The streaming engine is now the default for every `LazyFrame` query, and it doesn’t guarantee row order for some operations. If any of your code or tests depend on the order of a `.group_by()`, join, or unpivot result, then pass `maintain_order=True` or sort explicitly before you upgrade:

```
result = (
    orders.lazy()
    .group_by("customer_id", maintain_order=True)
    .agg(pl.col("amount").sum())
    .collect()
)
```
The final release also turns on spilling to disk by default when a query uses about eighty percent of your RAM, and it adds a new `Map` data type and much fuller SQL support. Polars 2.1 followed on October 25. It disallows mixing strings and numbers in eager arithmetic.

Astral shipped `uv` 0.13.0 and Ruff 0.17.0 on the same day, October 9, and both are minor releases with breaking changes, following the pattern of `uv` 0.12 and Ruff 0.16 in July.

In `uv`, Python 3.15 is now the default version for new downloads, so a bare `uv python install` on a fresh machine gives you 3.15. `uv` still uses interpreters you’ve already installed. Run `uv python pin 3.14` in a project if you’re not ready to move. Less visibly, `uv` now honors `--require-hashes` in constraints files that you include with `-c`, so an install that used to pass can fail if a requirement is missing a hash.

Ruff’s default target version, which it uses when your project doesn’t specify one, moved from Python 3.10 to 3.11, in line with 3.10’s end of life. If your project doesn’t set `requires-python`, or if you support an older Python version, then pin the target version explicitly in your `pyproject.toml`:

```
[tool.ruff]
target-version = "py310"
```
An explicit `target-version` wins over the version that Ruff infers from `requires-python`, which also keeps upgrade rules like `UP040` from suggesting syntax that your oldest supported Python can’t parse.

More surprisingly, most of the `flake8-datetimez` rules that flag naive datetimes, such as `DTZ005` for `datetime.now()` without a time zone, are no longer on by default. A CPython `datetime` maintainer had argued that naive datetimes are a legitimate way to represent local time and that the rules taught bad habits.

The three rules that the issue asked to keep, `DTZ002` to `DTZ004`, which flag `datetime.today()`, `datetime.utcnow()`, and `datetime.utcfromtimestamp()`, are still on. You can turn the rest back on with `extend-select = ["DTZ"]`. Real Python’s Ruff tutorial covers additional configuration options.

Astral also announced an experimental `uv` and ty integration on October 13. The type checker can now read inline script metadata and ask `uv` to build each script’s environment, and a new `uv check` command runs ty across your project. `uv check` turns the integration on automatically, but building environments from inline script metadata is opt-in for now, because it can run third-party code.

pytest 9.2.0, released on October 27, makes the `indirect`, `ids`, and `scope` arguments of `@pytest.mark.parametrize` keyword-only. If you’ve passed `indirect` positionally to route values through a fixture, then test collection will fail after you upgrade:

```
# Fails on pytest 9.2
@pytest.mark.parametrize("user", ["alice", "bob"], True)
# Works on all versions
@pytest.mark.parametrize("user", ["alice", "bob"], indirect=True)
```
Here, `user` is a fixture that receives each value through `request.param`. The positional form worked on pytest 9.1. It now raises a `TypeError` at collection time.

The stricter rules also have an upside, because passing `ids` as the third positional argument never worked. pytest read the list as `indirect` and complained about an `indirect fixture 'x'` that you’d never asked for. Now that mistake also gets a `TypeError` that points straight at the call. If you’re new to parametrized tests, the pytest tutorial walks through them.

Here are three more release highlights from October:

- **Django 6.1.2, 6.0.9, and 5.2.18** fix four security issues, including two denial-of-service bugs and a model formset flaw. On models with an editable primary key, the vulnerability let forged POST data delete objects outside the formset’s queryset or create objects through edit-only formsets.
- **Pydantic 2.14.0** went final after the betas mentioned in last month’s roundup. It drops Python 3.9 and adds support for 3.15.
- **Pillow 13.0.0** drops Python 3.10, adds 3.15, and removes a few older APIs.

If you run Django in production, the security releases are the ones to install first.

For the third month in a row, AI providers shipped changes that Python developers need to keep up with. After September’s cascade of breaking SDK releases and last month’s API breakages, October brought another round. Anthropic retired request parameters that many scripts rely on, while OpenAI added a new endpoint and a type change to go with it.

On October 21, Anthropic released API version `2026-10-21` along with version 2.0.0 of its Python SDK, which sends the new version by default.

The migration guide has the details. The change most likely to catch people out is that, under the new version, any request that includes `temperature`, `top_p`, or `top_k` returns a 400 error, even if you use the parameter’s default value. The SDK already dropped these parameters in version 1.0.0, so passing them as keyword arguments raises a `TypeError` before your request ever leaves your machine.

If you lowered the temperature to get more focused output, then you’ll lose that option, and according to the guide, no other setting replaces it. The SDK brings four other changes, and all but the last one can break existing code:

- The SDK now sends your API key as an `Authorization: Bearer` header instead of`X-Api-Key` , which matters if a proxy or gateway inspects that header.
- A client accepts only one credential, so passing both `api_key` and`auth_token` raises a`ValueError` .
- `MessageParam` is now a union of per-role types, and calling it like a class raises a`TypeError` . Plain dictionaries still work.
- From 2.0.0 on, the SDK is licensed under Apache 2.0 instead of MIT.

If you aren’t ready for the other changes, then pin the old SDK with `python -m pip install "anthropic>=1,<2"`, where 1.17.0 is the last 1.x release and still sends the old `2023-06-01` API version. Neither SDK accepts `temperature` as a keyword argument, but under the old API version, you can still pass it through `extra_body`.

If you’ve already upgraded to SDK 2.x, then you can buy time by setting the old API version through `default_headers`. This is a temporary compatibility path, not a migration to the new API:

```
import anthropic
client = anthropic.Anthropic(
    default_headers={"anthropic-version": "2023-06-01"},
)
message = client.messages.create(
    model="claude-sonnet-4-6",
    max_tokens=1024,
    messages=[{"role": "user", "content": "Summarize PEP 823."}],
    extra_body={"temperature": 0.2},
)
```
The header switches the request back to the old API version, and `extra_body` adds `temperature` to the request body without going through the SDK’s keyword arguments. Once you drop the header, the same request returns a 400 error. The workaround also helps only on models that still accept a non-default `temperature`, such as Claude Sonnet 4.6. Claude 4.7 and later models, including Haiku 5.5, accept only the default value under the old API version.

Similar rules apply to the new Claude Haiku 5.5, which Anthropic released on October 7. It has a one-million-token context window and costs $0.10 per million input tokens for prompts up to 100,000 tokens, a tenth of Haiku 4.5’s price. But code written for Haiku 4.5 may need changes, because requests that use sampling parameters, manual `budget_tokens` thinking, or assistant message prefill can all return 400 errors on the new model.

**Note:** If you’re updating a Claude integration, then the Using the Claude API in Python video course shows you how to send prompts, steer responses with system instructions, and get structured output. It’s a good refresher before you migrate.

Whichever model you use, run your typical requests after you upgrade the SDK. Watch for two kinds of failure: `TypeError` and `ValueError` exceptions that the SDK raises locally before sending anything, and 400 errors from the API for requests that the new version rejects.

After the viral success of Jev, many providers, including Amazon, Microsoft, and Cloudflare, have added decision-type models and endpoints. OpenAI joined in the fun, too. Following a preview at DevDay in September, its Decisions API entered beta on October 6 and became generally available on October 22.

You send it an input, such as text, images, or JSON, plus up to 400 questions, and it returns a typed answer for each question: the probability that a condition is true, a choice from a list, or a score. Choices and scores also come with a confidence value. If you’ve been coaxing a chat model into returning JSON for classification tasks, then this is the purpose-built version.

However, if you already use the Decisions API from Python, then keep an eye on one change in openai-python 3.37.0: a question’s `instructions` can now be either a string or a list of structured text, image, or JSON parts. Code that calls string methods on it should check with `isinstance()` first.

OpenAI also released GPT-6.1 Terra on October 26, which it describes as a fast model for visual reasoning, computer use, and writing. Take a look at how it compares with other models on the Real Python AI Benchmark.

Following last month’s gradient changes in PyTorch 2.14, PyTorch 2.15.0 arrived on October 28 with three changes to check before you upgrade:

1. **No more Python 3.10 wheels.** On Python 3.10,`pip` can’t find a 2.15 wheel, so you’ll need Python 3.11 or newer, or you can stay on 2.14.
2. **Stricter checkpoint loading.** Nested tensors are no longer on the allowlist for`torch.load()` with`weights_only=True` , which is the default. To load a checkpoint that contains them, you can allow them with`torch.serialization.safe_globals()` , or pass`weights_only=False` , but only for files you trust.
3. **A rewritten NCCL backend.** Distributed training now defaults to a new implementation of NCCL, NVIDIA’s library for communication between GPUs. The old one remains available as`backend="nccl-legacy"` for this release only.

On the bright side, `torch.compile()` now works on Python 3.15, which removes one of the last reasons for PyTorch users to stay on 3.14. Elsewhere in the AI stack, langchain-core 1.7.0 changed how tool call arguments stream, which matters only if you parse raw stream events.

An Agent Skill is a folder with a `SKILL.md` file that teaches a coding agent how to handle a specific task, plus any scripts or reference material that the agent needs. Because the format is just files, you can reuse the same skill across different agents and tools.

On October 22, Anthropic donated the Agent Skills standard to the Agentic AI Foundation, the Linux Foundation project it co-founded with Block and OpenAI. The foundation already hosts MCP, the Model Context Protocol for connecting AI applications to tools and data. Agent Skills support in the Claude API became generally available on August 19, and Anthropic says more than seventy platforms now support the format.

Your existing skills keep working as before. However, if you’re deciding whether to invest in the format, then the move makes it less likely that a skill you write today will be locked into one vendor’s tools. For a practical example, How to Use Claude Code Skills for Better Python Docstrings builds one from scratch.

If you’d rather learn agentic coding with someone guiding you live, then the 2-day Codex for Python Developers live course runs on November 14 and 15. You’ll build, test, and ship a complete Python project with Codex in your terminal, and you’ll leave with reusable Codex skills of your own.

Stop Falling Behind on AI. Catch Up in Five Minutes.

New models, new prices, new agent tools, all the time. Our new AI newsletter gives you the handful of stories you actually need as a developer. Free, straight from the Real Python team.

Several PyCons take place in November, and a few calls for proposals close soon:

- **PyCon ES 2026:** November 6 to 8 in Barcelona, Spain
- **PyCon Chile:** November 14 to 15 in Santiago, Chile, with the second day online
- **PyCon Sweden:** November 19 to 20 in Stockholm, Sweden
- **PyCon Ireland:** November 21 in Dublin, Ireland
- **PyLadiesCon 2026:** December, online and in multiple languages
- **DjangoCon Europe 2027:** February 17 to 21 in Innsbruck, Austria, with a call for proposals that closes on November 15
- **PyCascades 2027:** March 20 to 21 in Seattle, Washington, with an extended call for proposals that closes on November 9

PyCon US 2027 will be in Long Beach, California, but its call for proposals isn’t open at the time of writing. Last year’s opened at the end of October, so keep an eye on the PyCon blog if you’re planning a talk. For more deadlines, Python Deadlines tracks open calls for proposals for conferences around the world.

Many of October’s Real Python tutorials covered AI, including guides to Claude Code hooks and MCP. The team also published a practical guide to performance optimization and a fix for one of Python’s most confusing errors, circular imports.

You can get started with these new **written tutorials**:

If you prefer learning by watching, then check out these new **video courses**:

Test your understanding with these new **quizzes**:

If you’d rather learn by doing, then try the **coding exercises**. Real Python now has more than 700 of them, from basic to advanced, and you solve each one right in your browser without installing anything. Many sit inside tutorials and course lessons, so you can practice right where you learn. Start on the Coding Exercises page, or search all exercises by topic.

You can explore a range of topics on **The Real Python Podcast** this month:

- Episode 313: Python 3.15: Exploring the New Features
- Episode 314: Codemoo: Demonstrating How Coding Agents Work
- Episode 315: Python Pitfalls: Common Mistakes and How to Avoid Them
- Episode 316: The Future of Python Security: Trends, Challenges, and Solutions
- Episode 317: Demystifying Python Memory Management and Choosing a Project Layout

If coding agents still feel like a black box, then start with Episode 314. Geir Arne Hjelle joins the show to walk through Codemoo, a demo project that pulls apart the core pieces of a coding agent so you can see how they fit together. It’s a great companion to this month’s AI news. The discussion gives you context for how API changes, such as a removed `temperature` parameter, can affect an agent.

You can also see agents up close in Real Python Live, a free weekly session on Zoom. If you missed the October 28 session, the replay of Does a Better Prompt Really Help? pits two agents with two different prompts against the same task. The next session, **AI Agents Aren’t Magic: Let’s Build One in Plain Python**, is on November 11 at 20:00 UTC, and everyone’s welcome to join.

November looks like a quieter month: Python 3.16.0a2 is due on November 10, and the discussions around PEPs 823 and 824 will decide whether `?.` and `??` have a real shot at 3.16.

Meanwhile, if you haven’t yet upgraded `pip` and `certifi` in every environment and `uv` on each machine, then put it at the top of your list. The certificate fixes only protect the environments you actually update.

Here’s what’s coming up in the next few weeks:

- **November 9:** The extended PyCascades call for proposals closes.
- **November 10:** Python 3.16.0a2 is scheduled for release.
- **November 11:** Real Python Live builds an AI agent in plain Python, starting from an empty file.
- **November 14 to 15:** The Codex for Python Developers live course takes place. Enrollment closes on November 14 at 06:00 UTC.
- **November 15:** The DjangoCon Europe 2027 call for proposals closes.
- **November 20:** Nominations for the next round of PSF Fellows close.

Looking back, most of October’s ecosystem work happened after release day. Python 3.15 took well over a year to build and only one blog post to announce, and then pandas, PyTorch, Pydantic, and `uv` spent the rest of the month making room for it.

And thanks to everyone who worked on keeping Python secure by fixing the fallout from the certificate issues. See you next month!
