---
id: collect-261121-veille-ia/securite/nine-ai-developer-platforms-hit-in-black-october
title: "Nine AI Developer Platforms Hit in 'Black October'"
domain: securite
role: reference
task: article
actors: []
dates: []
keywords: ["october", "keys", "platforms", "says", "stored", "users", "credentials", "developer"]
source: https://tsnmedia.org/black-october-ai-developer-platforms/
source_anchor: ""
source_lines: [1, 55]
section: "Securite"
---
If you stored AI provider keys on a hosted developer platform this month, revoke them with the provider that issued them, not just on the platform, and check your AI usage and billing for calls you cannot explain. That is the practical lesson of what security company Wiz calls Black October: “At least nine AI Developer Platforms have disclosed incidents over the course of the month,” its researchers Benjamin Read and Rami McCarthy wrote in a Wiz blog post on 30 October.

The platforms are different companies with different weaknesses, but Wiz says the intruders wanted the same thing. “All public reports highlight targeted exploitation, explicitly hunting stored environment variables, with a focus on those granting AI access,” the post says. It is careful to add that “there is no single vulnerability or dependency linking these attacks”. McCarthy said on X that the list kept growing during drafting: “two more disclosed incidents while we were writing up this blog”, he wrote, in a post Wiz’s official account reposted.

## What developers on affected platforms should do

The platforms’ own advice overlaps, and it is the clearest guide for anyone who used one of them.

1. **Revoke at the source.** Fireworks AI warns that “Removing a stored secret alone does not invalidate the underlying credential.” Zeabur tells users to revoke old keys “with the service provider first, then create new ones and redeploy”.
2. **Rotate everything stored, not only AI keys.** Zeabur lists API keys, cloud keys, database passwords, and signing and encryption keys. Lightning AI recommends all users “Rotate secrets, API keys, and revoke credentials” held in their environments, even if they were not contacted.
3. **Check billing and usage.** Northflank asks customers to review AI provider usage and billing logs, and Wiz’s cases show stolen keys running up model calls on the key owner’s account.
4. **Read the logs of connected services.** Railway asks customers to review logs “for unusual activity”; Fireworks asks affected users to check for unrecognised keys, users and jobs.
5. **Act on platform notices, then go further.** Several platforms emailed only the customers they identified as affected. Railway and Lightning AI still told everyone to rotate.

## Nine Black October incidents, each in the platform’s own words

Each company has given its own account, and they should be read separately.

- **Fireworks AI (6 October).** Its security bulletin says internal credentials were misused and environment variables, “including API keys and tokens stored as secrets”, were captured from evaluation jobs in “a limited number of customer accounts”. TSN covered the Fireworks AI incident when it was disclosed.
- **Andromeda (about 9 October).** Andromeda says it found unauthorised access to credentials on 13 October, in activity that “began as part of an industry-wide attack on or about October 9th”. It has “not yet established the root cause”.
- **Zeabur (14–15 October).** Its preliminary report says an attacker used leaked internal credentials and read environment variables in the platform database, and that “long-lived admin credentials were stored in plaintext”. Zeabur has since said it is shutting down, with all servers off by 31 December. TSN’s report on the Zeabur breach has the detail.
- **Lightning AI (15 October).** Lightning says it found unauthorised access to credentials late on 15 October and revoked all user access early the next morning. It restored read-only access to its Drive storage on 28 October.
- **OpenCode (16–17 October).** The coding-agent maker said on X that a breach “related to a large scale distillation attack” hit 1.4% of OpenCode Console users. Users later reported charges on linked AI accounts, as TSN reported on the OpenCode breach.
- **Railway (20 October).** Railway’s first disclosure said encrypted environment variables may have been accessed and some decrypted. Its preliminary report says the attack was “attempting to obtain AI provider API keys to perform distillation attacks”. See TSN’s Railway breach coverage.
- **Warp (23 October).** Chief executive Zach Lloyd said on X that the attack “appears to have come from an agent swarm”, probably through a vendor dependency, and that an “upper bound of roughly 1%” of users may have been affected. TSN covered the Warp incident.
- **CommandCode (28 October).** The coding-agent company said on X it stopped an incident “linked to a large-scale distillation attack”, revoked all API keys and has “found no evidence that any users were impacted”.
- **Northflank (30 October).** The cloud platform told all customers to assume their environment variables were decrypted, after a threat alert from Wiz; TSN’s Northflank report sets out its steps.

## What Wiz saw stolen AI keys being used for

Wiz says it tracked misuse of enterprise keys whose theft it can “attribute to one of these vendor breaches”, without saying which one. In two cases the first unauthorised use came “within 30 minutes” of the reported credential access, followed by days of quiet checks of “14–120 calls a day” to confirm the keys still worked. Then came a flood: about 800,000 calls to Amazon Bedrock on one key and about 1.7 million on the other. Wiz estimates the two cases “likely ran up over $100k in usage” at list price.

Wiz also says it linked the keys to sites reselling cheap access to AI models. “The use of the keys by multiple actors suggests a robust market for this type of credential,” it writes.

## Stolen AI keys now pay faster than the platforms can respond

The pattern in Black October is not one clever exploit. The entry points vary: plaintext admin credentials at Zeabur, build infrastructure at Railway, a vendor at Warp. What links them is the target. Platforms that hold secrets for many customers have become the quickest route to AI accounts, because one intrusion yields many keys at once. The Cloud Security Alliance made the same point on 23 October, while noting that “No vendor has publicly linked these incidents to a single actor”.

The 30-minute window in Wiz’s cases matters more than any attribution. A disclosure that comes days after the intrusion arrives long after a key has been tested and sold on. The quiet trickle of checking calls that followed is also the kind of activity a team watching its AI usage could spot before the flood.

For businesses, the risk sits with whoever pays the bill. A stolen key draws on the customer’s account, not the platform’s. Spending caps, per-project keys and alerts on unusual model use are cheap compared with a bill like the “over $100k” Wiz estimates for just two keys. The same lesson came from Allium, a blockchain data company outside Wiz’s list, which revoked existing API keys after finding unauthorised access on 25 October.

## Still unknown after Black October

None of the nine has published a full count of affected customers or keys, and most investigations are still open. Wiz has not named the two further incidents McCarthy mentioned, and it does not say which vendor’s breach produced the keys it tracked. TSN follows these incidents in its ransomware and cyber attacks hub.

The next markers are Railway’s promised follow-up once its forensic work concludes, and Zeabur’s dates for final data export ahead of its 31 December shutdown.

## Sources

Get TSN’s biggest tech stories, explained.

News and insights across AI, tech, crypto and cybersecurity. Every source linked. Unsubscribe anytime.

Get the newsletter
