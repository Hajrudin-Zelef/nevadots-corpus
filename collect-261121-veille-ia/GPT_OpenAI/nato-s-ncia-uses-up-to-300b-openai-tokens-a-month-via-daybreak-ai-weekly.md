---
id: collect-261121-veille-ia/mikrotik/nato-s-ncia-uses-up-to-300b-openai-tokens-a-month-via-daybreak-ai-weekly
title: "NATO's NCIA Uses Up to 300B OpenAI Tokens a Month via Daybreak | AI Weekly"
domain: mikrotik
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["openai", "ncia", "daybreak", "nato", "confirmed", "cert", "polska", "enisa"]
source: https://aiweekly.co/alerts/natos-ncia-uses-up-to-300b-openai-tokens-a-month-via-daybreak
source_anchor: ""
source_lines: [1, 36]
section: "GPT_OpenAI"
---
## TL;DR

- NCIA confirmed four national agencies are active Daybreak users, with ENISA and CERT Polska independently validating AI-flagged vulnerabilities in production software.
- CERT Polska found six vulnerabilities in MikroTik RouterOS via Daybreak, all subsequently patched, marking the program's first confirmed national-agency production finds.
- NCIA is conducting formal token-consumption economics analysis before committing to procurement, signaling institutional concern about single-vendor dependency at scale.

NATO's Communications and Information Agency has used up to 300 billion raw input and output OpenAI tokens a month to defend Allied networks, with access routed through Daybreak, OpenAI's program for vetted defenders doing authorized code review, suspicious-activity work and fix testing, Help Net Security reports. The same tier now sits behind cyber-defense work at Ukraine's Ministry of Digital Transformation, France's ANSSI, national agencies in Germany and Poland, ENISA and CERT-EU.

The concrete yield is modest but documented. ENISA used OpenAI models to find four vulnerabilities in software run across EU institutions, and fixes are out for all four. Together with CERT-EU, it reviewed 27 software projects using several frontier models, OpenAI's among them, along with tools such as the Codex security plug-in; maintainers assessed 88 potential issues and confirmed 95% of them as real. CERT Polska used OpenAI models to help find six vulnerabilities in MikroTik RouterOS; its researchers directed the work and checked findings on real systems. MikroTik released fixes, and CERT Polska confirmed they stop the attacks it had seen.

"As we scale adoption, we will build a deeper understanding of token consumption, usage patterns and the underlying economics of AI models. This insight will help us optimise costs, evaluate competing solutions and make informed procurement decisions," said NCIA general manager Dr Dylan Browne.

That procurement line matters because the volume is already at hyperscaler-grade scale while the economics are not. The disclosure also lands inside a thick run of related coverage: it joins 406 OpenAI stories we have tracked in the past 90 days, including a California AG suit filed Monday over the Hugging Face breach.

At NATO's 2026 Crossed Swords exercise, run by the CCDCOE, participants built and tested AI agents for cyber operations inside Daybreak, with an emphasis, the article says, on keeping humans in charge.

## What others are reporting

1. 
                
                                  First-party announcement detailing Trusted Access partnerships with nine nations and EU institutions, the Crossed Swords AI agent track, and ENISA and CERT Polska operational findings. "As we scale adoption, we will build a deeper understanding of token consumption, usage patterns and the underlying economics of AI models." — Dr Dylan Browne, NCIA
2. 
                
                                  Adds internal OpenAI framing: Brockman treating cyber as a larger long-term business than coding, and safety concerns the NATO deployment has raised inside the company. "there's no question cyber will be a bigger business than coding" — Greg Brockman
3. 
                
                                  Juxtaposes NATO credibility against the California AG lawsuit; estimates input-token costs at $60K-plus monthly; flags NCIA's cautious language on evaluating competing solutions.
4. 
                
                                  Leads on operational validation metrics: ENISA confirmed 4 vulns in EU software, CERT Polska confirmed 6 in MikroTik RouterOS, 95% AI hit rate across 88 flagged issues in 27 projects.

          Originally reported by
          **helpnetsecurity.com**
        

Original headline: OpenAI Puts NATO NCIA on Daybreak Cyber Program, Peaking at 300B Tokens a Month
