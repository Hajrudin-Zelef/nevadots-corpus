---
id: collect-261121-veille-ia/claude-anthropic/anthropic-threat-report-summary
title: "Anthropic Threat Report — Summary"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["claude", "threat", "used", "anthropic", "report", "cases", "these", "actors"]
source: https://medium.com/@kevin.mcnam/anthropic-threat-report-751699b00542
source_anchor: ""
source_lines: [1, 93]
section: "Claude_Anthropic"
---
It’s mostly about people using Claude for things it was designed to do.

On September 10th 2026, Anthropic issues a 154 page Threat Report that details about 40 cases from between Dec 2025 and Aug 2026, where “threat actors” tried to use Claude for malicious activity. While the majority of cases simply leverage Claude’s ability to generate content and write code, some of the cases of abuse specifically focus on Claude’s expertise in hacking, threat analysis, reverse engineering, vulnerability discovery, exploit generation. Many of them are also combined with credential theft to gain illicit access and then use of automation to amplify the impact of the malicious activity.

Most of the reported incidents involve state-sponsored groups, financially motivated criminals, state propaganda institutions, and politically motivated individuals. One wonders why Anthropic, which is apparently an “AI safety and research company”, is hosting features that can be utilized in such a malicious fashion. The report covers seven categories of abuse. Each of these is discussed below.

## Cyber Threats

These are cases where the threat actors use Claude’s cyber-security capabilities to break into computer systems, launch phishing attacks, harvest access credential, and launch cyber attacks at adversaries. They seem quite proud of Claude’s ability to do this and specifically call out the “uplift” that threat actors can gain from using Claude.

*“The report also attempts to measure uplift, a term we use to describe the AI capability boost, or how much more harm was caused with AI versus without AI… Many commentators focus on the risk of AI developing exploits at scale. While this is a danger, the risk from AI adoption is more pronounced across the cyber kill chain, where adversaries can operate faster, across a broader and deeper surface area, with fewer resources.”*

So, Anthropic is helping hackers to work faster, broader, and deeper, with fewer resources. Despite all the talk, nowhere does the report actually measure uplift.

The incidents in this section include:

· A Russian operative who used customized AI-driven workflows to automated development, infrastructure acquisition, phishing, persistence through command and control, to data exfiltration in attacks on Ukraine and Europe.

· A group called ShinyHunters that use Claude to accelerate their ability to rapidly scan, exploit, and take over target systems.

· Chinese graduate student in Hunan that used Claude as the engineering and orchestration layer for: intrusion attempts against production systems; reconnaissance of foreign-government networks across the Middle East, Europe, and Southeast Asia; a standing vulnerability-research and exploit development effort against major endpoint-security products; malware development; and an intelligence collection platform.

· French-speaking hacktivist used Claude to target European political parties, media, think-tanks, and the SaaS providers used by these organizations. The actor used Claude to develop and debug the exploit that succeeded against at least four victim websites.

In many of these cases, Claude is providing the expertise to assist the attacker. Why does Anthropic allow it to do this.

## Influence

These are cases where Claude was used in operations to influence specific factions of the population, usually for political purposes. This typically involved flooding social media with posts that support a specific narrative. The AI can be used to generate content and automate the process. AI agents can actively participate. It often involves creating thousands of fake accounts spewing AI generated slop.

*“The actors behind these operations included governments, state-aligned propaganda institutions and state media, as well as private firms selling influence to paying clients, domestic political operators, and in one case an opposition movement in exile.”*

I won’t list the cases in detail. Most simply used Claude for some sort of automated content generation. In some cases, this content was directly sent to thousands of spoofed social media accounts. In others, it was distributed via state run media outlets. One case involved an “influence-as-a-service” operation where you can pay for a specific message to be disseminated, but most were state operated and designed to influence voters and public opinion. They involved Russia, Iran, Malaya, Bangladesh, Kenya, but none from Europe or the Americas.

## Surveillance

Anthropic reported the AI is being used in three ways for surveillance. The first involves using AI to develop software platforms to support surveillance, and point incidents where a Malian operative used Claude to build a platform to monitor the country’s mobile operators, an Iranian used Claude to build a Firefox plugin that spied on users, and the Chinese built an agent to monitor religious affairs. Using Claude to develop code is one of its primary uses. I don’t see how Anthropic can act as the judge as to what’s good or bad.

The second is to use AI to analyze the collected data. Here they point to several cases where Claude was used to analyze social media posts and create reports on individuals. The third use of AI in surveillance is when it is fully integrated with the state surveillance infrastructure. In other word government surveillance operatives routinely use AI in their jobs.

## Conventional Weapons

This section focuses on the use of Claude to develop software for conventional weapons, including firearms, missiles, armed drones, bombs, and other munitions, as well as the targeting and control systems that operate them. The Anthropic threat intelligence team has disrupted the activities on a number of threat actors who are using Claude to develop weapons related software. The report shares details on six cases: three in China, two in Russia, and one in Yemen.

These include the development of missile guidance software in Yemen; a Chinese use of Claude to develop specifications for anti-torpedo weapons; a Russian attempt to develop software to control military drones; Chinese software to jam radar and communications; Russian use of Claude in military procurement; and finally, the Chinese use of Claude to collect intelligence on energy weapons.

## Biological Misuse

Anthropic regards this as one of the most serious risks, but they do seem acknowledge that current models may not yet be up to the task. They say…

*“To our knowledge, no private company, AI or otherwise, has yet shared evidence of the potential misuse of their platforms for biological weapons development publicly.”*

They then go on to presents five cases where the use of their models “could” support the development of biological weapons. These included:

· A request for Claude’s assistance in authoring a grant application for scientific funding for work involving gain-of-function research on the chikungunya virus.

· A researcher using Claude in their research on highly-pathogenic avian influenza (“bird flu”). The research focused on viruses’ adaptation to mammals, and the mechanism by which it causes severe disease beyond the respiratory tract.

· An “covert” account that used Claude to write a grant application for orthopoxvirus research at a state-associated infectious disease laboratory. The application described access to high-containment facilities and planned work with live orthopoxviruses.

· Researchers used Claude to develop an “atlas” of venom toxins.

· Researchers used Claude to write progress reports on various projects on viral and bacterial toxins.

Grant applications and progress reports???

## Scams

Surprisingly, this section contains only one example. I would have expected scammers to be lining up to leverage AI in their nefarious activities.

The example in the report is about how a China-based app studio used Claude to build a network of over 20 dating apps that used AI based personas to converse with the app users. The users thought they were talking to people, but it was actually Claude. Anthropic discovered that more than 4,700 distinct AI personas that engaged in conversations with at least 25,000 unique individuals.

This seems like quite a clever use of AI.

## Distillation

A distillation is technique that uses an AI to train another AI. Anthropic says…

*“We define illicit distillation as an industrial-scale, covert campaign to extract a model’s capabilities and replicate them in another model without authorization. Illicit distillation is typically enabled by fraud: sophisticated networks of fake accounts created with stolen credit cards, login credentials, and API keys.”*

So, I can see why they included these incidents in their Threat Report. Using stolen credentials to steal Anthropic’s intellectual property is definitely bad.

However, if you take away the “stolen credentials” part, this is similar to how Anthropic and other AI vendors crawled the web to collect vast quantities of training data, much of it copyrighted, to build their models in the first place. In the past, Web site owners welcomed crawling bots because the were typically used by search engines to index the web content and attract customers to the Web sites. However, AI crawlers broke that model, because they take the content without bringing any benefit to the Web site owners. In fact, it’s often to the detriment of the Web site owner. An AI trained on a news story, can provide a summery of that story, and the reader no longer has to visit the news Web site, and the Web site loses revenue. I’m sort of happy that AI vendors are now doing this to each other.

Almost all the illicit distillation cases in the report are from Chinese AI companies.

## Conclusion

For me the Threat Report offered very little of value and appeared to be mostly self serving. Most of the “exploits” seem to be Anthropic complaining about people using Claude for things they designed to do in the first place: like research a topic, generate content, write a summary, or analyze some data, write some software; hack into a computer. The exploits pointed out by Anthropic mostly relate to: who is doing it; and what the results are used for.

Anthropic seems highly biased against the Russians, Iranians, and Chinese. I guess this is to be expected from a US company, especially when Chinese AI vendors are their main competition. It seems that in many of these cases, Anthropic is making a value call based on what the result are used for. Using Claude generated content to influence people on social media could go either way. AI bots flooding social media with utter lies is certainly at the evil end of the spectrum. AI can be used to generate all sorts of software. Who gets to say which applications are allowed and which are evil. The biological examples were mostly about using Claude to write grant applications and reports.

Training AI to hack into computer systems is completely irresponsible, and making it available to the public should be a criminal offence.
