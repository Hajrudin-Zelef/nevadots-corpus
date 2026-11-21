---
id: collect-261121-veille-ia/securite/java4shell-what-happened-and-what-to-do-now
title: "Java4Shell: What Happened and What to Do Now"
domain: securite
role: reference
task: article
actors: []
dates: ["2026-11-17"]
keywords: ["java", "base64", "java4shell", "happened", "like", "them", "about", "thats"]
source: https://medium.com/@ashenbhagye/java4shell-what-happened-and-what-to-do-now-6c5721503d99
source_anchor: ""
source_lines: [1, 105]
section: "Securite"
---
# Java4Shell: What Happened and What to Do Now

## A simple guide to the new Java bug, who it hits, and the one-line fix you can use today.

What if a short line of random-looking text could take over your server?

Not a virus. Not a stolen password. Just about 65 characters of Base64, sent to the wrong place.

That’s the story of Java4Shell. I’ll be honest: when I first saw the name this week, my stomach dropped a little. It sounds like Log4Shell’s scary cousin. If you were around in December 2021, you remember that mess.

So I sat down and read the reports. Here’s what I learned, in plain words. What happened, who should worry, and what you can do today.

## First, what is Base64?

Base64 is a way to turn data into plain text letters. Pictures, files, login tokens. It’s everywhere.

Think of it like the scissors in your kitchen drawer. You use them all the time and never think about them.

Java has a built-in Base64 decoder. Lots of apps use it every second, and nobody notices.

## So what went wrong?

Java likes to be fast. On some newer Intel and AMD chips, it uses a shortcut. These chips have special powers called AVX-512, which let them handle many letters at once.

The bug lives inside that shortcut.

If someone sends badly formed Base64, Java gets the length math wrong. It writes more data than the space it set aside.

Picture pouring a full jug of water into a small glass. The extra water spills onto the table. In a computer, that “table” is memory that belongs to something else.

Reports say a string of only 65 characters is enough to trigger it on the right machine. That’s shorter than this paragraph.

## Why is spilled memory such a big deal?

When memory spills, three bad things can happen. Secrets can leak. The app can crash. Or, worst of all, an attacker can run their own code on your server.

And they don’t need to log in first.

The security firm Calif found the bug and reported it to Oracle before going public. They say they used it to take over Jira, Confluence, Jenkins and Keycloak servers without a password. Those are tools a lot of companies run every single day.

The official name is CVE-2026-114938. It went public on November 17, 2026. Oracle rates it 8.1 out of 10 and says the attack is hard to pull off. Calif gives it 9.2 using a newer scoring system.

## The AI part surprised me most

Calif says their researcher used an AI coding agent (OpenAI’s Codex) to find the bug in about one hour. Then it took about two more hours to build a working attack. The cost? Around $200.

I read that twice.

## Get Ashen Bhagya’s stories in your inbox

Join Medium for free to get updates from this writer.

Bugs like this used to take experts weeks. If the good guys can find them this fast, so can the bad guys. The gap between “bug announced” and “bug attacked” is getting very small.

## Should you panic? Probably not

Here’s the good news. You need three things to line up at the same time.

First, an x86-64 computer whose chip supports AVX-512 VBMI. Many newer servers have this. ARM machines, like Apple’s M-series Macs or AWS Graviton servers, are not affected.

Second, an affected Java version. That means OpenJDK 17.0.6 and newer, including Java 21, 25 and 27. Oracle’s Java 8 Enterprise Performance Pack is affected too.

Third, your app has to decode Base64 that comes from outside, like from a user or a web request.

One thing that’s easy to miss: plain Java 8 and Java 11 don’t have this fast decoder at all. Some early posts just said “Java 8” without the fine print. If you’re on regular Java 8 or 11, this bug isn’t your problem.

Calif also says there is no single attack that works everywhere. It depends on how each app uses Base64. For now, they’re holding back their app-specific attacks so people have time to update.

That’s kind of them. But I wouldn’t count on everyone else being that kind.

## What you can do today

**Step 1: Check if you’re in the danger zone.** On a Linux server, run these three commands:

```
uname -m
grep -o -w avx512vbmi /proc/cpuinfo | head -1
java -version
```
If you see `x86_64`, then `avx512vbmi`, and a Java version of 17.0.6 or newer, take this seriously.

**Step 2: Update Java.** Oracle released fixed builds on November 17 for Java 17, 21, 25 and 27. If you use another vendor, like Temurin, Corretto or Red Hat, check their site for patched builds.

**Step 3: Can’t update yet? Flip the switch.** Add this to your Java startup options:

`-XX:-UseBASE64Intrinsics`
This turns off the fast shortcut. Base64 might get a tiny bit slower, but the buggy code won’t run. It’s like taking the stairs while the elevator gets fixed.

**Step 4: Hunt for hidden Java.** This is where people get caught.

Many apps carry their own copy of Java inside them. Docker images do too. Updating the main Java on your server does not fix those copies. You have to check each one.

Start with anything facing the internet. Login pages, APIs, single sign-on servers. Those are the doors attackers knock on first.

## My simple takeaway

This isn’t a new lesson. It’s an old lesson moving at a new speed.

Know where your Java lives. Patch quickly. And keep a backup switch ready for the day you can’t patch right away.

Because the next bug might be found in an hour, too.

So, have you checked your servers yet? Tell me in the comments what you found, or what tripped you up. And if this helped, send it to the one person on your team who “owns the Java stuff.”

They’ll thank you. Probably.
