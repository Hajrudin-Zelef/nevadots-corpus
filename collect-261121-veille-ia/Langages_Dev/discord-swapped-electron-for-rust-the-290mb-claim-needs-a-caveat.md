---
id: collect-261121-veille-ia/langages-dev/discord-swapped-electron-for-rust-the-290mb-claim-needs-a-caveat
title: "Discord Swapped Electron for Rust. The 290MB Claim Needs a Caveat"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["discord", "rust", "electron", "client", "side", "people", "said", "claim"]
source: https://dev.to/dishant0406/discord-swapped-electron-for-rust-the-290mb-claim-needs-a-caveat-1n2o
source_anchor: ""
source_lines: [1, 107]
section: "Langages_Dev"
---
Stanislav Vishnevskiy posted a 15 second clip of two ThinkPads side by side, and the left side is the part that got me.

That's the Discord you and i have installed right now. Black window for about three seconds. Then a loading tip: "In the ancient days, Discord started as a browser-only app."

The joke writes itself. The app is still shipping a browser engine while telling you about its browser-only past.

The right side is the new client, codenamed Phoenix. It gets double clicked a second and a half later. Window fades in within a second.

By the time the Electron app finally shows its interface, the Rust client already has the friends list loaded.

**The old app still says "There are no friends online at this time" while the new one is done.**

The clip hit 2.3 million views. People asked if it was a tray launch.

Coral, who leads audio and video at Discord, said both were cold starts. Travis, who filmed it, pointed out that a tray launch would mean the friends list was already loaded.

Then came the number. 290.2MB of RAM. With two accounts signed in at once.

A Discord employee said in December 2025 that normal usage sits under 1GB, and the company tested auto restarting the app at 4GB. So 290MB is roughly 70% below the normal mark.

One reply called 290MB "still vile" for a chat app. Vishnevskiy answered: "Those 4k avatar decorations gotta render somewhere ;) but joking aside this client is actually running 2 accounts concurrently."

That reply is where i stopped scrolling and started taking notes.

Most people guessed Tauri. Tauri uses Rust for app logic but builds the interface in HTML and CSS inside the system webview. Discord didn't do that.

They went with GPUI, the framework from the Zed editor team. It draws the interface on the GPU with no HTML at all. Rust handles memory without a garbage collector.

| Tech | What it is | Role | 
|---|---|---|
| Rust | Systems language | New client and shared code | 
| GPUI | Rust UI framework from Zed | Draws the desktop app | 
| Electron | Chromium plus Node | Current app, being removed | 
| Tauri | Rust plus system webview | Not used | 

So the chat app stopped being a website wearing a coat.

The mobile apps drop React Native and share the same Rust core. The browser version keeps rendering HTML but moves its logic to WebAssembly. Web tech stays for the things that need it, like YouTube embeds and Activities.

Here's what the headlines skip. That number came from Task Manager on a ThinkPad, not a benchmark. Discord hasn't published a like for like test of both clients.

And the 4GB figure was a restart threshold Discord set for itself, not typical usage. Normal was under 1GB. So "4GB to 290MB" compares a ceiling to an idle snapshot.

That's not a knock on the client. The thing genuinely looks fast in the clip. But the math in the headline is doing a lot of work.

i had Discord open the entire time i was reading about this. Activity Monitor said 1.4GB. i closed it and opened it again out of spite.

A cold start comparison is real evidence. A single Task Manager screenshot is a claim.


i used to think the case for Electron was settled. One codebase, every platform, ship it. The cost of writing native UI three times was the whole argument.

A commenter on r/rust named its_lukebond put the shift better than any article did. GPUI maturing, plus AI writing most of the platform glue, is making that cost argument fall apart.

That's the story people are missing. Not the RAM. The price of not using Electron went down.

Which is also why this is bigger than Discord. If a company with a decade of React investment can walk away, the next one has less excuse.

here's a question people always ask when a web stack gets dropped. What happens to screen readers?

On Hacker News, a legally blind user said per app UI zoom in the web stack made their life easier. Someone else pointed out that Windows apps had accessibility before computers had 25MB of RAM total.

Both true. Which is the annoying part.

But a developer who built on GPUI, zac_attack_ on Reddit, said the crate's README still had a broken 404 link for "Accessibility." He also hit rendering bugs and rough edges.

**The framework's own docs couldn't find the accessibility page.**

Discord's VP of Engineering says accessibility and security matter as much as performance. i want that to be true. i also want to see the commit history.

the first time i tried this kind of thing, it was a small tool, not a chat app with millions of people. Immediate mode UI gets harder as the tree grows.

A commenter, Grollicus2, worried about exactly that. Thousands of chat messages in one component tree, all needing diffing on every change.

That's the risk specific to Discord. Zed mostly renders editor panes. Discord renders a firehose.

And modded clients probably die. Disconsented pointed out that client mods like BetterDiscord and Vencord depend on the Electron shell.

Blocking already misbehaves with ghost notifications. A rewrite is a clean place to close that door.

The build is called Phoenix. Which is a strange choice for a company that already rose from a browser.

Every big rewrite gets a codename like this. Phoenix, Nova, Atlas, something with a bird or a star. Nobody ships a rewrite called "Project Dave."

i once worked somewhere that named a migration after a Greek god. The migration failed twice and the name stopped being funny. Nobody says "Zeus is down" without sounding like they've lost it.

Phoenix is at least honest. It implies the old thing has to burn first.

If Discord runs fine on your machine, none of this changes your week. You will get the update eventually and it will feel slightly snappier. That's it.

The 290MB number also won't hold. As accessibility, features, and years of patches land, that figure creeps up.

Chrome used about 25MB per tab in 2010. It's often 500MB now. Every app gets heavier once real users show up.

And if you run a modded client, this is bad news dressed as good news. Performance you didn't ask for, at the cost of the thing you actually use.

Discord hasn't shipped a public date. Private testing starts soon, and the company says it's taking its time on stability and security. Believe that when the build is in your hands.

The part that stuck with me wasn't the RAM. It was Vishnevskiy thanking Travis for "finding these janky old laptops."

ThinkPads with a Copilot key. Machines from 2024. He called them janky.

That's a person who has been staring at this problem for years, finally watching the old thing lose to the new thing on hardware he clearly doesn't respect. There's something honest in that.

i still have the Electron app installed. i'll probably keep it until the day it updates itself into something faster. And i'll open Task Manager that day, out of habit, and judge the number.

## Top comments (0)
