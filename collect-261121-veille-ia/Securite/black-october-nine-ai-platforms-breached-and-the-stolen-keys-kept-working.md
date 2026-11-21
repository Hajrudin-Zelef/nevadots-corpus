---
id: collect-261121-veille-ia/securite/black-october-nine-ai-platforms-breached-and-the-stolen-keys-kept-working
title: "Black October: Nine AI Platforms Breached, and the Stolen Keys Kept Working"
domain: securite
role: reference
task: article
actors: []
dates: []
keywords: ["october", "platforms", "days", "nine", "keys", "calls", "access", "stolen"]
source: https://www.secure.com/news/black-october-stolen-ai-api-keys
source_anchor: ""
source_lines: [1, 105]
section: "Securite"
---
## TL;DR

Nine AI developer platforms disclosed security incidents in October. Attackers went after stored environment variables, especially the ones holding AI credentials. In two tracked cases, **stolen keys were validated within 30 minutes, sat quiet for days, then burned through millions of model calls.** Rotating the stored copy of a key does not stop it. **Only the provider that issued it can.**

## Introduction

Nine AI developer platforms. Twenty five days. One pattern. Attackers spent October hunting stored environment variables, and the keys they found did not get used right away. **They got tested, parked, then drained.**

## What Happened?

On Friday, October 30, cloud security researchers published a tally of the month and called it **Black October**. At least nine AI developer platforms disclosed incidents between October 6 and October 30. The researchers say public reports describe attackers explicitly hunting stored environment variables, with a focus on the ones granting AI access, and that **no single vulnerability or shared dependency links the attacks.** Three platforms reported indications of an agent swarm. That is an indication, not proof, and it is the sort of claim the agentic AI era produces faster than it produces evidence.

### Black October: nine platforms in 25 days

AI developer platforms that disclosed security incidents between October 6 and October 30, 2026. No single vulnerability or shared dependency links them.

- Oct 6Fireworks AIRoot cause tied to credential access
- Oct 9AndromedaCalled it part of an industry wide attack
- Oct 14ZeaburCredential access root cause, full write up published
- Oct 15Lightning AICredential access root cause
- Oct 16 to 17OpenCodeConsole breach may have exposed API keys
- Oct 20RailwayDetected Oct 20, disclosed Oct 21. Privilege escalation in build infrastructure
- Oct 23WarpAgent logs and telemetry exposed, vendor dependency suspected
- Oct 28CommandCodeAttributed to a distillation attempt
- Oct 30NorthflankTold customers to assume every environment variable was decrypted

**9**platforms disclosed

**25 days**Oct 6 to Oct 30

**3**reported signs of an agent swarm

Sources: Wiz, Black October for AI Developer Platforms (Oct 30, 2026); Railway preliminary report; Northflank status page.

## The Quiet Days

Here is the part worth sitting with. In two cases the researchers tracked, **each stolen key got a single test call within 30 minutes** of the reported credential access. Then almost nothing. One key saw 14 to 120 calls a day from one small client, the kind of traffic nobody investigates. Four days after validation, the attacker enumerated quotas and started bulk traffic 22 seconds later: **roughly 800,000 calls over about 30 hours**, with around 90% succeeding. The second key surfaced in a reseller pool on day seven, then ran **about 1.7 million calls in 13 hours** from a single address. Researchers say they cannot definitively establish the link between the resale operators and the original compromise. At list price, they put the two cases at **likely over $100,000 in usage.**

### Tested in 30 minutes. Drained days later.

Two stolen AI keys tracked by researchers. Both got a single test call, then sat on low traffic that nobody would think to investigate.

#### The four day wait

1. Day 0**Validated** One test call within 30 minutes of credential access
2. Days 1 to 3**Kept quiet** 14 to 120 calls a day from one small client
3. Day 4**Quotas enumerated** Bulk traffic started 22 seconds later

**~800,000 calls**over about 30 hours, around 90% succeeded

#### The reseller pool

1. Day 0**Validated** One test call within 30 minutes of credential access
2. Days 1 to 6**Kept quiet** Low daily traffic from the same small client
3. Day 7**Surfaced in a reseller pool** Bulk traffic followed from a single address

**~1.7 million calls**in about 13 hours from one address

**likely over $100,000**in usage. The link between the resale operators and the original compromise is not established.

Source: Wiz, Black October for AI Developer Platforms (Oct 30, 2026). Cases are not attributed to a named platform.

## What’s the Impact?

One platform told customers to **assume every environment variable had been decrypted** and to disable or rotate all keys. Another said about 1.85% of its workspaces, roughly 100,000, may have had variables exposed. **The bill lands on the customer, not the platform.**

## Rotating Is Not Revoking

A breach notice is day one, not the finish line, and the first steps after a breach notice decide how much this ends up costing. **The stolen key keeps answering until the provider that issued it revokes it**, so swapping the value in a platform secret store accomplishes nothing on its own. One affected platform handed potentially exposed keys to the vendors who issued them so they could be killed centrally. That is the right instinct, and it is not something a customer should assume is happening for them.

### Rotating is not revoking

Changing a secret on the breached platform and killing the key at its source are two different actions. Only one of them stops the bill.

#### Swap the value in the platform secret store

- *✓* The platform now holds a new value
- *✕* The stolen key still works at the provider
- *✕* The attacker keeps calling the model
- *✕* Usage charges keep landing on your account

Result: the leak stays open

#### Kill the key where it was issued

- *✓* The provider disables the key everywhere
- *✓* The stolen copy stops answering
- *✓* Generate a replacement, then update every store
- *✓* Check provider usage and billing logs for calls you never made

Result: the leak is closed

The stolen key keeps working until the provider that issued it revokes it.

## How to Avoid This

Most of this comes down to knowing what your team actually owns before someone else inventories it for you.

- **List** every AI key and secret stored on any platform that disclosed an incident
- **Revoke** each key with the provider that issued it, then generate a replacement
- **Review** AI provider usage and billing logs for calls your team never made
- **Turn off** public access to add ons and databases where it is switched on
- **Treat** the week after a vendor notice as live incident time, not cleanup

None of this needed a new exploit. The attackers used credentials that were already trusted, on platforms that were already approved. That is an access governance problem, not a patching one, and **it is the harder of the two to close.**
