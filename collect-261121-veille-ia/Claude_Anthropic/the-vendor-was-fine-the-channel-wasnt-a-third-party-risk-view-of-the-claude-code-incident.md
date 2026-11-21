---
id: collect-261121-veille-ia/claude-anthropic/the-vendor-was-fine-the-channel-wasnt-a-third-party-risk-view-of-the-claude-code-incident
title: "The vendor was fine. The channel wasn’t. A third-party risk view of the Claude Code incident"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-10-29"]
keywords: ["extension", "code", "cursor", "marketplace", "claude", "vendor", "channel", "anthropic"]
source: https://panorays.com/blog/cursor-marketplace-supply-chain-attack/
source_anchor: ""
source_lines: [1, 114]
section: "Claude_Anthropic"
---
**The vendor was fine.** *The channel wasn’t.*

Nobody broke into Anthropic. Nobody broke into the companies whose developers were affected. And for about eleven hours on October 29, 2026, up to 5,000 people who installed or auto-updated the official Claude Code extension through the Cursor Marketplace got an attacker’s SSH key planted in their `authorized_keys` file, a persistence mechanism on their machine, and a beacon that reported their hostname, username, Git email and the names of every environment variable that looked like a secret.

The real extension was inside the package and kept working. Nothing looked wrong. That is the detail I keep coming back to, and the reason this post is not about Cursor, or Anthropic, or one bad build. It is about the connection most third-party risk programs never assess: the channel that delivers a trusted vendor’s software onto a developer’s laptop, and from there into everything that laptop can reach.

Here is what happened, how the backdoor worked, why I think this is a supply chain problem rather than a vendor problem, and what third-party risk teams should do over the next 72 hours. The Panorays Threat Center report carries the full timeline, MITRE ATT&CK mapping and indicators.

## Eleven hours, one marketplace, one trusted name

Claude Code is Anthropic’s coding agent, and its VS Code extension ships under the extension ID `anthropic.claude-code`. Cursor, the AI-first editor built on VS Code, runs its own extension marketplace, so Cursor users typically install the extension from there rather than from Microsoft’s store or Open VSX.

At about 2:27 a.m. Pacific time on October 29, a modified build of the extension, version 2.1.313, became available on the Cursor Marketplace. It carried the real extension ID, a plausible version number, and the real extension code. Anyone installing fresh, or with auto-update on, could receive it. At about 1:35 p.m. the window closed. Cursor has put the number at no more than 5,000 users.

Cursor pulled the malicious version as soon as it spotted it, then shut the whole marketplace down out of caution. It rotated its keys, rebuilt the marketplace, brought it back online, and worked with the hosting provider to take the attacker’s server offline. Anthropic published a clean 2.1.314 to Open VSX and the Microsoft marketplace the next day. By October 30, OpenSourceMalware had published a full analysis of the artifact, Cursor had posted its security notice, and the story was in the press. Cursor was explicit that the malicious build was not developed by Anthropic and that no other marketplace extension was affected.

For context, this landed ten days after five OVHcloud CLI releases were replaced with secret-stealing malware, and OpenSourceMalware has tentatively tied it to the GlassWorm operation, through a GitHub account compromised back in March. Different victims, same play: get a modified build into a trusted distribution path and let the auto-updater do the rest.

## A 66-line loader in front of the real thing

The attacker did not rewrite the extension. They prepended 66 lines of JavaScript to `extension.js`, wrapped so that any error was swallowed and the legitimate code loaded afterward. The loader ran the moment the extension activated in the editor. No conversation with Claude was needed, and there was no install script for a package scanner to inspect.

Three things happened:

1. **SSH access.** An attacker Ed25519 public key was appended to`~/.ssh/authorized_keys` , labeled`svc-telemetry` so it would blend in with the service accounts that live in that file. The loader then tried to switch on the local SSH service and open a reverse tunnel to`134.209.82[.]225` , forwarding the victim’s port 22 back to the attacker, with host-key checking disabled and a reconnect every 60 seconds.
2. **Persistence.** A fake`com.apple.sysmond.plist` LaunchAgent on macOS, an`@reboot` cron entry on Linux, and a scheduled task called SysMon that tries to run as SYSTEM on Windows, with a Startup-folder fallback. Supporting files lived in`~/.config/.smt` and`.smtk on macOS, with smt.cmd and smtk under %USERPROFILE%\.config on Windows` .
3. **Reconnaissance.** After a short delay the loader looked up the public IP and sent hostname, username, OS, architecture, home and working directories, the global Git email, and the names of up to 40 environment variables matching KEY, TOKEN, SECRET, PASS, AWS or GCP. All of it went over plain HTTP to`134.209.82[.]225:8099/beacon` . The response was ignored.

Two things limited the damage, and both are worth being honest about. No second-stage payload was ever downloaded; this build plants access and reports in, nothing more. And enabling SSH needs administrator rights, which the extension did not have on most macOS and Windows machines, so the tunnel often led nowhere. The key, the persistence and the reconnaissance still landed regardless. That is why everyone in the window is being told to remediate, not just to update.

Two more caveats. Nobody has said how the modified build got onto the marketplace; Cursor says it audited access and found no other extensions tampered with, and the attacker is unattributed. And some early coverage placed the package on Open VSX. Cursor’s notice describes its own marketplace, and The Hacker News found no evidence that it reached Open VSX or Microsoft’s marketplace.

## This is a supply chain problem, not only a Cursor problem

If your third-party risk program had assessed Anthropic last month, it would most likely have found nothing wrong, and it would have been right. It would also have told you nothing about this incident, because the vendor was never the point of failure.

Three things make this a pattern rather than an event:

- **The channel is a dependency nobody onboarded.** Developers trust the extension because they trust Anthropic. They get it from a marketplace because that is how the editor works. Nobody filled in a vendor form for the marketplace, and nobody assessed what happens if the marketplace, rather than the vendor, serves a bad build. The same is true of package registries, CLI installers, container image repositories and every other distribution path between a vendor and your engineers. The OVHcloud and Nx Console incidents broke at the same seam.
- **AI coding tools are being adopted faster than they are being assessed.** Cursor, Claude Code and their peers have moved from curiosity to everyday tooling remarkably fast. They run with the developer’s full access, they update themselves, and they sit on the machines that hold source code, cloud credentials and CI/CD secrets. The beacon in this incident was literally mapping which secrets a machine held. An extension is a third party with a shell on your endpoint, and it deserves to be treated as one.
- **Your suppliers’ developers are part of your attack surface.** Those installs were individual engineers, at companies of every size, and any of them could work for one of your suppliers. If a vendor that handles your data has an engineer with an attacker key in their`authorized_keys` file, that is your exposure, and the only way to know is to ask, get a dated answer, and hold them to it.

None of this is a reason to stop using AI coding tools. It is a reason to put them in the inventory, map which suppliers depend on them, and have a question ready for the day one of their channels is abused. Cursor’s response was fast and transparent, Anthropic shipped the clean build the next day, and OpenSourceMalware published a full chain analysis within 24 hours. That is what a healthy connection looks like under pressure. The companies that benefited from it were the ones that could find out, quickly, whether it touched them.

## How this surfaced in Panorays

Panorays Threat Center tracks the incident, classified it as Contained once Cursor confirmed the takedown, and mapped the attacker behavior to ten MITRE ATT&CK techniques: software supply chain compromise, IDE extensions, SSH authorized keys, protocol tunneling, cron, scheduled task and LaunchAgent persistence, system and user discovery, and exfiltration over unencrypted HTTP. Cursor sits as the primary impacted company; Anthropic is listed because its extension ID and brand were abused, not because its systems were.

The part that matters for a third-party risk team is the TPCRM perspective. The report names the dependency (developer IDE marketplace integration), the exposure path (a trojanized extension served under a trusted identity executing on IDE startup), the assets at risk (workstation access, SSH keys, private source code, cloud tokens in environment variables), and the condition for exposure: engineers at your company or at your suppliers who downloaded or auto-updated the Claude Code extension through the Cursor Marketplace on October 29. It then does the one thing a news article cannot, which is to let you send a confirmation task to every company in your portfolio linked to Cursor, asking whether their developers installed 2.1.313, whether the injected key and persistence have been audited and removed, and whether sensitive credentials were rotated.

The full report, with the confirmation ladder, timeline, attack analysis, blast radius and indicators, is here:

## A 72-hour playbook for third-party risk teams

You cannot clean your vendors’ laptops. You can find out which vendors are exposed, get an answer you can hold them to, and verify it. Here is how we are approaching it with customers.

**Hours 0 to 12: scope.** Build the list of third parties whose engineers are likely to run Cursor, and who touch your source code, your cloud environment or your data: outsourced development shops, software vendors with a CI/CD relationship to you, integration partners, anyone with repository or deploy access. Add your own engineering org to the top of that list. Then check the quieter dependencies: contractors with personal machines, the agency that maintains your website, the vendor whose engineers you gave a VPN account to last year.

**Hours 12 to 24: prioritize.** Three questions decide the order. Does this party hold credentials to our environment (cloud roles, deploy keys, API tokens)? Do they have write access to code we ship? Is their development largely done on individual machines rather than managed build infrastructure? High on two of three goes to the top.

**Hours 24 to 48: ask, precisely.** Send a short attestation, not a 200-question survey. Three questions move things:

1. Did any of your engineers have the Claude Code extension installed through the Cursor Marketplace on October 29, 2026? If so, how many machines, and did any run version 2.1.313?
2. For each affected machine: has the `svc-telemetry` key been removed from`authorized_keys` , has the LaunchAgent, cron entry or scheduled task been removed, and has the machine been checked for an active tunnel to 134.209.82[.]225?
3. Which credentials present on those machines have been rotated, from a clean device, and when? Specifically cloud, CI/CD and repository tokens, and any key that grants access to our environment.

Ask for a dated answer from a named owner. “Our developers don’t use Cursor” is an answer only if someone has actually checked. “We’re looking into it” is a finding, not an answer.

**Hours 48 to 72: verify and track.** Where a vendor confirms rotation, ask for the date and the scope. Where they confirm none of their engineers were affected, ask how they know. Then keep watching: the speed with which your vendor base closes this tells you more about their security operations than any questionnaire will, and it is the same signal you will want the next time a channel is abused.

For Panorays customers, the Threat Center report, the Cursor supply chain view and the confirmation task are already in the platform. If you are not a customer, the three questions above are yours to use.

## For your own developers

The vendor work only makes sense if your own engineering org has done the same, so briefly. If anyone on your team runs Cursor, these are the places to look. Uninstalling the extension alone may not be enough, because everything below can survive it.

| Where | What to look for | Why | 
|---|---|---|
| Version | `anthropic.claude-code 2.1.313` | The trojanized build. 2.1.314 is clean. | 
| SSH key | `svc-telemetry in ~/.ssh/authorized_keys` | Attacker Ed25519 key, labeled to look like a service account | 
| Network | `134.209.82[.]225 (ports 22 and 8099)` | Reverse SSH tunnel target and HTTP beacon endpoint | 
| macOS | `~/Library/LaunchAgents/com.apple.sysmond.plist` | Fake LaunchAgent with RunAtLoad and KeepAlive | 
| Linux | `@reboot entry in the user’s crontab` | Persistence on boot | 
| Windows | `Scheduled task named SysMon, set to run as SYSTEM` | Startup-folder fallback if task creation fails | 
| Files | `~/.config/.smt and ~/.config/.smtk (macOS); %USERPROFILE%\.config\smt.cmd and smtk (Windows)` | Persistence script and embedded key material | 
| Process | `ssh or autossh with -N -R *:0:localhost:22` | The reverse tunnel, reconnecting every 60 seconds | 

Then, in order: update to Claude Code 2.1.314 or later; block 134.209.82[.]225 on ports 22 and 8099 at the egress; remove the key, the persistence and the files; kill any tunnel process; and rotate every credential that was on the machine, from a different, clean device. Treat the environment variable names the beacon collected as a shopping list the attacker now holds, and rotate in that order.

If you have endpoint telemetry, search back to October 29 for `authorized_keys` writes by an editor process, new LaunchAgents or scheduled tasks created by a non-admin user, and outbound connections to that address. Credit to OpenSourceMalware for publishing the analysis and hashes that make this hunt possible.

## What concerns me more than one extension

Sixty-six lines of code.

Eleven hours.

No exploit, no zero-day, no phishing email.

The attacker needed one thing: the ability to put a build under a trusted name in a place developers already trusted, and an auto-updater did the rest. Ten days ago it was a CLI tool. This week it was an IDE extension. The common thread is not the vendor or the product category. It is that the path between a vendor and your engineers has become a supply chain of its own, and almost nobody treats it as one.

The question security leaders should be asking this week is not only “did this touch us?” It is “which of our connections run through a channel like this, and would we know within a day if that channel were abused?” The teams that can answer that, map it to the suppliers who matter, and verify the answers they get back, will handle the next one in hours rather than weeks.

If your own vendor data tells a different story about who was exposed, I’d be interested to hear what you’re seeing.

*Demi Ben-Ari, Co-Founder and Chief Strategy Officer, Panorays*

### For Panorays customers

Search “Cursor” in Risk Insights. The Supply Chain Impact tab lists your direct and indirect suppliers linked to Cursor. The confirmation task “Confirm Cursor extension inspection and secret rotation” is ready to send to the companies in that list.

Don’t wait for a vendor incident to discover your exposure. Get the visibility you need to act. Book a demo today.

## Sources
