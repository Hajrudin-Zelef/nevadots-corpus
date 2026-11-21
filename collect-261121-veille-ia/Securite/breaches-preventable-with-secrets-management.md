---
id: collect-261121-veille-ia/securite/breaches-preventable-with-secrets-management
title: "Breaches Preventable with Secrets Management"
domain: securite
role: reference
task: article
actors: []
dates: []
keywords: ["keys", "variables", "customers", "environment", "trigger", "northflank", "platform", "cloud"]
source: https://www.doppler.com/breach-dashboard
source_anchor: ""
source_lines: [1, 991]
section: "Securite"
---
Trigger.dev: An intrusion reached customer environment variables

BREACH DATE: November 2026

ENTRY UPDATED: November 3, 2026

SUMMARY

On November 1, 2026, Trigger.dev disclosed that an attacker had gained unauthorized access to the environment variables of a subset of organizations on its platform. The company said it had blocked the exploit used and emailed affected account owners, but it has not published a root cause or named which variables were read. Environment variables on Trigger.dev hold the credentials its jobs use to reach other services, so the exposure is really the exposure of whatever keys and tokens each customer stored there. Trigger.dev told customers to rotate affected secrets at the issuing provider.

COMPRIMISED ACCOUNTS

Undisclosed. Trigger.dev said the access affected a subset of organizations and that it had emailed impacted account owners. It did not give a count.

COMPRIMISED DATA

Customer environment variables. Trigger.dev did not name specific services, though such variables commonly hold API keys for AI providers, databases, cloud accounts, payment processors, and messaging tools. Whether variables marked as secret were handled differently is unconfirmed.

IMPACT ON END USERS

No misuse has been confirmed. The exposure lands on the connected services whose credentials sat in Trigger.dev, from databases to payment and messaging tools, because a stored key keeps working until it is rotated at the service that issued it. Changing a variable inside the platform does not revoke the underlying credential, which is why the guidance was to rotate at each provider.

Northflank: A platform breach let attackers abuse customers' AI API keys

BREACH DATE: October 2026

ENTRY UPDATED: November 3, 2026

SUMMARY

On October 30, 2026, the security firm Wiz alerted the developer platform Northflank to an intrusion, and within hours Northflank told customers to assume every environment variable they had stored had been decrypted. The company's guidance was blunt: disable and rotate all keys. The only misuse confirmed so far is unauthorized use of customers' AI API keys, though environment variables on the platform routinely hold database passwords, cloud credentials, and auth tokens alongside those keys. Northflank has not said how the attacker got in.

COMPRIMISED ACCOUNTS

Undisclosed. Northflank said only that multiple customers reported unauthorized AI API key use. Customers on dedicated or forward-deployed control planes were not affected.

COMPRIMISED DATA

AI API keys were confirmed abused. Northflank told customers to assume all stored environment variables were decrypted, which can include database passwords, cloud credentials, and authentication tokens. It did not confirm which of those were accessed.

IMPACT ON END USERS

Customers saw their AI services used without authorization, with billing impact undisclosed. Northflank patched the network path within an hour, enabled credential rotation for its managed databases, and told customers to rotate every stored secret starting with AI provider keys. When a platform holds your secrets in one encrypted store, a single decryption event becomes a full rotation job, so knowing what lives in each environment and being able to rotate it quickly is what limits the damage.

Gitee: 26,400 working cloud and AI keys sat exposed in public repositories

BREACH DATE: October 2026

ENTRY UPDATED: November 3, 2026

SUMMARY

A researcher scanning 7.25 million public repositories on Gitee, a Chinese code-hosting service, found 26,400 credentials that still worked when tested against the services that issued them. Reported on October 28, 2026, the haul was mostly cloud and AI keys, led by 9,473 Alibaba Cloud access-key pairs, 3,805 Google-issued YouTube API keys, 3,170 DeepSeek keys, and 1,348 OpenAI keys. The count included keys developers had already deleted from their current code but left behind in commit history, and one was an active organization-admin credential for an unnamed enterprise AI platform.

COMPRIMISED ACCOUNTS

Undisclosed. The keys belonged to the developers and organizations that committed them across the 7.25 million scanned repositories. By commit date, 12,242 dated to 2025, and some went back to the late 2000s.

COMPRIMISED DATA

26,400 valid credentials, mostly cloud and AI service keys, including Alibaba Cloud access keys, YouTube and Google Cloud keys, DeepSeek, Zhipu BigModel, and OpenAI keys, plus smaller numbers of GitHub and AWS keys.

IMPACT ON END USERS

Anyone who found these keys could run up charges, reach data, or disrupt API access on the owners' accounts, depending on the service. The researcher notified the most-affected vendors but not the individual developers, and it is unknown whether the keys were revoked or already used. Deleting a secret from the latest commit does not remove it from history, so a key is only safe once it has been rotated at its source.

Warp: A third-party vendor breach exposed AI agent logs and user telemetry

BREACH DATE: October 2026

ENTRY UPDATED: November 3, 2026

SUMMARY

Warp, the AI terminal, disclosed on October 26, 2026 that an unnamed third-party vendor had been compromised, exposing AI agent decision logs and user telemetry for roughly 1% of its users. Warp said the exploitation was detected on October 23. The company has not confirmed whether the exposed logs contained code, prompts, or credentials, so the sensitivity of what leaked is still open. The entry point sat in a vendor Warp relied on, outside its own systems.

COMPRIMISED ACCOUNTS

Roughly 1% of Warp users, per the company. Warp did not give an exact number.

COMPRIMISED DATA

AI agent decision logs and user telemetry from a compromised third-party vendor. Warp has not confirmed whether those logs included source code, prompts, or credentials.

IMPACT ON END USERS

No specific misuse has been reported. The incident is a reminder that data handed to a vendor carries your exposure too, and that agent logs can hold sensitive material whether or not they were meant to. Scoping what a vendor can see, and keeping secrets out of logs and traces in the first place, limits what a vendor breach can turn into.

LiteLLM: An unrotated master key let attackers hijack the release pipeline

BREACH DATE: October 2026

ENTRY UPDATED: November 3, 2026

SUMMARY

A LiteLLM Agent Platform master key was committed to a public repository on May 27, 2026. The commit was force-pushed over, but the key was never revoked. On October 10 an attacker used it to reach the platform and pull a GitHub CLI OAuth token that belonged to an organization owner and had not been scoped down. On October 21 the attacker used that access to override branch protections, point the release at a lookalike repository, and publish a backdoored LiteLLM 1.107.0 to PyPI, Docker Hub, and GHCR. The package ran a credential stealer at Python startup and beaconed to a domain registered days earlier.

COMPRIMISED ACCOUNTS

Undisclosed. Anyone who installed the unpinned 1.107.0 from PyPI or pulled the affected Docker tags during the roughly two-hour window was exposed. Enterprise customers pulling from LiteLLM's private registry were not affected.

COMPRIMISED DATA

The leaked Agent Platform master key and stolen GitHub CLI OAuth tokens, plus secrets the malware harvested from developer, cloud, and browser environments, reportedly including GitHub tokens, a GitHub App private key, and keys for AWS, OpenAI, Anthropic, and other services, along with PostgreSQL and Redis credentials.

IMPACT ON END USERS

A single credential that sat live for months became the entry point for a supply-chain compromise that could run code on any machine that merely started Python with the package installed. The malicious release was pulled within about three hours, but harvested credentials keep working until they are rotated. Two controls would have changed the outcome: revoking a secret the moment it turns up in a commit, and scoping tokens so one leaked token cannot own every repository.

Railway: A platform intrusion read customer environment variables and secrets

BREACH DATE: October 2026

ENTRY UPDATED: November 3, 2026

SUMMARY

On the morning of October 21, 2026, Railway detected unauthorized activity in the systems that store its customers' environment variables. In its preliminary report the company said the attacker escalated privileges in its build infrastructure and used compromised CI credentials to decrypt a subset of those variables, which on Railway routinely include API keys, database credentials, and private registry credentials. Railway paused all deployments, invalidated every API token, reset its storage-bucket credentials, and told customers to rotate every secret they had stored whether or not they were notified. It tied the incident to a wider campaign against hosting platforms aimed at reaching AI inference providers.

COMPRIMISED ACCOUNTS

Railway estimated that at most 1.85% of its workspaces, about 100,000, had environment variables potentially exposed, and said it would email confirmed accounts while cautioning that not receiving an email did not mean an account was safe. The platform reports roughly 2 million developers.

COMPRIMISED DATA

Customer environment variables, which Railway said may have been accessed in encrypted form and in some cases decrypted. Named examples include third-party and AI provider API keys, database credentials, and private container registry credentials.

IMPACT ON END USERS

Railway has not confirmed misuse, though one developer reportedly tied a $370,000 cryptocurrency theft to secrets stored on the platform, which Railway has not verified. Because deployments were paused during the response, customers could not immediately ship rotated credentials, which stretched out the exposure. A platform that decrypts your secrets to run your workloads can have those secrets read in a single intrusion, so treating every value stored there as rotatable, and knowing the order to rotate them in, is what shortens that window.

OpenCode Console: A breached legacy system exposed stored AI provider keys

BREACH DATE: October 2026

ENTRY UPDATED: November 3, 2026

SUMMARY

On the night of October 16, 2026, an intruder reached a legacy system inside OpenCode's hosted Console service, where workspaces had saved their third-party AI provider keys (OpenAI, Anthropic, Google). OpenCode emailed affected users the next morning under the subject "Action Required: Potential provider key breach" and said the keys may have been exposed, without confirming any were copied. Keys kept locally in the CLI and desktop apps were not part of the incident. The provider keys stored in Console were what the attacker was after, and a secret that one service holds on behalf of many is only as safe as that service's weakest system.

COMPRIMISED ACCOUNTS

OpenCode said 1.4% of Console users were affected, all of them users who had linked an outside AI provider. It did not say how many people that percentage represents.

COMPRIMISED DATA

Third-party AI provider API keys (OpenAI, Anthropic, and Google) that users had saved in Console. OpenCode did not disclose how many keys the legacy system held.

IMPACT ON END USERS

OpenCode has not confirmed misuse, though some users reported unauthorized provider charges, ranging from a few hundred dollars up to a reported $1,700. The company linked the incident to a large-scale model distillation effort and told users to revoke the affected keys, issue new ones, and set spend limits. A key that lives in a vendor's cloud needs the same rotation discipline as one in your own, because you inherit that vendor's exposure the moment you paste it in.

Lightning AI: Unauthorized credential access took the Studio platform offline

BREACH DATE: October 2026

ENTRY UPDATED: November 3, 2026

SUMMARY

Lightning AI disclosed a security incident involving unauthorized access to credentials on its platform and took the Lightning Platform fully offline while it investigated. The company described it as an active investigation and said the platform would stay inaccessible to users as its teams worked on it. Reporting placed the detection and disclosure around October 15, 2026. The scope was still being determined, and customer credentials and connected third-party keys were believed compromised.

COMPRIMISED ACCOUNTS

Undisclosed. Lightning AI did not state how many users or accounts were affected, and said the scope was still being determined.

COMPRIMISED DATA

Credentials on the Lightning Platform. Reporting indicated customer credentials and connected third-party or cloud keys were believed compromised. Lightning AI did not publish a specific list.

IMPACT ON END USERS

Taking the whole platform offline limited further access while the investigation ran, at the cost of customers' own uptime. Any third-party keys a customer had connected should be treated as exposed until rotated at the issuing service, since a key read once keeps working until it is replaced.

Zeabur: A non-expiring admin token opened the whole shared cluster

BREACH DATE: October 2026

ENTRY UPDATED: November 3, 2026

SUMMARY

Zeabur, a developer hosting platform, traced its breach to a cluster admin token created in January 2024 that had no expiry and was never rotated. The token had also been written in plaintext to a private config repository and could be used from the public internet. On October 14, 2026, an attacker signed in with it, reached the shared Kubernetes cluster across nine regions, and read more than 3.2 million environment-variable records over about a day and a half before a user reported the activity. A rotation after an earlier breach in August had missed this token and the AWS keys stored beside it.

COMPRIMISED ACCOUNTS

Undisclosed as distinct customers. The attacker read 3,218,965 environment-variable records, and eSecurity Planet reported 321,810 affected accounts. The intrusion reached 865 database containers and about 53 secrets in the secrets manager.

COMPRIMISED DATA

Customer environment variables stored on the platform, including API keys, database passwords, and signing keys. A separate path used an old GCP service-account key to gain root on 101 dedicated servers through plaintext root passwords.

IMPACT ON END USERS

Customers' stored credentials were exposed, and Zeabur announced it will shut down by the end of 2026. This was its second breach in two months, and both came back to long-lived credentials kept in plaintext where the internet could reach them. Short-lived, rotated tokens, and keeping the keys that encrypt secrets apart from the secrets they protect, are what stop one forgotten credential from reaching an entire fleet.

Andromeda: Company and customer credentials exposed in an industry-wide attack

BREACH DATE: October 2026

ENTRY UPDATED: November 3, 2026

SUMMARY

Andromeda, an AI infrastructure provider, reported unauthorized access to credentials in its environment, which exposed some of its own credentials and some customer credentials. The company said the activity began on or about October 9, 2026, was identified on October 13, and that it disabled the compromised accounts the same day. Andromeda found no evidence that customer models, datasets, or training data were reached. It linked the incident to a wider attack hitting several AI infrastructure providers at once and did not detail how access was gained.

COMPRIMISED ACCOUNTS

A limited number of customer accounts, per Andromeda, which contacted affected customers directly. The company did not give an exact number.

COMPRIMISED DATA

Certain Andromeda credentials and certain customer credentials. Andromeda reported no access to customer models, datasets, or training data within its security perimeter.

IMPACT ON END USERS

Andromeda said it saw no further unauthorized activity after October 13, rotated all affected keys, and brought in outside forensics. When the same campaign is working through a whole class of providers at once, a credential exposed at one of them can be tried everywhere it was reused, so rotating fast and keeping credentials unique to each service is what keeps one provider's incident from becoming yours.

Fireworks AI: Stolen internal credentials exposed customer secrets in eval jobs

BREACH DATE: October 2026

ENTRY UPDATED: November 3, 2026

SUMMARY

Fireworks AI traced its October 2026 incident to unauthorized use of internal Fireworks credentials. With them, a third party reached the environment variables attached to evaluation jobs in a limited number of customer accounts, and Fireworks believes those variables included API keys and tokens stored as secrets. The company found no evidence that customer models, datasets, training data, or inference traffic were touched. How the internal credentials were obtained was not explained.

COMPRIMISED ACCOUNTS

A limited number of customer accounts, per Fireworks. The company did not give an exact number.

COMPRIMISED DATA

Environment variables used in evaluation jobs, which Fireworks believes included customer API keys and tokens stored as secrets. Fireworks reported no access to customer models, datasets, training data, inference prompts or outputs, or personal information.

IMPACT ON END USERS

Fireworks detected the activity on October 6, began notifying affected customers the next day, and rotated its own employee API keys and service-account tokens over the following days, revoking keys it identified as created by the attacker. An internal credential that can read the secrets attached to customer jobs is a high-value target, so limiting what such a credential can reach, and rotating it quickly once misused, is what keeps one stolen login from turning into many stolen keys.

Double Counter: A stolen bot token hijacked a Discord security bot

BREACH DATE: October 2026

ENTRY UPDATED: November 3, 2026

SUMMARY

The attacker who hijacked Double Counter, a Discord bot used to detect alt accounts, reached its Discord token by working inward from an exposed analytics tool. They exploited that tool to get an administrator's credentials, used those to reach the company's cloud infrastructure, opened a shell in a running bot container, and pulled the bot's Discord token. That token handed them the bot's identity, which they used to post their own invites to about 50 large communities as if the messages came from Double Counter.

COMPRIMISED ACCOUNTS

Figures are company-reported and not independently verified. Double Counter cited roughly 28 million Discord user IDs and usernames, about 27 million IP addresses with coarse geolocation, around 1 million email addresses, and about 15 million VPN detection records. It said a separate cold-storage database of about 58 million users was not accessed.

COMPRIMISED DATA

Discord user IDs and usernames, IP addresses and coarse geolocation, user-agent hashes, some email addresses, and VPN detection records. Double Counter said it never held Discord passwords and that stored payment card numbers were not taken.

IMPACT ON END USERS

The attacker charged $7,316 to three cards, and two customer cards saw $18 in charges that were later refunded. The intrusion ran through a public analytics tool and an admin credential before it ever reached the bot token, so the lesson sits upstream: an exposed service and one set of admin credentials were enough to walk into the cloud account and lift a token that unlocked everything downstream.

Twitch: A malicious browser extension leaked 31,000 users' OAuth tokens

BREACH DATE: September 2026

ENTRY UPDATED: September 14, 2026

SUMMARY

A browser extension called JeetBot, marketed as ad-free, region-unlocked Twitch streaming, quietly copied each user's Twitch OAuth token and forwarded it to proxy servers run by its operators. Researchers found the token was appended in cleartext as an 'auth' URL parameter for nearly every channel a user watched, leaving it exposed in proxy request logs. The extension reached close to 31,000 users, roughly 30,000 through the Chrome Web Store and about 600 on Firefox, before the behavior was reported in September 2026. Because OAuth tokens act as bearer credentials, they granted account access with no password or two-factor code required.

COMPRIMISED ACCOUNTS

Nearly 31,000 users, about 30,000 on Chrome and 604 on Firefox, as reported by researchers.

COMPRIMISED DATA

Twitch OAuth session tokens, exfiltrated in cleartext to operator-controlled proxy servers. The tokens allowed access to chat, private whispers, and account settings.

IMPACT ON END USERS

Anyone who installed the extension handed over a live session token that could act as them on Twitch, reading private messages and changing account settings, with no password prompt or MFA in the way. A stolen bearer token works regardless of how strong the password behind it is, which is why tokens need short lifetimes, tight scopes, and fast revocation when an app looks suspicious. Affected users should remove the extension and sign out everywhere to invalidate the leaked tokens.

METR: A stolen API key ran up $600,000 in AI model credits

BREACH DATE: September 2026

ENTRY UPDATED: September 14, 2026

SUMMARY

METR, a nonprofit that evaluates AI models, disclosed that an attacker stole one of its AI provider API keys and burned through model credits valued at about $600,000 before the theft was caught. The key was exposed in March 2026 after a researcher's personal EC2 instance ran an agent orchestration dashboard with a fail-open flaw that silently disabled authentication, leaving it reachable on the open internet for several days. There were no caps on token spend, so the misuse blended into normal high-volume evaluation traffic and went unnoticed for about three weeks. METR reported no sensitive data was accessed, and the AI provider had supplied the credits for free rather than billing the nonprofit.

COMPRIMISED ACCOUNTS

Undisclosed. One METR API key for a general-access AI model account was stolen. METR did not name the AI provider.

COMPRIMISED DATA

An API key for an AI provider account, exposed through a publicly reachable agent orchestration dashboard. No sensitive or non-public data was reported compromised.

IMPACT ON END USERS

No user data was exposed, but the stolen key gave weeks of unrestricted use of a paid model account, running up credits worth about $600,000. A single exposed key with no spending caps is what let the abuse run for three weeks. Keys need to be scoped, rotated, and watched for anomalous spend, and the services that hold them should fail closed when authentication breaks.

JetBrains Cadence: An unpatched TeamCity flaw exposed AWS credentials and stored secrets

BREACH DATE: August 2026

ENTRY UPDATED: September 14, 2026

SUMMARY

Attackers broke into JetBrains' Cadence service by exploiting an unpatched TeamCity server, using CVE-2026-63077 to bypass authentication and run commands, then pulled out AWS IAM credentials and a 2024 server backup that held stored credentials, configuration, and logs. The entry point was a flaw JetBrains had told its own customers to patch, left unpatched on its own server. From there the standing AWS keys and the secrets sitting in that backup gave the attackers reach into JetBrains AWS accounts and S3 storage. JetBrains invalidated the affected credentials and told users to revoke or rotate every secret used in Cadence executions.

COMPRIMISED ACCOUNTS

The same group of Cadence users JetBrains had previously contacted; JetBrains reported no additional affected users identified. Exact numbers were not disclosed.

COMPRIMISED DATA

AWS IAM credentials and secrets used with Cadence, a full 2024 Cadence server backup containing credentials, configuration, artifacts, and logs, source code from synchronized PyCharm projects, and user details such as names, emails, login timestamps, and IP addresses.

IMPACT ON END USERS

Affected users had AWS credentials and any secrets stored in their Cadence executions exposed, which is why JetBrains told them to rotate everything. The break-in came through an unpatched flaw. What turned it into a credential leak was long-lived AWS keys and a backup full of stored secrets sitting where the attacker could reach them. Short-lived credentials, regular rotation, and keeping secrets out of backups and logs are what limit how far this kind of access spreads.

Exposed .git directories: 28,000 misconfigured repos leaked live API keys

BREACH DATE: August 2026

ENTRY UPDATED: August 31, 2026

SUMMARY

Misconfigured web servers left .git directories publicly readable, and anyone who found them could walk the full commit history, including files and secrets that had been deleted from current code. Security firm Intruder scanned about 3.5 million hosts and found 28,000 exposed repositories. Inside them were more than 400 AWS access keys, 123 OpenAI API keys, 107 Stripe keys, 80 Telegram bot tokens, and 17 GitHub personal access tokens, several of which were still valid when tested. Deleting a secret from the latest commit does nothing if the old commit is still served to the public web.

COMPRIMISED ACCOUNTS

Undisclosed number of end users. 28,000 exposed repositories were found across roughly 3.5 million scanned hosts, spanning organizations of many sizes.

COMPRIMISED DATA

Secrets recovered from public .git history, including over 400 AWS access keys, 123 OpenAI API keys, 107 Stripe keys, 80 Telegram bot tokens, and 17 GitHub personal access tokens, alongside source code and internal files. Some keys were confirmed active during testing.

IMPACT ON END USERS

Active AWS and payment keys gave direct access to cloud accounts and customer data, and exposed keys can be used before anyone notices the repo is open. A leaked secret lives in git history, not just the working tree, so it has to be revoked and rotated rather than quietly removed from the latest commit, and .git directories should never be reachable from the public web.

Stripe merchants: 650 live secret keys leaked, exposing 688,000 customer records

BREACH DATE: August 2026

ENTRY UPDATED: August 31, 2026

SUMMARY

Live Stripe secret keys were the loot. A user on a data-trading forum posted roughly 35 GB of payment data tied to 659 Stripe merchant accounts, and researchers who analyzed it found 650 live secret keys beginning with the sk_live prefix, plus nine restricted keys. Stripe's own platform was not breached; the keys were taken from the merchant side and used to pull data through normal API calls. The exposed records covered customers, charges, payouts, invoices, and subscriptions from January 2022 through June 2026, though full card numbers were not included.

COMPRIMISED ACCOUNTS

659 Stripe merchant accounts and roughly 688,000 customer records across 42 countries. The United States, United Kingdom, and France had the most affected merchants.

COMPRIMISED DATA

650 live Stripe secret keys (sk_live) and nine restricted keys, along with customer records, charges, payment intents, checkout sessions, invoices, payouts, balance transactions, refunds, disputes, and subscriptions. Full payment card numbers were reportedly not present.

IMPACT ON END USERS

A live secret key is a bearer credential, so whoever holds it can act as the merchant. Researchers reported that 573 of the accounts could accept payments and 531 could send payouts, which opens the door to fraud and data extraction through legitimate-looking API requests. Secret keys belong in a managed secrets store, scoped to the minimum needed and rotated the moment they are exposed, so a single leaked key cannot stand in for the whole account.

n8n: Leaked automation tokens open live instances and their secrets

BREACH DATE: August 2026

ENTRY UPDATED: August 14, 2026

SUMMARY

GitGuardian reported in August 2026 that it had found 4,576 unique n8n API tokens sitting in public GitHub commits, tied to more than 1,200 hostnames. When researchers tested them, 896 instances were reachable and 321 still accepted a leaked token. Because n8n wires together the systems it automates, one working token exposed workflow definitions and the credentials stored inside them, including OpenAI keys and database passwords, all through normal API calls with no vulnerability involved. The set also included hundreds of Model Context Protocol tokens, a handful still valid.

COMPRIMISED ACCOUNTS

4,576 leaked tokens across more than 1,200 hostnames. Of 896 reachable instances, 321 still accepted a leaked token.

COMPRIMISED DATA

Valid n8n API tokens, and some MCP tokens, granting access to workflow definitions and the credentials stored in them, including OpenAI keys, database passwords, and deployment keys.

IMPACT ON END USERS

A single leaked automation token was a skeleton key to every system wired into that platform, reachable through standard API calls. Automation and agent platforms concentrate secrets, so their API tokens are high-value keys that belong out of git and on an aggressive rotation schedule.

Hugging Face: An AI agent breaks in and steals internal credentials

BREACH DATE: July 2026

ENTRY UPDATED: August 14, 2026

SUMMARY

Hugging Face disclosed in July 2026 that a malicious dataset uploaded to its platform exploited a vulnerability to run code on its servers. It attributed the activity to an external AI agent that carried out thousands of actions across short-lived sandboxes, using stolen and forged machine credentials to move deeper into internal systems. The intruder reached Kubernetes admin access and gained write access to a subset of internal source-code repositories. Hugging Face revoked and rotated the stolen credentials, fixed the vulnerability, and urged all users to rotate their access tokens.

COMPRIMISED ACCOUNTS

Undisclosed. Hugging Face urged all users to rotate access tokens, and its investigation into customer and partner data was ongoing.

COMPRIMISED DATA

Internal service credentials and forged identity tokens, internal datasets, and write access to a subset of internal source-code repositories.

IMPACT ON END USERS

Users had to rotate their Hugging Face tokens and review account activity because stolen and forged machine credentials let the intruder move laterally into internal systems. Short-lived, tightly scoped machine identities and fast rotation are the main defense once an attacker is running thousands of actions inside your environment.

Klue: Stolen OAuth tokens reach customers' Salesforce data

BREACH DATE: June 2026

ENTRY UPDATED: August 14, 2026

SUMMARY

An extortion actor calling itself Icarus breached Klue, a competitive-intelligence vendor, in June 2026 by abusing an OAuth integration credential to steal the tokens customers used to connect Klue to their own systems. With those tokens the attacker reached connected SaaS platforms like Salesforce and Gong and pulled business data. Nearly 200 companies were named in the actor's claims, and Huntress, Recorded Future, Tanium, and Jamf confirmed they were affected. The stolen data was limited to CRM and business records; Huntress said no products, infrastructure, passwords, or payment data were touched.

COMPRIMISED ACCOUNTS

Nearly 200 companies were named in the actor's claims. Huntress, Recorded Future, Tanium, and Jamf confirmed impact.

COMPRIMISED DATA

Business contact details, sales communications, pricing, and trial data pulled from customers' Salesforce and Gong systems using stolen OAuth tokens.

IMPACT ON END USERS

Customers had CRM data taken through an integration they had authorized, without their own logins ever being phished. OAuth tokens granted to third-party apps are standing keys to your data, so keep them least-privilege, short-lived, and revocable.

Nx Console: A poisoned VS Code extension scrapes developer secrets

BREACH DATE: May 2026

ENTRY UPDATED: August 14, 2026

SUMMARY

Attackers published a malicious version of the Nx Console extension, 18.95.0, to the VS Code Marketplace on May 18, 2026, using a stolen contributor token. The extension has millions of installs, and simply opening a workspace with the poisoned version silently harvested a developer's credentials. The payload swept up GitHub and npm tokens, AWS credentials, HashiCorp Vault and Kubernetes tokens, 1Password data, SSH keys, and the contents of .env files, and on macOS it installed a persistent backdoor. The malicious version was live only briefly, but developer machines that upgraded during the window had their full credential set exposed.

COMPRIMISED ACCOUNTS

The extension had more than 2.2 million installs. Actual theft was limited to developers who upgraded during the short window and then opened a workspace.

COMPRIMISED DATA

GitHub and npm tokens, AWS credentials, HashiCorp Vault and Kubernetes tokens, 1Password data, SSH keys, and .env file contents from developer machines.

IMPACT ON END USERS

Opening a workspace was enough to exfiltrate a developer's entire credential set, and on macOS it left a persistent backdoor. Editor extensions are part of your supply-chain attack surface, so scope developer tokens tightly and treat local dev secrets as targets.

CISA: A public GitHub repo full of production secrets

BREACH DATE: May 2026

ENTRY UPDATED: August 14, 2026

SUMMARY

A GitGuardian researcher found a publicly accessible GitHub repository named Private-CISA holding roughly 844MB of production infrastructure secrets belonging to the U.S. cyber agency. It contained plaintext passwords, AWS credentials, Azure keys, GitHub personal access tokens, Artifactory tokens, and SAML certificates, some in files named things like Important AWS Tokens.txt. The repository had reportedly been exposed for about six months. After disclosure on May 14, 2026, CISA took it down within about a day, and said it had no indication the data was misused.

COMPRIMISED ACCOUNTS

Roughly 844MB of one agency's production infrastructure secrets were public for a reported six months.

COMPRIMISED DATA

Plaintext passwords, AWS credentials, Azure registry keys, GitHub personal access tokens, Artifactory tokens, SAML certificates, and Kubernetes and Terraform configs.

IMPACT ON END USERS

Anyone who found the repo had a map and the keys to production infrastructure. Hardcoded, long-lived secrets committed to a repo are a breach waiting to happen, and private by name is not access control; secrets belong in a managed store, never in source.

Vercel: An OAuth grant to a third-party AI tool leads into customer env vars

BREACH DATE: April 2026

ENTRY UPDATED: August 14, 2026

SUMMARY

Vercel disclosed in April 2026 that attackers had compromised Context.ai, a third-party AI tool one of its employees used, and abused its Google Workspace OAuth access to take over that employee's account. From there they pivoted into Vercel's internal systems and reached the environment variables of a limited subset of customers. Vercel characterized the exposed variables as non-sensitive and said its published npm packages were not affected. The company framed the incident around OAuth sprawl and the risk of broadly scoped grants to shadow AI tools.

COMPRIMISED ACCOUNTS

Undisclosed. Vercel described a limited subset of customers plus a small number of additional accounts.

COMPRIMISED DATA

Environment variables belonging to a limited subset of customers, which Vercel characterized as non-sensitive. Its npm packages were not affected.

IMPACT ON END USERS

The entry point was a single broadly scoped OAuth grant to a third-party AI tool, which became a path into internal systems and a customer secret store. Environment-variable secrets need short lifetimes, and OAuth grants to outside tools need tight scoping and review.

LiteLLM: Poisoned AI gateway package harvests cloud and API keys

BREACH DATE: March 2026

ENTRY UPDATED: August 14, 2026

SUMMARY

Attackers pushed two backdoored versions of LiteLLM, 1.82.7 and 1.82.8, to PyPI on March 24, 2026. LiteLLM sits in front of many AI stacks as a gateway and pulls in millions of downloads a day, so a tainted release could reach a lot of pipelines fast. The malicious code ran a credential stealer against CI/CD environments, sweeping up cloud credentials, SSH keys, Kubernetes tokens, .env files, and the AI provider API keys the gateway exists to route. The packages were pulled quickly, but stolen credentials keep working until they are rotated, and a large trove of the harvested secrets later surfaced publicly.

COMPRIMISED ACCOUNTS

Undisclosed and not independently confirmed. Reporting cited around 2,500 exposed organizations, and BleepingComputer's sources put the number of credential exfiltrations near 500,000.

COMPRIMISED DATA

Cloud credentials (AWS, GCP, Azure), SSH keys, Kubernetes tokens, CI/CD secrets, .env contents, and AI provider API keys pulled from build environments.

IMPACT ON END USERS

Any team that installed a poisoned version had live secrets swept out of its pipelines, and some of those credentials still worked months later. A single compromised upstream package can turn a trusted AI dependency into an exfiltration tool, so pin and verify dependency versions and rotate pipeline and gateway secrets after any supply-chain event.

CarGurus confirmed a cybersecurity incident involving unauthorized access to an internal company database. According to the company, the breach exposed limited dealership contact and internal business data, highlighting the risks of credential compromise and weak access workflows. The incident follows broader reporting tying the attack to social engineering and credential-focused intrusion methods used by the ShinyHunters group. CarGurus said the event was contained, services remained operational, and affected parties were directly notified where sensitive information may have been involved.

COMPRIMISED ACCOUNTS

Undisclosed

COMPRIMISED DATA

Internal company database, employee authentication workflows

IMPACT ON END USERS

Attackers reportedly gained access through social engineering techniques targeting employee authentication workflows. Centralized access controls, phishing-resistant authentication, and tighter credential management help reduce the blast radius when employee accounts are compromised.

Moltbook: A hardcoded key exposes 1.5 million AI agent tokens

BREACH DATE: February 2026

ENTRY UPDATED: August 14, 2026

SUMMARY

Wiz found that Moltbook, a social network for AI agents, had left its Supabase database open through a hardcoded API key in client-side JavaScript with no row-level security behind it. Anyone could read and write the production database. The exposure included about 1.5 million agent API tokens, more than 35,000 user emails, and roughly 4,060 private messages, some of which held users' own OpenAI keys, adding up to around 4.75 million records. The issue was reported on January 31, 2026 and patched within a day.

COMPRIMISED ACCOUNTS

More than 35,000 user emails and roughly 1.5 million agent accounts owned by about 17,000 people, around 4.75 million records in total.

COMPRIMISED DATA

About 1.5 million agent API tokens, more than 35,000 user emails, and roughly 4,060 private messages, some containing third-party OpenAI API keys.

IMPACT ON END USERS

Anyone who looked could impersonate any agent, read private messages, and lift the third-party keys inside them, with full write access on top. Never ship a privileged API key in client-side JavaScript, and never treat a database as private without row-level access controls.

Security researchers uncovered over 175,000 publicly exposed Ollama AI servers across ~130 countries, many misconfigured without authentication despite running local LLM infrastructure. Nearly half allow tool-calling, expanding the attack surface, and are now being actively exploited for abuse like resource hijacking and LLMjacking.

COMPRIMISED ACCOUNTS

The compromise stems from misconfiguration of AI servers.

COMPRIMISED DATA

No specific personal or proprietary data was reported as directly exfiltrated.

IMPACT ON END USERS

Exposed AI hosts can be hijacked for spam, malware, or cryptocurrency mining at the owner’s expense. Tool-enabled endpoints further expand the attack surface by allowing command execution and access to external systems, while unsecured hosts risk being used for harmful activity, creating potential legal and reputational fallout.

Salesloft Drift: Stolen OAuth tokens drain Salesforce data

BREACH DATE: August 2025

ENTRY UPDATED: August 12, 2026

SUMMARY

A threat actor tracked by Google as UNC6395 used OAuth tokens stolen from the Salesloft Drift integration to reach the Salesforce instances of hundreds of organizations and pull data at scale. The attacker then combed the stolen records for more secrets, specifically AWS access keys, Snowflake tokens, and passwords, to enable further compromise, and deleted its query jobs to cover its tracks. Salesloft, Salesforce, and Google revoked all active Drift tokens, and a follow-up disclosure widened the scope to Drift Email tokens tied to Google Workspace. Confirmed victims included security vendors such as Cloudflare, Palo Alto Networks, and Zscaler.

COMPRIMISED ACCOUNTS

Google's initial assessment referenced more than 700 potentially affected organizations. Confirmed victims included Cloudflare, Palo Alto Networks, and Zscaler.

COMPRIMISED DATA

Salesforce records including accounts, contacts, cases, and opportunities, reached with stolen OAuth access and refresh tokens.

IMPACT ON END USERS

Organizations that never touched the attacker still lost sensitive CRM and support data because one trusted integration's tokens were stolen, and the attacker mined that data for still more secrets. OAuth tokens are standing credentials that skip MFA, so third-party app tokens need least-privilege scoping, monitoring, and fast revocation.

Amazon Q Developer extension: Malicious code slips into an AI dev tool

BREACH DATE: July 2025

ENTRY UPDATED: August 12, 2026

SUMMARY

An attacker submitted a pull request to the open-source repository behind Amazon's Q Developer extension for VS Code and gained enough repository access to commit a malicious prompt. The prompt, bundled into version 1.84.0 and published to the marketplace, instructed the AI agent to wipe local files and delete AWS resources through the CLI. The code reached an extension with more than 964,000 installs but failed to run because of a formatting error, and AWS confirmed no customer resources were affected. AWS revoked the exposed credentials, pulled the code, and shipped a clean version about a week later.

COMPRIMISED ACCOUNTS

Undisclosed. The affected extension had been installed more than 964,000 times.

COMPRIMISED DATA

No data was exfiltrated. An attacker used repository access to inject a destructive prompt into a widely distributed AI coding extension.

IMPACT ON END USERS

Had the payload executed, it could have deleted developers' local files and torn down AWS resources, but the formatting error meant no environments were harmed. AI coding assistants run with real privileges, so the repository tokens and credentials behind them need least-privilege scoping and tight review.

xAI: Private API key on GitHub exposed internal LLMs

BREACH DATE: May 2025

ENTRY UPDATED: August 12, 2026

SUMMARY

A security researcher flagged that an xAI employee had committed a private x.ai API key to a public GitHub repository. GitGuardian's scanners caught the key and alerted the employee in early March 2025, but it stayed valid until xAI's security team was told at the end of April, roughly two months later. During that window the key granted access to at least 60 private and unreleased large language models, including development versions of Grok reportedly tuned on internal SpaceX, Tesla, and X data. No misuse was confirmed, but a single hardcoded credential sat live long after a scanner had already found it.

COMPRIMISED ACCOUNTS

Undisclosed. One employee's key granted access to at least 60 private and fine-tuned language models.

COMPRIMISED DATA

A private x.ai API key that unlocked internal and unreleased large language models, some reportedly tuned on SpaceX, Tesla, and X data.

IMPACT ON END USERS

No customer data breach was confirmed, but the key gave two months of standing access to proprietary models that could have been copied, abused, or probed. Detection is only half the job; a leaked key has to be revoked the moment it is found, which is what scanning paired with fast rotation is for.

A supply chain attack compromised the GitHub Actions tool 'tj-actions/changed-files', redirecting version tags to malicious code. Organizations have been advised to immediately rotate credentials as a precautionary measure.

COMPRIMISED ACCOUNTS

23,000 organizations

COMPRIMISED DATA

Credentials such as AWS access keys, GitHub Personal Access Tokens (PATs), npm tokens, and private RSA keys.

IMPACT ON END USERS

Potential unauthorized access to their sensitive data.

DeepSeek: Exposed database spilling API keys and chat logs

BREACH DATE: January 2025

ENTRY UPDATED: August 12, 2026

SUMMARY

Wiz Research found a DeepSeek ClickHouse database sitting on the open internet with no authentication in front of it. It held more than a million log lines, including plaintext chat history, API keys, and backend service details, and the open HTTP interface let anyone run arbitrary SQL against it. DeepSeek locked it down after Wiz reported the exposure, but for a window the keys and conversations were readable by anyone who found the port. Shipping an AI product quickly does not remove the basics: a database holding secrets should never be reachable without authentication.

COMPRIMISED ACCOUNTS

Undisclosed. The exposed log stream alone held more than a million entries.

COMPRIMISED DATA

A publicly reachable ClickHouse database with no authentication, exposing plaintext chat history, API keys, and backend operational metadata.

IMPACT ON END USERS

Anyone who located the open ports could read live user conversations and pull the API keys stored alongside them, and could have escalated to broader control of DeepSeek's systems. Keeping credentials out of logs and behind authenticated, access-controlled storage is what stops a single misconfiguration from turning into full exposure.

Postman: Tens of thousands of secrets left in public workspaces

BREACH DATE: December 2024

ENTRY UPDATED: August 12, 2026

SUMMARY

Research by CloudSEK found more than 30,000 public Postman workspaces leaking sensitive data. Developers had hardcoded secrets directly into public API collections and environments, including access and refresh tokens, third-party API keys, credentials, and session secrets. The exposure spanned major services, with thousands of leaked instances each for GitHub, Slack, and Salesforce. This was not one company being breached; it was systemic, self-inflicted exposure caused by workspaces left public and secrets left in the clear.

COMPRIMISED ACCOUNTS

More than 30,000 public workspaces, belonging to organizations across healthcare, finance, SaaS, and consumer sectors.

COMPRIMISED DATA

Hardcoded access and refresh tokens, third-party API keys, credentials, and session secrets embedded in public API collections.

IMPACT ON END USERS

Any live key could let an attacker impersonate an app, drain a paid API account, or pivot into a connected system, often without the owning team realizing a testing tool had become the leak. API testing platforms are a real secrets-sprawl surface, so keep workspaces private, use environment variables and a vault instead of hardcoding, and scan public collections for keys.

U.S. Treasury (BeyondTrust): Exposed support API key attack

BREACH DATE: December 2024

ENTRY UPDATED: March 10, 2025

SUMMARY

In late 2024, a Chinese state-sponsored hacking group (APT) carried out a supply-chain attack by exploiting a stolen API key from BeyondTrust’s Remote Support service. BeyondTrust is a vendor that provides remote tech support tools to organizations, including the U.S. Treasury Department. On December 8, 2024, BeyondTrust notified the Treasury that attackers had obtained an API key used to secure its cloud-based remote support portal for Treasury Department employees. With this key, the threat actor was able to bypass security controls in the support service, effectively gaining remote access to certain Treasury user workstations and the data on them.

COMPRIMISED ACCOUNTS

Treasury Department employees affected

COMPRIMISED DATA

Unclassified documents maintained by Treasury Departmental Offices users. Sensitive law enforcement information. Materials related to investigations conducted by the Committee on Foreign Investment in the U.S.

IMPACT ON END USERS

For the U.S. Treasury, the incident was a serious security breach: attackers remotely accessed employee computers, which could undermine trust in IT support channels and potentially expose sensitive information.

Internet Archive: Unrotated access token exposes support tickets

BREACH DATE: October 2024

ENTRY UPDATED: August 12, 2026

SUMMARY

An attacker accessed the Internet Archive's Zendesk support system using a stolen API token and sent messages warning that data had been exposed. The token was reachable because an exposed GitLab configuration file on a development server had held authentication credentials since at least December 2022. The token had access to more than 800,000 support tickets going back to 2018, some containing identity documents that users had uploaded to request page removals. The organization had reportedly not rotated the exposed credentials despite earlier warnings, and this was its third security incident that month.

COMPRIMISED ACCOUNTS

Undisclosed. The stolen token had access to more than 800,000 support tickets dating back to 2018.

COMPRIMISED DATA

Support tickets including personal identity documents, reached through an API token exposed in a GitLab configuration file.

IMPACT ON END USERS

Anyone who had ever emailed support, including people who submitted ID to request page removals, risked exposure of their messages and documents. A leaked credential is only half the failure; leaving it unrotated after being warned is what turned a known risk into a breach.

Slack, the workplace messaging platform, experienced a security incident over the 2022 holiday period when a limited number of employee tokens were stolen and misused to access the company’s externally hosted GitHub code repositories. On December 27, 2022, a threat actor used these leaked credentials to download some private Slack source code. Fortunately, no customer data or Slack’s primary codebase was accessed, and the breach did not stem from a vulnerability in Slack itself. Slack quickly invalidated the compromised tokens and rotated other secrets as a precaution.

The New York Times: Source code stolen with an exposed GitHub token

BREACH DATE: June 2024

ENTRY UPDATED: August 12, 2026

SUMMARY

An attacker used an exposed GitHub authentication token to reach the New York Times' repositories and download internal data. The Times confirmed a credential to a third-party code platform had been inadvertently made available, and said there was no sign of access to its own internal systems. In June 2024 the archive surfaced publicly, around 273GB across roughly 5,000 repositories, including source code and internal tooling. A note posted with the leak said the exposed token was how the data was taken.

COMPRIMISED ACCOUNTS

Undisclosed. No reader or subscriber accounts were reported compromised.

COMPRIMISED DATA

Roughly 273GB of internal data across about 5,000 repositories, including source code and IT documentation, reached through an exposed GitHub token.

IMPACT ON END USERS

The damage landed on internal intellectual property and tooling rather than reader accounts, though leaked infrastructure code and any embedded secrets create follow-on risk. A single over-privileged, long-lived repo token can expose an entire codebase, so source-platform tokens should be scoped tightly, kept short, and watched.

Hugging Face: Stolen auth tokens from the Spaces platform

BREACH DATE: June 2024

ENTRY UPDATED: August 12, 2026

SUMMARY

Hugging Face detected unauthorized access to its Spaces platform and disclosed that a subset of Spaces secrets, meaning the authentication tokens and API keys developers store for their hosted AI apps, could have been accessed. The company revoked affected tokens, emailed impacted users, and pushed everyone toward fine-grained access tokens. In response it rolled out a key management service for Spaces secrets, removed org-wide tokens to improve traceability, and improved detection of leaked credentials. The remediation reads like a secrets-hygiene checklist because the stolen assets were the credentials themselves.

COMPRIMISED ACCOUNTS

Undisclosed. A subset of Spaces secrets could have been accessed.

COMPRIMISED DATA

Authentication tokens and API keys that developers stored so their hosted AI apps could reach other services.

IMPACT ON END USERS

Exposed tokens could let an attacker impersonate users, reach private models and datasets, or push malicious changes, so affected users had to rotate everything the tokens protected. Fine-grained, short-lived, rotatable tokens limit how much a single stolen credential can reach.

Snowflake customer breaches: Stolen logins with no MFA

BREACH DATE: May 2024

ENTRY UPDATED: August 12, 2026

SUMMARY

Through 2024 a financially motivated group tracked as UNC5537 broke into a string of Snowflake customer accounts using logins that infostealer malware had swiped from employees, some of them years earlier. Mandiant found the abused accounts shared three gaps: no multi-factor authentication, credentials that were never rotated, and no network allow-listing. The platform itself was not breached; the accounts were. Roughly 165 organizations were flagged as potentially exposed, and victims including Ticketmaster, AT&T, and Santander were later extorted or had data leaked.

COMPRIMISED ACCOUNTS

Around 165 organizations were notified as potentially exposed. Downstream victims included Ticketmaster, AT&T, and Santander.

COMPRIMISED DATA

Customer data held in Snowflake cloud instances, including personal, contact, and transaction records, exfiltrated with valid stolen logins.

IMPACT ON END USERS

Hundreds of millions of people had personal and transactional data exposed and monetized through extortion, feeding downstream fraud and phishing. A password stolen from a laptop years ago should not still open a data warehouse, which is why MFA, rotation, and allow-listing matter on every account.

Sisense: CISA warns customers to rotate every credential

BREACH DATE: April 2024

ENTRY UPDATED: August 12, 2026

SUMMARY

CISA issued an advisory urging every Sisense customer to reset any credential or secret they had ever shared with the analytics vendor, an unusually broad warning that signaled real supply-chain risk. Reporting by Krebs on Security traced the breach to a Sisense-hosted GitLab repository that held credentials to the company's Amazon S3 storage. From there, attackers reportedly pulled terabytes of data, including millions of access tokens, email passwords, and SSL certificates. Because Sisense plugs into its customers' own data systems, the exposed secrets reached well beyond Sisense itself.

COMPRIMISED ACCOUNTS

Undisclosed. Sisense serves more than a thousand customers across finance, healthcare, telecom, and other sectors.

COMPRIMISED DATA

Reportedly millions of access tokens, email passwords, and SSL certificates, reached through hardcoded credentials in a Git repository.

IMPACT ON END USERS

Any credential a customer had shared with Sisense had to be treated as compromised and rotated, from Active Directory and Git to SSO secrets, database logins, and API keys. Secrets that cross a vendor boundary need an inventory, short lifetimes, and a fast way to revoke them.

Dropbox Sign (HelloSign): Service account breach exposing API keys

BREACH DATE: April 2024

ENTRY UPDATED: January 1, 2025

SUMMARY

Dropbox disclosed that its electronic signature subsidiary, Dropbox Sign (formerly HelloSign), was breached in April 2024. Attackers compromised a service account in the Dropbox Sign production environment, which is a non-human account used for automated backend tasks. Using this account’s credentials, the intruders gained admin-level access to Dropbox Sign’s production systems, including its customer database. As a result, they were able to access a variety of user data and secrets. Specifically, the attackers retrieved Dropbox Sign customer account information, including authentication data such as API keys, OAuth access tokens, and MFA setup info.

COMPRIMISED ACCOUNTS

All Dropbox Sign (formerly HelloSign) customers

COMPRIMISED DATA

Customer information like email addresses, usernames, phone numbers (in some cases), hashed passwords, and important authentication details like API keys and OAuth tokens were accessed.

IMPACT ON END USERS

Users were required to take multiple security actions. Dropbox invalidated all exposed passwords and session tokens, forcing password resets for affected accounts. They also restricted the functionality of exposed API keys and OAuth tokens until customers could rotate them.

A third-party breach exposed API keys used by OpenSea, a major NFT marketplace. The breach highlighted the cyber risks posed by external service providers and the importance of securing API keys.

COMPRIMISED ACCOUNTS

Unknown.

COMPRIMISED DATA

API keys with various permissions, potentially affecting account management and authentication processes.

IMPACT ON END USERS

The exposure of API keys could lead to unauthorized transactions and account manipulations. OpenSea initiated a rotation of API keys and urged users to secure their accounts.

Microsoft: Password spray and OAuth abuse reach corporate email and secrets

BREACH DATE: January 2024

ENTRY UPDATED: August 12, 2026

SUMMARY

The Russian state-sponsored group Midnight Blizzard, also tracked as APT29, ran a slow password-spray attack against a legacy test account that had no multi-factor authentication. From there it abused a legacy OAuth application with high permissions, created more malicious OAuth apps, and granted itself access to read corporate mailboxes. A later update revealed the attacker used information from those emails to reach source-code repositories, and accessed secrets that customers had shared with Microsoft over email. Microsoft found no evidence its customer-facing production systems were compromised.

COMPRIMISED ACCOUNTS

Undisclosed. Microsoft described access to a very small percentage of corporate email accounts, including senior leadership.

COMPRIMISED DATA

Corporate email and attached documents, some source-code repositories, and credentials that customers had shared with Microsoft in email.

IMPACT ON END USERS

Customers whose secrets were sitting in emails to Microsoft support faced exposure of those very credentials and had to rotate them. Enforce MFA on every account including legacy and test ones, retire dormant accounts and over-privileged OAuth apps, and keep plaintext secrets out of email and ticketing systems.

Cloudflare: Unrotated Okta tokens open a path to internal systems

BREACH DATE: November 2023

ENTRY UPDATED: August 12, 2026

SUMMARY

Attackers used one access token and three service-account credentials that had been stolen in Okta's October 2023 support breach and that Cloudflare had not rotated. Starting in mid-November, they reached Cloudflare's self-hosted Atlassian environment, established persistent access around Thanksgiving, and browsed Confluence, Jira, and Bitbucket, viewing some source code before being evicted. Cloudflare rotated more than 5,000 production credentials, triaged nearly 4,900 systems, and reimaged its Atlassian servers. The company said no customer data or systems were affected.

COMPRIMISED ACCOUNTS

Cloudflare itself. No customer data, systems, or network configuration were affected.

COMPRIMISED DATA

Internal Atlassian tools (Confluence, Jira, Bitbucket) and a limited amount of source code, reached with one access token and three service-account credentials.

IMPACT ON END USERS

No customers were harmed, but the intrusion succeeded only because credentials leaked in an upstream vendor breach were never rotated. Credentials exposed anywhere in the supply chain should be treated as compromised and rotated right away.

Okta: Support system compromise via exposed credentials

BREACH DATE: October 2023

ENTRY UPDATED: January 23, 2024

SUMMARY

Identity provider Okta revealed that a threat actor gained unauthorized access to its customer support case management system by exploiting leaked credentials for a support service account. An Okta employee had inadvertently saved the support account’s username and password in their personal Google Chrome profile, which was likely compromised, exposing the credentials. Between late September and October 17, 2023, the attacker used these credentials to access files from support tickets of 134 Okta customers. Some of these files included HTTP Archive (HAR) logs containing session cookies/session tokens.

COMPRIMISED ACCOUNTS

134 customers

COMPRIMISED DATA

The hacker used stolen session tokens to hijack five customers’ active Okta sessions, impersonating those users in their own Okta orgs.

IMPACT ON END USERS

For the five affected customer organizations, the attacker’s use of session tokens could have led to unauthorized access to those organizations’ Okta tenant data. Okta worked with those customers to revoke compromised sessions and cookies. Beyond those cases, other customers whose support files were accessed were advised to review what was in those uploads (e.g., scrub HAR logs) and to monitor for any misuse.

Microsoft (AI research data leak): Exposed Azure storage key

BREACH DATE: September 2023

ENTRY UPDATED: December 12, 2023

SUMMARY

Researchers from Wiz discovered that Microsoft’s AI research team had accidentally exposed an overly-permissive Azure Shared Access Signature (SAS) token in a GitHub repository. This single leaked storage access key granted full control over an entire Azure Storage account, exposing 38 TB of private data that Microsoft did not intend to share. The trove included a disk backup of two employees’ workstations containing sensitive secrets, private keys, passwords, and over 30,000 internal Microsoft Teams messages from 359 Microsoft employees. The leak was an internal exposure (not a result of an outside attack) caused by a misconfigured token that allowed access beyond the specific AI training data it was meant to share. Microsoft was alerted to the issue on June 22, 2023, and promptly revoked the SAS token to secure the data.

COMPRIMISED ACCOUNTS

Limited / Internal exposure

COMPRIMISED DATA

38 TB of private data that Microsoft did not intend to share.

CircleCI, a popular CI/CD platform, suffered a major breach caused by an infostealer malware on an engineer’s laptop. The malware stole a 2FA-backed SSO session cookie, allowing the attacker to impersonate the employee and access CircleCI’s internal production systems. Using this access, the hacker generated new tokens and exfiltrated sensitive data from some of CircleCI’s databases, including customers’ environment variables, API keys, and tokens. Although the stolen data was encrypted at rest, the attacker even grabbed encryption keys from memory to decrypt the stolen secrets. CircleCI disclosed the incident on January 4, 2023, urging all customers to rotate any secrets stored in the platform.

COMPRIMISED ACCOUNTS

Undisclosed / Potentially All

COMPRIMISED DATA

Customers’ security was at risk because the stolen API keys and tokens could be used to access those customers’ systems.

IMPACT ON END USERS

CircleCI required users to rotate all secrets (SSH keys, API tokens, OAuth tokens, etc.) that were in CircleCI. "Fewer than 5” customers reported unauthorized access to their systems as a result of the stolen tokens.

Toyota: A hardcoded access key left public on GitHub for five years

BREACH DATE: October 2022

ENTRY UPDATED: August 12, 2026

SUMMARY

Toyota disclosed that part of its T-Connect website source code had been posted to a public GitHub repository by a subcontractor, where it stayed accessible for nearly five years. The source code contained a hardcoded access key to a data server holding customer records. Up to 296,019 customers may have had their email addresses and management numbers exposed. Toyota restricted the repository and rotated the key in September 2022, and said it could not confirm or rule out that anyone else had accessed the data.

COMPRIMISED ACCOUNTS

Up to 296,019 T-Connect customers registered between December 2017 and September 2022.

COMPRIMISED DATA

Customer email addresses and management numbers, exposed through a hardcoded server access key committed to public source code.

IMPACT ON END USERS

Affected drivers faced a higher risk of phishing and targeted spam, and Toyota could not say for certain whether anyone had used the exposed key. Secrets embedded in code can sit public for years unnoticed, which is why credentials belong in a managed store and repositories need continuous scanning.
