---
id: collect-261121-veille-ia/actu-business/developer-tool-shutdown-tracker-2026-2
title: "Developer Tool Shutdown Tracker 2026"
domain: actu-business
role: reference
task: article
actors: []
dates: ["2025-03-20"]
keywords: ["openai", "effective", "until", "source", "january", "bedrock", "google", "keeps"]
source: https://agentdeals.dev/shutdowns
source_anchor: ""
source_lines: [1501, 1669]
section: "Actu_Business"
---
      
          | effective Jan 6, 2027 | OpenAI | The OpenAI API keeps running. The tts-1, tts-1-hd, gpt-4o-mini-tts-2025-03-20 and gpt-4o-mini-tts-2025-12-15 models can be called until January 6, 2027. OpenAI's replacement for each: gpt-realtime-2.1-mini. Source ↗ | LOW | 
      
          | effective Jan 6, 2027 | OpenAI | The OpenAI API keeps running. From January 6, 2027, no organization can create new fine-tuning jobs on OpenAI's self-serve fine-tuning platform. Since May 7, 2026, organizations that never ran fine-tuning cannot create jobs. Since July 2, 2026, organizations that have not run inference on a fine-tuned model in the past 60 days cannot either. Fine-tuned models keep serving inference until their base model is deprecated. Source ↗ | MEDIUM | 
      
          | effective Jan 8, 2027 | AWS | Amazon Bedrock removes Claude Opus 4.1 on January 8, 2027. The date covers us-east-1, us-east-2 and us-west-2. Requests to it on Bedrock fail after that date. It has been Legacy on Bedrock since July 8, 2026. New customers cannot use it. Accounts that leave it unused for 15 days may lose access. Since October 8, 2026 it has been in public extended access. Bedrock says to expect higher prices set by Anthropic. Anthropic's own Claude API retired Claude Opus 4.1 on August 5, 2026. Bedrock's dates apply only to Bedrock. Bedrock lists Claude Opus 4.8, Claude Opus 5 and Claude Opus 5.5. Source ↗ | LOW | 
      
          | effective Jan 10, 2027 | Val Town | Val Town can be used until January 10, 2027, when all its services end. Val Town announced on 2026-11-10 that it is winding down: new signups and plan upgrades ended that day, and Val Town becomes read-only on December 10, 2026, except SQLite and blob storage. Billing is paused, and existing subscriptions continue free until the end. Source ↗ | HIGH | 
      
          | effective Jan 14, 2027 | OpenAI | The OpenAI API keeps running. The gpt-4o (gpt-4o-2024-08-06 and gpt-4o-2024-11-20), gpt-4o-mini, gpt-4.1 and gpt-4.1-mini models, with their fine-tuned versions, can be called until January 14, 2027, when OpenAI shuts down their access. OpenAI names gpt-6-luna as the replacement for all four. Source ↗ | MEDIUM | 
      
          | effective Jan 20, 2027 | OpenAI | The OpenAI API keeps running. The gpt-realtime, gpt-4o-realtime, gpt-realtime-mini, gpt-4o-mini-realtime, gpt-audio, gpt-4o-audio, gpt-audio-mini, gpt-4o-mini-audio and gpt-4o-mini-transcribe-2025-03-20 models can be called until January 20, 2027. OpenAI's substitutes: gpt-realtime-2.1 for gpt-realtime and gpt-4o-realtime; gpt-realtime-2.1-mini for gpt-realtime-mini and gpt-4o-mini-realtime; gpt-audio-1.5 for gpt-audio, gpt-4o-audio, gpt-audio-mini and gpt-4o-mini-audio; gpt-4o-mini-transcribe-2025-12-15 for gpt-4o-mini-transcribe-2025-03-20. Source ↗ | LOW | 
      
          | effective Jan 28, 2027 | Google Gemini API | The Gemini API keeps serving gemini-3.6-flash with no shutdown date. On Gemini Enterprise Agent Platform (formerly Vertex AI), gemini-3.6-flash can be called until January 28, 2027, when Google retires it. Google names gemini-4-hydrogen as the replacement. Source ↗ | LOW | 
      
          | effective Jan 28, 2027 | Google Gemini API | gemini-3.7-flash can be called on Gemini Enterprise Agent Platform (formerly Vertex AI) until January 28, 2027, when Google retires it. Google names gemini-4-hydrogen as the replacement. On the Gemini API it is not shutting down and Google routes its requests to gemini-3.8-flash. Source ↗ | LOW | 
      
          | effective Feb 1, 2027 | OpenAI | The OpenAI API keeps running. The gpt-5.5-cyber-preview model can be called until February 1, 2027, when OpenAI removes it from the API. OpenAI tells users to move to the most capable cyber model available to them. Source ↗ | LOW | 
      
          | effective Feb 1, 2027 | HubSpot | HubSpot's design manager serverless functions stop running on February 1, 2027. Requests to /_hcms/api/ endpoints fail then. No new functions can be created since November 3, 2026. Existing functions cannot be edited from November 17, 2026 but keep running until February 1, 2027. HubSpot cannot migrate them automatically. Rebuild each function in a developer project, served from /hs/serverless/, on platform version 2026.09 with Node.js 24. Workflow custom code actions on Node.js 20 cannot be edited from November 17, 2026, keep running, and should move to Node.js 24. Source ↗ | MEDIUM | 
      
          | effective Feb 1, 2027 | AWS | Amazon ElastiCache charges an Extended Support premium on every cache still running Redis OSS 6 starting February 1, 2027. Standard support for Redis OSS 6 ends January 31, 2027. Caches move to Extended Support automatically. The premium is 80% of the on-demand node price in years 1 and 2 and 160% in year 3. Reserved node discounts do not apply to the premium. AWS's example: a cache.m5.large node in US East (Ohio) costs $0.156 an hour plus $0.1248 an hour. The premium stops when the cache is upgraded to Valkey or Redis OSS 7 or later, or deleted. Source ↗ | MEDIUM | 
      
          | effective Feb 10, 2027 | OpenAI | The OpenAI API keeps running. The gpt-image-2 model and its gpt-image-2-2026-04-21 snapshot can be called until February 10, 2027, when OpenAI removes them from the API. OpenAI names gpt-image-2.6-sunburst or gpt-image-2.6-flare as the replacement. Source ↗ | MEDIUM | 
      
          | effective Feb 26, 2027 | OpenAI | The OpenAI API keeps running. The whisper-1, gpt-4o-transcribe, gpt-4o-mini-transcribe and gpt-4o-transcribe-diarize models can be called until February 26, 2027. OpenAI's substitute for each is gpt-live-transcribe or gpt-transcribe. The /v1/audio/translations endpoint also shuts down on February 26, 2027. OpenAI's replacement is /v1/realtime/translations. Source ↗ | MEDIUM | 
      
          | effective Mar 1, 2027 | AWS | On March 1, 2027, AWS charges for RDS Extended Support on Amazon RDS for PostgreSQL 14 and Aurora PostgreSQL 14 databases that are enrolled and still on version 14. Standard support for PostgreSQL 14 on RDS and Aurora ends February 28, 2027. Databases on PostgreSQL 15 or later are not affected. Each database's RDS Extended Support setting decides what happens: an enrolled PostgreSQL 14 database is charged, and a PostgreSQL 14 database with it disabled is upgraded to a supported version on or shortly after February 28, 2027. The AWS CLI, the RDS API and CloudFormation enable it by default, and the console does not select it for new databases. The charge stops when the database is upgraded to a version under standard support or deleted. For earlier versions AWS charges $0.10 per vCPU-hour in years 1 and 2 in US East (Ohio), and Aurora Serverless v2 on PostgreSQL 14 is $0.085 per ACU-hour in years 1 and 2. On the same date RDS for PostgreSQL 12 moves from $0.100 to $0.200 per vCPU-hour in US East (Ohio). Source ↗ | MEDIUM | 
      
          | effective Mar 15, 2027 | Google Gemini API | The Gemini API keeps running. Gemini 2.5 Flash Image (gemini-2.5-flash-image) can be called until March 15, 2027, when Google shuts it down. Google names gemini-3.1-flash-lite-image as the replacement. Source ↗ | LOW | 
      
          | effective Mar 30, 2027 | AWS | Amazon Bedrock removes 26 models on March 30, 2027. DeepSeek: DeepSeek-R1, DeepSeek-V3.1. Google: Gemma 3 4B IT, Gemma 3 12B IT, Gemma 3 27B PT. Meta: Llama 3 8B Instruct, Llama 3 70B Instruct, Llama 3.1 8B Instruct, Llama 3.1 70B Instruct, Llama 3.3 70B Instruct, Llama 4 Maverick 17B Instruct, Llama 4 Scout 17B Instruct. MiniMax: MiniMax M2, MiniMax M2.1. Mistral AI: Devstral 2 123B, Mistral 7B Instruct, Mixtral 8x7B Instruct, Voxtral Mini 3B 2507, Voxtral Small 24B 2507. Moonshot AI: Kimi K2 Thinking. NVIDIA: NVIDIA Nemotron Nano 9B v2, NVIDIA Nemotron Nano 12B v2 VL BF16. Qwen: Qwen3-Coder-30B-A3B-Instruct, Qwen3 Coder 480B A35B Instruct. Z.AI: GLM 4.7, GLM 4.7 Flash. They have been Legacy on Bedrock since September 30, 2026. New customers cannot use them. Accounts that leave one unused for 15 days may lose access. No new Provisioned Throughput can be created. Bedrock moves them to public extended access before the end date, with higher prices set by the provider. It has not announced when. After March 30, 2027 requests to them on Bedrock fail. Bedrock does not migrate applications automatically. The date covers the Regions Bedrock lists for each model, us-west-2 among them for all 26. A model can stay available in other Regions. Bedrock names no replacement. Its model cards list newer models: DeepSeek V3.2, GLM 5, Gemma 4, Kimi K3, MiniMax M2.5 and Mistral Large 3. Source ↗ | MEDIUM | 
      
          | effective Mar 31, 2027 | Firebase | Firebase keeps running. The Firebase Extensions service can be used until March 31, 2027. Installed extensions keep running after that date. After March 31, 2027, they cannot be updated, reconfigured or uninstalled, and their configuration cannot be downloaded. Google's replacement is function kits in Cloud Functions for Firebase (2nd gen). Source ↗ | MEDIUM | 
      
          | effective Mar 31, 2027 | Google Gemini API | The Gemini API keeps running. Live API preview model gemini-2.5-flash-native-audio-preview-12-2025 can be called until March 31, 2027, when Google shuts it down. Google names gemini-3.8-live as the replacement. The other Live API preview, gemini-3.1-flash-live-preview, has no shutdown date. Google had dated both in November and moved the dates. Source ↗ | LOW | 
      
          | effective Apr 1, 2027 | OpenAI | The OpenAI API keeps running. The gpt-5.3-codex, gpt-5.1 and gpt-5.4-nano models can be called until April 1, 2027. OpenAI's replacements: gpt-6-sol for gpt-5.3-codex and gpt-5.1; gpt-6-luna for gpt-5.4-nano. Source ↗ | MEDIUM | 
      
          | effective Apr 1, 2027 | Google Gemini API | text-embedding-005, text-embedding-004, text-multilingual-embedding-002 and multimodalembedding@001 can be called on Gemini Enterprise Agent Platform (formerly Vertex AI) until April 1, 2027, when Google retires them. Google’s table names no replacement for them. Source ↗ | MEDIUM | 
      
          | effective Apr 8, 2027 | AWS | Amazon Bedrock removes Claude Sonnet 4.5 on April 8, 2027. The date covers its commercial AWS Regions. In the AWS GovCloud (US) Regions it is later in April 2027. Requests to it on Bedrock fail after that date. Anthropic's own Claude API retires the model at the end of November 2026. Bedrock says only its own dates apply to Bedrock usage. It has been Legacy on Bedrock since October 8, 2026. New customers cannot use it. Accounts that leave it unused for 15 days may lose access. From January 2027 it is in public extended access. Bedrock says to expect higher prices set by Anthropic. Bedrock lists Claude Sonnet 5 and Claude Sonnet 5.5. Source ↗ | MEDIUM | 
      
          | effective May 7, 2027 | Google Gemini API | The Gemini API keeps running. Gemini 3.1 Flash-Lite (gemini-3.1-flash-lite) can be called until May 7, 2027, when Google shuts it down. Google names gemini-3.5-flash-lite as the replacement. Source ↗ | LOW | 
      
          | effective Aug 31, 2027 | AWS | AWS Lambda blocks creating new functions on its deprecated runtimes from July 29, 2027. It blocks updating the code and configuration of existing functions from August 31, 2027. AWS is delaying these blocks beyond its usual 30 and 60 days after deprecation. Functions on .NET 8, Python 3.10 and the other deprecated runtimes can still be created and updated until those dates through the AWS CLI, AWS SAM or CloudFormation, not the Lambda console. After both dates, existing functions can still be invoked and can still be switched to a supported runtime. Deprecated runtimes include Node.js 16, 18 and 20; Python 3.8, 3.9 and 3.10; Ruby 3.2; .NET 6 and 8; and provided.al2. Python 3.10 was deprecated on October 31, 2026 and .NET 8 on November 10, 2026. These runtimes are deprecated next and get the same two dates: Ruby 3.3 on March 31, 2027, Node.js 22 on May 30, 2027, and Python 3.11 and Java 8, 11 and 17 on Amazon Linux 2 on June 30, 2027. Older runtimes are the exception: functions on Node.js 14, Python 3.7, Ruby 2.7, Go 1.x, Java 8 on Amazon Linux and provided can no longer be created since 2024 and can no longer be updated from August 31, 2027. Source ↗ | MEDIUM | 
      
          | effective Sep 30, 2027 | smartlook.com | Smartlook shuts down on 2027-09-30. Source ↗ | HIGH | 
      
          | effective Sep 30, 2027 | Firebase | Firebase keeps running. Firebase Test Lab can be used until September 30, 2027. After that date, no tests can run. Google's replacement is the Developer Device Platform on Google Cloud. The Developer Device Platform requires billing. The no-cost Spark plan includes 10 virtual-device tests a day and 5 physical-device tests a day. Source ↗ | MEDIUM | 
      
          | effective Nov 7, 2027 | AWS | After November 7, 2027, Amazon OpenSearch Service isolates every domain still running Elasticsearch 1.5, 2.3, 5.1, 5.3, 5.5, 6.0, 6.2, 6.3, 6.4, 6.5, 6.7, 7.1, 7.4, 7.7 or 7.8, or OpenSearch 1.0, 1.1, 1.2, 2.3, 2.5, 2.7 or 2.9. Isolated domains can no longer be accessed. AWS keeps their data for at least 30 days. Data must be restored into a new domain on a supported version. New domains cannot be created on these versions after that date. Since November 7, 2026, these domains have paid an Extended Support surcharge equal to their instance price, doubling the cost. Elasticsearch 1.5 and 2.3 domains cannot be upgraded in place. Upgrading before November 7, 2027 stops the surcharge and avoids isolation. Source ↗ | MEDIUM | 
      
          | effective May 14, 2028 | Google Gemini API | The Gemini API keeps running. The embedding model gemini-embedding-001 can be called until May 14, 2028, when Google shuts it down. Google names gemini-embedding-2 as the replacement. Source ↗ | LOW | 
    
  

  ## Methodology

  
    **How we track shutdowns:** We monitor official vendor announcements, API changelogs, developer blogs, and community reports for service deprecation and shutdown notices. Each entry was read from the vendor’s official documentation on the day it was added.

    **Urgency classification:** Imminent (<30 days) — take action now. Upcoming (30–90 days) — plan migration. Later (90+ days) — start planning when convenient.

    **Stability ratings** are computed from our deal changes database of 567 tracked changes. View the full stability dashboard.

    **Updates:** This page is updated as new shutdowns are announced. Subscribe to our Atom feed for change notifications, or query via MCP server.

   
  ## Related Guides

  
  
    ### More Alternatives Guides

    - LocalStack CE Alternatives — In March 2026, LocalStack merged Community and Pro images, requiring an auth token for the latest image — compare 9 free open-source AWS emulators
- Postman Alternatives — Postman killed free team collaboration March 1, 2026 — 5 free API testing alternatives
- HCP Terraform Alternatives — HCP Terraform legacy free plan ended March 31, 2026; free tier now caps managed resources at 500 — free IaC alternatives compared
- Hetzner Alternatives After April 2026 Price Increase — Hetzner raised prices twice in 2026 and its Cost-Optimized line is unavailable — cloud hosting alternatives with free tiers
- Freshping Alternatives — Freshping shut down March 6, 2026 — 13 free uptime monitoring alternatives
- Heroku Alternatives — Heroku removed free tier Nov 2022, entered sustaining mode Feb 2026 — 8 free PaaS options
- Firebase Alternatives — Firebase Studio is closing (no new workspaces since June 22, 2026; shutdown March 22, 2027) + Cloud Storage for Firebase now requires Blaze — 7 BaaS alternatives
- GitHub Actions Alternatives — GitHub postponed its self-hosted runner fee, so self-hosted runners stay free — 10 free CI/CD alternatives compared
- Cursor Alternatives — Cursor credit-based pricing drives alternatives search — 8 free AI coding tools compared
- Datadog Alternatives — Unpredictable pricing drives developer search — 12 free monitoring and observability alternatives compared
- Vercel Alternatives — Hobby plan limits and $20/seat Pro pricing drive alternatives search — 10 free deployment platforms compared
- Auth0 Alternatives — B2C paid plans start at $35/mo (Essentials) and $240/mo (Professional) — 9 free authentication platforms compared
- MongoDB Alternatives — 512 MB free tier + SSPL license drive alternatives search — 10 free databases compared
- Redis Alternatives — Redis's move to RSALv2/SSPLv1 (AGPLv3 added in Redis 8) + 30 MB free tier — 8 open-source and managed alternatives compared
- Best Free AI APIs and Coding Tools in 2026 — Compare 65 free AI APIs, LLM inference, vector databases, and coding tools — exact limits and rate caps
- Best Free Database Hosting for Developers in 2026 — Compare 30+ free databases by type — Postgres, document, key-value, edge, graph, vector, and time-series
- Best Free Hosting for Developers in 2026 — Compare 30+ free hosting options by type — PaaS, static/JAMstack, serverless, containers, VPS, and edge/CDN
- Best Free Monitoring Tools for Developers in 2026 — Compare 70+ free monitoring tools by type — APM, uptime, logs, error tracking, and infrastructure
- Email Service Alternatives — SendGrid retired its free plan in 2025 — 8 free transactional email alternatives compared
- Best Free CI/CD Tools for Developers in 2026 — 35+ free CI/CD tools compared — build minutes, runners, and pipelines by type (general, cloud-native, mobile, IaC)
- Best Free Security Tools for Developers in 2026 — 100+ free security tools compared — SAST/DAST, secret scanning, dependency analysis, container security, and identity/auth
- Best Free Cloud Storage for Developers in 2026 — 55+ free cloud storage tools compared — object storage, media/image CDN, file hosting, and general-purpose storage
- Best Free Testing Tools for Developers in 2026 — 45+ free testing tools compared — browser, visual regression, load, E2E, API, and code coverage
- Best Free Analytics Tools for Developers in 2026 — 45+ free analytics tools compared — product analytics, web analytics, event tracking, and data infrastructure
- Best Free AI & ML Tools for Developers in 2026 — 65+ AI/ML tools and their free tiers compared — LLM APIs, AI coding assistants, ML platforms, observability, and specialized AI services
- Best Free Design Tools for Developers in 2026 — 100+ free design tools compared — UI design, prototyping, component libraries, icons, stock assets, and color tools
- Best Free Email Tools for Developers in 2026 — 59+ free email tools compared — transactional APIs, marketing platforms, verification services, forwarding, and email infrastructure
- Best Free Project Management & Collaboration Tools in 2026 — 93+ free project management tools compared — issue tracking, kanban boards, team chat, video conferencing, scheduling, and knowledge management
- Best Free IDEs, Code Editors & AI Coding Tools in 2026 — 59+ free IDEs and coding tools compared — desktop editors, cloud IDEs, AI coding assistants, AI app builders, and specialized development environments
- Best Free LLM APIs in 2026 — 25+ LLM API providers and their free tiers compared — proprietary model APIs, open-model inference platforms, and AI gateways with exact rate limits
- Best Free API Development Tools in 2026 — 39+ free API development tools compared — REST/GraphQL clients, mocking, documentation, marketplaces, and integration platforms
- Q1 2026 Developer Pricing Report — Pricing changes in Q1 2026 — free tiers removed, narrative analysis, category breakdown, monthly timeline, Cloudflare counter-trend, Q2 outlook
- Hetzner Cloud Pricing After the April and June 2026 Adjustments — Hetzner's 2026 price increases analysed — every cloud plan priced and its availability, impact assessment, 8+ alternatives compared
- Best Free Team Collaboration Tools in 2026 — 60+ free team collaboration tools compared — chat, video conferencing, documentation, scheduling, and async communication
- The Complete Free Startup Stack for 2026 — Complete free SaaS infrastructure stack — 10 categories with recommended picks, scaling guidance, and stability ratings
- The Complete Free AI/ML Stack for 2026 — Complete free AI/ML development stack — 10 categories with recommended picks, scaling guidance, and stability ratings
- The Complete Free DevOps Stack for 2026 — Complete free DevOps infrastructure stack — 10 categories with recommended picks, scaling guidance, and stability ratings
- The Complete Free Frontend Stack for 2026 — Complete free frontend/Jamstack development stack — 10 categories with recommended picks, scaling guidance, and stability ratings
- The Complete Free Next.js Stack for 2026 — Complete free Next.js full-stack infrastructure — 10 layers with recommended picks, growth cost analysis, and stability ratings
- The Complete Free Django/Python Stack for 2026 — Complete free Django/Python infrastructure — 10 layers with recommended picks, growth cost analysis, and stability ratings
- The Complete Free FastAPI/Python Stack for 2026 — Complete free FastAPI/Python infrastructure — 10 layers with recommended picks, growth cost analysis, and stability ratings
- The Complete Free Go/Golang Stack for 2026 — Complete free Go/Golang infrastructure — 10 layers with recommended picks, growth cost analysis, and stability ratings
- The Complete Free SaaS Starter Stack for 2026 — Complete free SaaS infrastructure stack — 11 categories with opinionated picks, growth cost analysis at 4 scale points, and Stripe payments integration
- Q2 2026 Developer Pricing Preview — Q2 2026 pricing preview — upcoming changes, deadlines, impact analysis, and what to watch
- Google Developer Program 2026 — Standalone Google Developer Program Premium no longer takes sign-ups — current plans, Cloud credits and free alternatives
- Supabase vs Firebase Free Tier Comparison — Deep comparison of Supabase and Firebase free tiers — database, auth, storage, functions, and scaling costs
- Vercel vs Netlify Free Tier Comparison — Deep comparison of Vercel and Netlify free tiers — bandwidth, functions, builds, commercial use, and scaling costs
- Neon vs Supabase Free Tier Comparison — Deep comparison of Neon and Supabase free tiers — branching, auth, storage, functions and scaling costs
- Railway vs Render Free Tier Comparison — Deep comparison of Railway and Render free tiers — usage-based vs fixed pricing, databases, sleep behavior, and scaling costs
- Datadog vs New Relic Free Tier Comparison — Deep comparison of Datadog and New Relic free tiers — per-host vs per-GB pricing, APM, logs, synthetics, and scaling costs
- Free Tier Risk Index — Predictive risk analysis for developer free tiers — grades dated and scored against what happened next, category heatmap, pattern analysis, counter-trends
- Developer Free Tier Stability Dashboard — Visual stability dashboard — which developer free tiers are safe, watched, volatile, or improving
- Gemini API Pricing 2026 — Gemini API billing guide — spend caps ($250-$100K+/mo), prepaid billing, 3.1 Pro paid-only, free tier changes, 8-provider comparison
- Free Tier Tracker 2026 — A list of developer tool free tiers that were removed or cut in 2026, grouped by quarter, with dates and what changed.
- Startup Credits Comparison 2026 — The definitive startup credits comparison — 16 programs across cloud infrastructure, fintech, and developer tools with eligibility requirements, vesting schedules, and stacking strategies
- AI Coding Tools Pricing Comparison 2026 — The definitive AI coding tools comparison — 17 tools across IDE, CLI, cloud agent, and app builder categories with free tier analysis and cost breakdowns
- CI/CD Tools Pricing Comparison 2026 — The definitive CI/CD pricing comparison — 17+ tools across general, cloud-native, mobile, and self-hosted categories with free tier analysis and cost breakdowns
- Database Pricing Comparison 2026 — The definitive database pricing comparison — 25+ services across managed Postgres, serverless/edge, document/NoSQL, cloud provider, and specialized categories with free tier analysis and cost breakdowns
- Vector Database Pricing Comparison 2026 — The definitive vector database pricing comparison — 11 services across dedicated cloud, open-source, pgvector, embedded, and serverless categories with free tier analysis for RAG/AI
- Cloud Hosting & PaaS Pricing Comparison 2026 — The definitive cloud hosting pricing comparison — 15 platforms across PaaS, edge/serverless, full-featured, and static categories with free tier analysis, pricing gotchas, and Railway referral
- LLM API Free Tiers & Free Credits 2026 — Which LLM APIs have a genuinely free tier or free credits — frontier labs, inference providers, open-source hosts, and specialized services with free tier analysis and token cost breakdowns
- AWS Free Tier 2026 — AWS Free plan: up to $200 in credits over 6 months, 30+ always-free services, short-term trials, hidden costs, cheaper alternatives.
- GCP Free Tier Complete Guide 2026 — Complete GCP free tier guide — 20+ always-free products, $300 trial, hidden costs, and comparison with AWS and Azure
- Azure Free Tier 2026 — Azure free tier guide — 65+ always-free services, $200 trial, Cosmos DB lifetime free tier, and comparison with AWS and GCP
- DigitalOcean Free Tier Complete Guide 2026 — Complete DigitalOcean guide — $5 free credits, App Platform free tier, per-second billing, and Big Three comparison
- Cloud Free Tier Comparison 2026 — Side-by-side comparison of AWS, GCP, Azure, and DigitalOcean free tiers — compute, databases, serverless, storage, startup credits, and hidden costs
- Database Free Tier Comparison 2026 — Side-by-side comparison of 10+ database free tiers — Postgres, BaaS, edge, key-value, and vector databases compared
- CI/CD Free Tier Comparison 2026 — Side-by-side comparison of 10+ CI/CD free tiers — build minutes, concurrency, runners, storage, and hidden costs compared
- Serverless Free Tier Comparison 2026 — Side-by-side comparison of 10+ serverless free tiers — invocations, compute time, cold starts, billing models, and hidden costs compared
- Auth & Identity Comparison 2026 — Side-by-side comparison of 20+ auth free tiers — MAU/MRU limits, SSO, M2M tokens, agentic AI auth, self-hosted options, and growth cost traps
- Free Tiers for Error Tracking, Monitoring & Observability 2026 — Side-by-side comparison of 25+ monitoring free tiers — data ingest, retention, APM, error tracking, uptime, and the observability cost trap at 10 to 500 hosts
- Email & Transactional Messaging Comparison 2026 — Side-by-side comparison of 20+ email free tiers — SendGrid exodus migration guide, transactional vs marketing breakdown, growth cost at 10K-1M emails/month, deliverability traps
- Storage & CDN Comparison 2026 — Side-by-side comparison of 15+ storage and CDN free tiers — S3 egress tax breakdown, zero-egress providers, media CDN, self-hosted options, and scaling costs at 100GB/1TB/10TB/100TB
- Analytics & Product Analytics Free Tier Comparison 2026 — Side-by-side comparison of 15+ analytics free tiers — event limits, session replays, feature flags, data retention, and the analytics cost trap at scale
- Testing & QA Tools Free Tier Comparison 2026 — Side-by-side comparison of 15+ testing tool free tiers — E2E, visual regression, load testing, API testing, local dev, and the testing cost trap at scale
- API Development Tools Free Tier Comparison 2026 — Side-by-side comparison of 12+ API development tool free tiers — users, collections, requests, mock servers, local-first vs cloud, and the API tool migration trap
- Hosting & PaaS Free Tier Comparison 2026 — Side-by-side comparison of 12+ hosting free tiers — bandwidth, compute, build minutes, cold starts, commercial use restrictions, and the hosting cost trap at scale
- Developer Security Tools Free Tier Comparison 2026 — Side-by-side comparison of 20+ developer security tool free tiers — SAST, SCA, DAST, secrets detection, container security, and the DevSecOps cost trap at scale
- State of Developer Free Tiers 2026 — Data-driven analysis of developer tool free tiers by category — trends, risks, and recommendations
- OpenAI Assistants API Sunset — OpenAI Assistants API sunset August 2026 — migration paths, free AI API alternatives, and cost comparison
- OpenAI Assistants API Migration Guide 2026 — Comprehensive Assistants API migration guide — feature map, complexity assessment, decision framework, agent frameworks, wire-compatible bridges, cost comparison
- OpenAI Assistants API Sunset: Migration Cost Guide & Alternatives — OpenAI Assistants API shutdown guide — migration cost comparison for Responses API, Azure, Anthropic, Gemini, and open-source alternatives
- Firebase Studio Shutdown Guide — Firebase Studio shuts down March 22, 2027 — free cloud IDE alternatives with compute, storage, and collaboration limits compared
- Tenor API Shutdown: GIF API Alternatives & Migration Guide — Tenor API shutdown June 2026 — GIF API alternatives with free tier limits, migration code examples, and platform impact analysis
- Developer Services That Accept Agent Payments (2026) — Directory of developer services accepting AI agent payments via x402 and Stripe MPP — per-call pricing, free tiers, and protocol comparison
- AWS App Runner Migration Guide: Alternatives with Free Tiers & Pricing (2026) — AWS App Runner closed to new customers April 30, 2026 — migration guide to ECS Express Mode and container deployment alternatives with pricing comparison
- Free-Tier Facts AI Models Get Wrong (2026) — AI models give old free-tier terms
- Accounting Software 2026: Free Plans and Price Increases — QuickBooks Free connects one bank account. QuickBooks, Xero, FreshBooks raised prices in 2026. Zoho Books and Wave did not.
- Business Bank Accounts 2026: Free Plans, Fees and Changes — Mercury and Brex send domestic wires for $0 on free accounts. Found Plus now costs $35 a month. northone.com redirects to Relay.
- Forming a US Company in 2026: Costs and Changes — Firstbase Start costs $99 under an offer plus a required $299-a-year agent. Stripe pays the $500 Atlas fee back for a $5,000 Treasury deposit. Delaware's LLC tax is $400.
- WhatsApp Business API Pricing 2026: Service Messages No Longer Free — Since 2026-10-01 each business phone number gets 1,000 free service messages a month and then pays per message.
- Mistral API Pricing 2026: Free Tier, Costs and Limits — The Free plan has included no API access since about 2026-11-05. The API is for business customers only.

View all alternatives guides →
