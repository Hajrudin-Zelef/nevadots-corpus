---
id: collect-261121-veille-ia/banques-agents/llmjacking-and-the-60-stolen-ai-plan-signals-corps
title: "LLMjacking and the $60 stolen AI plan | Signals Corps"
domain: banques-agents
role: reference
task: article
actors: []
dates: []
keywords: ["keys", "llmjacking", "google", "stolen", "night", "plan", "token", "charges"]
source: https://www.signalscorps.com/blog/ignore-previous-instructions-llmjacking/
source_anchor: ""
source_lines: [1, 51]
section: "Banques_Agents"
---
## tl;dr

A service called Jet Night sells an “unlimited” AI plan for about $60 a month. Google believes it runs on stolen API keys. Jet Night passed nearly $1 million in token costs to one victim organization (tokens are the units AI providers bill by). Running AI on someone else’s account like this is called LLMjacking, after LLMs (large language models). In episode 3 of Ignore Previous Instructions, the Signals Corps podcast, threat researcher Marcus Arnold gives the verdict: the company whose key was stolen pays for every token, and usually finds out from the bill. The hosts’ advice: find out who in your company can switch an AI model on in your cloud accounts, and set spending alerts and limits on every one.

## What we covered

### What Google and Mandiant found (1:00)

On 2 November 2026, Google Threat Intelligence Group and Mandiant reported on LLMjacking. Mandiant, Google’s incident response team, handled several cases in 2026, at financial services, retail, technology and transportation companies in North America, South America and Asia Pacific. Some saw fraudulent charges of more than $100,000 in a single day, and cumulative charges at some organizations reached $1 million before access was revoked. Google says that, unlike cryptomining on stolen servers, LLMjacking can produce large spikes in charges within hours.

Jet Night is the service that sells the $60 plan. Google believes it runs on stolen keys, because the prices Jet Night charges per token are far below typical market rates. Google also says a comparable consumer plan typically costs $100 to $250 a month. One supplier used stolen keys to make more than 500,000 calls for Jet Night in under eight hours. Jet Night passed nearly $1 million in token costs to the victim organization whose keys were used. This is a different figure from the $1 million in cumulative charges above. Google says outages posted on Jet Night’s Telegram channel are likely victims revoking keys.

Another group, which Mandiant tracks as UNC6993, ran a fake page for joining a group of AI users. It asked people to paste in a live login token from the real AI service (the code a service gives you after you sign in). That token let the group into their accounts without their password or multi-factor check. Once it had keys, UNC6993 quickly caused more than $100,000 in charges.

### What counts as LLMjacking (3:25)

LLMjacking is running AI models on someone else’s account or hardware, so they pay for it. The access comes from stolen cloud logins or AI keys, or from model servers left open on the internet. Sysdig, a cloud security company, described it under the name LLMjacking in May 2024. In its first case, attackers used an old Laravel bug to steal cloud credentials and reach Anthropic’s Claude models on Amazon Bedrock. Sysdig estimated a worst case of more than $46,000 a day. Sarah Andrews, a Signals Corps threat researcher, points out that this was a calculation, and Google’s figures are real bills.

### How fast a leaked key gets used (4:11)

In public tests, attackers tried leaked keys within minutes. Many LLMjacking bills start with a leaked key like these. Entro Security leaked working Amazon Web Services (AWS) keys on purpose on GitHub, Pastebin and Reddit. Attackers tried them within 17 minutes on average, and within nine minutes at the fastest.

Google saw another group, which collected exposed AI keys from public GitHub repositories, test them with single-token requests, too small to trigger volume alerts. Sysdig found a proxy tool that says it won’t use AWS keys with prompt logging (a setting that records every request sent to the model) switched on, “for the sake of ‘privacy’”. Sysdig has also seen attackers switch logging off. Keys that pass these checks are pooled and resold through services Google calls “LLM mixers”.

### What Permiso saw on a test key (5:39)

Researchers have also watched what attackers do with a key. In 2024, Permiso, an identity security company, left an AWS key on GitHub with prompt logging switched on, so it could read every request. Within minutes someone tried the key, and it went on to power a sexual role-play chatbot service, with more than 75,000 successful requests over two days of the experiment. KrebsOnSecurity reports that the experiment produced a $3,500 bill from AWS. AWS blocked the activity about 35 hours after requests rose into the thousands, which was about 42 days after the key was first misused. A small percentage of the role-play included child sexual exploitation material. Sarah Andrews, a Signals Corps threat researcher, notes that this harm goes beyond the bill, because the material came from the key owner’s account.

### Microsoft’s lawsuit against Storm-2139 (6:29)

Only one LLMjacking case has reached a court, as far as the hosts could find. In 2024, Microsoft found a group it tracks as Storm-2139 using customers’ Azure OpenAI Service keys, scraped from public sources, to make banned images, including non-consensual intimate images of celebrities. Microsoft sued, and a court order let it seize a website the group used. The court gave Microsoft a default judgment in July 2026 because the defendants did not respond, and the hosts found no criminal charges. Sarah Andrews, a Signals Corps threat researcher, says a single civil case shows how little risk these attackers think they take.

### Breaches at AI developer platforms in October (7:30)

In October 2026, attackers also targeted the places that store many keys. Wiz, a cloud security company, calls it “Black October”: at least nine platforms that developers use to build and run AI apps disclosed incidents. Wiz traced two Amazon Bedrock keys stolen in one breach. The first test came within 30 minutes, and one internet address then made about 1.7 million calls in 13 hours. Wiz estimates the two keys used more than $100,000 of AI at list price. A reseller called Xingjiabi checked both keys, but Wiz says it can’t establish a link between Xingjiabi or any other reseller and the breaches.

### What happens next (8:41)

Google expects LLMjacking to grow, and writes that “threat actors will increasingly prioritize LLMJacking over traditional resource theft like cryptojacking”. Attackers are also building new tools to collect keys. Straiker found ACRStealer, malware spread through fake Claude Code install pages. It is set up to steal API keys from the settings files of the Cline and Continue AI coding assistants. Sysdig saw someone use a model server left open on the internet as the reasoning engine of an automated hacking tool. No provider sent a bill, but the server’s owner still paid for the hardware.

### Who’s actually paying for cheap AI? (9:18)

The company whose key or cloud account was stolen pays. Marcus Arnold, a Signals Corps threat researcher, says: “The reseller keeps the $60, and the company whose key it is pays for every token. Usually they find out from the bill.”

The buyers are a mixed group. In 2024, about 95% of the prompts Sysdig analyzed were role-play, and Sysdig saw access sold into sanctioned countries, where the big AI companies don’t sell. Wiz says the traffic on the stolen keys it traced resembles resold coding-agent traffic. Sarah Andrews, also a Signals Corps threat researcher, thinks that many buyers are developers at ordinary companies who ran out of tokens and found a cheaper plan.

The hosts’ advice is for the companies that pay. If someone switches on an AI model in your cloud account without approval, treat it as an incident: in several of Mandiant’s cases, the AI services were not enabled before the break-in. Alert when anyone turns prompt logging off. Set spending alerts on every account, and the per-model quotas and spending caps Google recommends. Google also advises scanning code for leaked keys and rotating keys automatically, and the hosts add that you should look for leaked keys outside code too. Above all, find out who in your company can switch an AI model on.

## Show notes

The sources we used, in the order the episode uses them:
