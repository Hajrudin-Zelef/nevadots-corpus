---
id: collect-261121-veille-ia/actu-business/developer-tool-shutdown-tracker-2026-1
title: "Developer Tool Shutdown Tracker 2026"
domain: actu-business
role: reference
task: article
actors: []
dates: ["2026-11-21"]
keywords: ["completed", "replaces", "developer", "shutdown", "april", "tool", "tracker", "shutdowns"]
source: https://agentdeals.dev/shutdowns
source_anchor: ""
source_lines: [1, 1500]
section: "Actu_Business"
---
# Developer Tool Shutdown Tracker 2026

  API sunsets, deprecations & migration deadlines — updated as new shutdowns are announced

  What this section prints is decided by the calendar, not by anything the vendor did. A shutdown is grouped by how many days its deadline is from 2026-11-21: 30 or fewer is Imminent, up to 90 is Upcoming, beyond that is Later. Shutdowns quoted from listings are marked Today on their stated date. After their stated date, shutdowns move to Recently Completed, below. Every pricing change we hold for these vendors is in the full change log.

  Published 2026-04-02 · Reviewed 2026-11-01 · 101 active shutdowns · Next deadline in 4 days · Figures in the tables below come from our records for 1,498 developer tools

  
  
  
    ## By month

    - **April 2026:** Alibaba Cloud Qwen Code's free tier (Qwen OAuth) (April 15, completed; Qwen Code points users to Alibaba Cloud Coding Plan, OpenRouter, Fireworks AI or another provider); AWS App Runner for new customers (April 30, completed; existing customers keep using it, including creating new services; AWS recommends ECS Express Mode); HubSpot Contact Lists API v1 (April 30, completed; its endpoints return HTTP 404, replaced by Lists API v3; six Contacts API v1 read endpoints still work but no longer return list memberships); Deprecation of AWS Lambda's Node.js 20 runtime (nodejs20.x) (April 30, completed; existing functions can still be invoked indefinitely; Lambda blocks creating nodejs20.x functions from July 29, 2027 and updating them from August 31, 2027).
- **May 2026:** OpenAI Realtime API Beta, requests with the OpenAI-Beta: realtime=v1 header (May 12, completed; the GA Realtime API keeps running); OpenAI's dall-e-2 and dall-e-3 in the API, including the image variations endpoint, which supported only dall-e-2 (May 12, completed; this includes POST /v1/images/generations calls that leave out the model parameter, which default to dall-e-2; OpenAI's substitutes are gpt-image-2, gpt-image-1, which shut down on October 23, or gpt-image-1-mini, which shuts down on December 1, and its DALL·E model pages now recommend GPT-Image-2.5 Sunburst); xAI's grok-4-1-fast-reasoning, grok-4-1-fast-non-reasoning, grok-4-fast-reasoning, grok-4-fast-non-reasoning, grok-4-0709, grok-3, grok-code-fast-1 and grok-imagine-image-pro (May 15, completed; the names still resolve and are served by grok-4.3, grok-build-0.1 and grok-imagine-image-2.0); Google Maps Platform Client IDs (May 31, completed; switch to API keys; requests from a paused client ID usually return within a few hours, Owners or Editors can unpause it in the Cloud Console, and Support can grant a temporary exemption); Mistral AI's mistral-large-2411, pixtral-large-2411, devstral-medium-2507, devstral-small-2507, voxtral-mini-2507 and mistral-ocr-2505 (May 31, completed), mistral-moderation-2411 (June 30), mistral-small-2506, magistral-small-2509, open-mistral-nemo-2407, devstral-2512 and magistral-medium-2509 (July 31), mistral-medium-2505 and mistral-medium-2508 (August 31) and mistral-ocr-4-0 (September 30) (a request to a retired model fails with a 404; Mistral Medium 3.5 replaces mistral-large-2411, pixtral-large-2411, devstral-medium-2507, devstral-2512, magistral-medium-2509, mistral-medium-2505 and mistral-medium-2508; Mistral Small 4 replaces devstral-small-2507, mistral-small-2506 and magistral-small-2509; Voxtral Mini Transcribe 2 replaces voxtral-mini-2507; OCR 4.1 replaces mistral-ocr-2505 and mistral-ocr-4-0; Mistral Moderation 2 replaces mistral-moderation-2411; Ministral 3 8B replaces open-mistral-nemo-2407).
- **June 2026:** Gemini 2.0 Flash (gemini-2.0-flash) and 2.0 Flash-Lite (gemini-2.0-flash-lite) (June 1, completed; Google recommends gemini-3.6-flash for 2.0 Flash and gemini-3.1-flash-lite for 2.0 Flash-Lite); Firebase Studio new workspaces and sign-ups (June 22, completed; existing workspaces keep working until March 22, 2027; Google recommends moving workspaces to Google AI Studio or Google Antigravity); Tenor API (June 30, completed; only the API ended: Tenor.com, the GIF Keyboard apps and Gboard still serve Tenor GIFs; Klipy or GIPHY offer Tenor-compatible v2 endpoints); AWS Fargate Platform Version 1.3.0 (June 30, completed; its tasks fail to launch; Platform Version 1.4.0 replaces it).
- **July 2026:** DeepSeek API model names deepseek-chat and deepseek-reasoner (July 24, completed; replaced by deepseek-flash and deepseek-v4-pro; DeepSeek API still serves deepseek-v4-pro; NVIDIA's free endpoint for deepseek-ai/deepseek-v4-pro ended August 7, 2026); GitHub Models (July 30, completed); Phase Two's free shared realms (July 30, completed).
- **August 2026:** Hypertune (August 10, completed); OpenAI Assistants API: Assistants, Threads, Runs and Messages endpoints (August 26, completed; replaced by the Responses API and the Conversations API).
- **September 2026:** OpenAI Videos API (Sora): the /v1/videos endpoints and the sora-2 and sora-2-pro models, including their dated snapshots (September 24, completed; OpenAI says there is no one-to-one replacement; third-party video generation APIs include Runway, Pika and Luma); OpenAI's gpt-3.5-turbo-instruct, babbage-002, davinci-002 and gpt-3.5-turbo-1106 (September 28, completed; replaced by gpt-5.6-terra); AWS App Mesh (September 30, completed; migrate to Amazon ECS Service Connect, Istio, Linkerd or EKS native service mesh).
- **October 2026:** xAI's grok-voice-transcribe-1.0 (October 2, completed; served by grok-voice-transcribe-2.0 at the same price); AWS Proton (October 7, completed; the Proton console and Proton resources are no longer accessible, while deployed CloudFormation stacks and the resources they manage keep running; migrate to CloudFormation Git Sync, Harmonix on AWS, CodePipeline with CodeBuild, or GitHub Actions); GPT-5.5 in ChatGPT, ChatGPT Work and Codex (October 14, completed; for Codex with ChatGPT sign-in, OpenAI names GPT-6.1 Sol as the replacement on Plus, Pro, Business, Enterprise and Edu plans and GPT-6 Luna on Free and Go plans; the OpenAI API is not affected); NVIDIA's free endpoint for nvidia/nemotron-3-super-120b-a12b (October 19, completed); gemini-2.5-pro, gemini-2.5-flash and gemini-2.5-flash-lite on Gemini Enterprise Agent Platform (formerly Vertex AI) (October 20, completed; Google names gemini-3.8-flash; on the Gemini API, gemini-2.5-pro and gemini-2.5-flash run until December 1 and gemini-2.5-flash-lite has no shutdown date); Together AI serverless access to meta-llama/Llama-3.3-70B-Instruct-Turbo and deepseek-ai/DeepSeek-V4-Flash-0731 (October 22, completed; Together recommends meta-models/Muse-Glimmer-30B or a dedicated endpoint in place of Llama 3.3 70B, and deepseek-ai/DeepSeek-V4.1-Flash in place of DeepSeek-V4-Flash-0731); Gemini Omni Flash Preview (gemini-omni-flash-preview) and the Veo 3.1 preview models (veo-3.1-generate-preview, veo-3.1-fast-generate-preview and veo-3.1-lite-generate-preview) on the Gemini API (October 22, completed; Google names gemini-omni-1.1-flash as the replacement for all four); OpenAI's gpt-3.5-turbo (gpt-3.5-turbo-0125, gpt-3.5-turbo-completions), gpt-4 (gpt-4-0613, gpt-4-0613-completions, gpt-4-completions), gpt-4-turbo (gpt-4-turbo-2024-04-09, gpt-4-turbo-completions), gpt-4.1-nano (gpt-4.1-nano-2025-04-14), gpt-4o-2024-05-13, o1 (o1-2024-12-17), o1-pro (o1-pro-2025-03-19), o3-mini (o3-mini-2025-01-31), o4-mini (o4-mini-2025-04-16) and gpt-image-1 in the API, with fine-tuned versions of gpt-3.5-turbo, gpt-4, gpt-4.1-nano, o4-mini, babbage-002 and davinci-002 (October 23, completed; OpenAI's substitutes: gpt-5.6-terra for gpt-3.5-turbo and o4-mini; gpt-5.6-sol for gpt-4, gpt-4-turbo, gpt-4o-2024-05-13, o1 and o3-mini; gpt-5.6-sol with reasoning.mode: pro for o1-pro; gpt-5.6-luna for gpt-4.1-nano; gpt-image-2.5-sunburst or gpt-image-2.5-flare for gpt-image-1; for fine-tunes, gpt-5.6-terra replaces gpt-3.5-turbo, o4-mini, babbage-002 and davinci-002, gpt-5.6-sol replaces gpt-4 and gpt-5.6-luna replaces gpt-4.1-nano; the gpt-5.6 models do not support fine-tuning or the legacy /v1/completions endpoint); the Gemini API's deep-research-pro-preview-12-2025 agent (October 23, completed; replaced by deep-research-preview-04-2026); NVIDIA's free endpoint for z-ai/glm-5.3 (October 27, completed; z-ai/glm-5.3-flash is still free); apiary.io (October 31, completed; Oracle stopped the service and deleted remaining instances, and told customers using the API Blueprint format to convert to OpenAPI and move the files to tools such as Apimatic, Postman, Swagger Editor, Stoplight, Redocly or Bump.sh); Z.ai GLM 5.2 (zai-glm-5-2) on Mistral AI's API (October 31, completed; Mistral replaced it with GLM 5.3); Ona Cloud (October 31, completed; Ona environments did not move to Codex automatically; Ona said it would refund October payments and unused purchased credits).
- **November 2026:** xAI's grok-imagine-image-quality (November 2, completed; served by grok-imagine-image-2.0 at low quality); Val Town's new signups and plan upgrades (November 10, completed; existing accounts work as before until December 10, when Val Town becomes read-only except SQLite and blob storage, and all its services end on January 10, 2027); Runpod's REST API v1 (November 15, completed; the base URL moves to https://api.runpod.io/v2); Claude Sonnet 4.6, Claude Opus 4.6 and GPT-OSS-120b on Google Antigravity's Free and Google AI Plus plans (November 18, completed; the plans keep running with only Gemini 3.8 Flash); pay-as-you-go API access to MiniMax-M2.7, MiniMax-M2.7-highspeed, MiniMax-M2.5 and MiniMax-M2.5-highspeed (November 18, completed; pay-as-you-go requests to these models no longer work; for M Plan and Token Plan users, requests to M2.5 and M2.7 are routed to M3.1 Flash; existing model names continue to work for backward compatibility); NVIDIA's free endpoints for deepseek-ai/deepseek-v4.1-flash, meta/llama-3.2-90b-vision-instruct and meta/llama-guard-4-12b (November 16, completed) and openai/gpt-oss-20b (November 18, completed; 55 other free endpoints ended between May 12 and November 12, 2026, including openai/gpt-oss-120b on September 3, each named with its date on agentdeals.dev/vendor/nvidia-nim; z-ai/glm-5.3-flash is still a free endpoint); Azure OpenAI o1 (version 2024-12-17), o1-pro (2025-03-19), o3 (2025-04-16), o3-deep-research (2025-06-26), o3-mini (2025-01-31), o3-pro (2025-06-10) and o4-mini (2025-04-16) (November 19, completed; Microsoft names gpt-5.6-sol as the replacement for o1, o1-pro, o3, o3-deep-research and o3-pro, and gpt-5.6-terra for o3-mini and o4-mini; Microsoft upgrades Global Standard, Data Zone Standard and Standard deployments to the named replacement where an upgrade is supported; Provisioned deployments of these models stopped working: Microsoft does not upgrade them, and requests to them return HTTP 410 Gone; Microsoft Foundry also retired Llama-4-Maverick-17B-128E-Instruct-FP8 that day, with no replacement named); 24 Workers AI models: @cf/moonshotai/kimi-k2.6, @cf/moonshotai/kimi-k2.7-code, @cf/zai-org/glm-5.2, @cf/zai-org/glm-5.3-flash, @cf/deepseek-ai/deepseek-v4-flash-0731, @cf/deepseek-ai/deepseek-v4-pro-0813, @cf/deepseek-ai/deepseek-r1-distill-qwen-32b, @cf/nvidia/nemotron-3-120b-a12b, @cf/qwen/qwq-32b, @cf/qwen/qwen3-30b-a3b-fp8, @cf/meta/llama-3.1-8b-instruct-fp8, @cf/meta/llama-3.2-1b-instruct, @cf/meta/llama-3.2-3b-instruct, @cf/meta-llama/llama-2-7b-chat-hf-lora, @cf/google/gemma-2b-it-lora, @cf/google/gemma-7b-it-lora, @cf/myshell-ai/melotts, @cf/stabilityai/stable-diffusion-xl-base-1.0, @cf/runwayml/stable-diffusion-v1-5-inpainting, @cf/bytedance/stable-diffusion-xl-lightning, @cf/lykon/dreamshaper-8-lcm, @cf/openai/whisper-tiny-en, @cf/llava-hf/llava-1.5-7b-hf and @cf/google/embeddinggemma-300m (November 20, completed; requests to them fail with HTTP 410 Gone; Cloudflare recommends @cf/deepseek-ai/deepseek-v4.1-flash-1017 or @cf/zai-org/glm-5.3 for frontier models and @cf/google/gemma-4-26b-a4b-it or @cf/qwen/qwen3.8-27b for smaller ones; replacements: @cf/deepgram/aura-2-en for @cf/myshell-ai/melotts; @cf/black-forest-labs/flux-2-klein-9b for @cf/stabilityai/stable-diffusion-xl-base-1.0, @cf/runwayml/stable-diffusion-v1-5-inpainting, @cf/bytedance/stable-diffusion-xl-lightning and @cf/lykon/dreamshaper-8-lcm; @cf/deepgram/nova-3 for @cf/openai/whisper-tiny-en; @cf/moondream/moondream3.1-9B-A2B for @cf/llava-hf/llava-1.5-7b-hf; @cf/google/embeddinggemma-2, not yet available, for @cf/google/embeddinggemma-300m); OpenAI's models in Cursor (November 25); Solo (November 30); OpenAI Agent Builder, Evals platform and reusable prompts (November 30; existing evals read-only since October 31, 2026); Claude Sonnet 4.5 (November 30).
- **December 2026:** Cloudflare's DNS Analytics and DNS Firewall Analytics REST APIs (December 1); Railway's Config as Code (railway.json and railway.toml) (December 1); Zeabur: shared-cluster services paused (December 1; no new dedicated servers or domains since October 17, 2026); remove.bg's self-service API and website (December 1); Gemini 2.5 Pro (gemini-2.5-pro) and Gemini 2.5 Flash (gemini-2.5-flash) on the Gemini API (December 1); Prisma Accelerate (December 1); gpt-image-1-mini, gpt-image-1.5 and chatgpt-image-latest (December 1); publishing new pods and versions to CocoaPods trunk (December 2); Gemini Omni 1.1 Flash (gemini-omni-1.1-flash) on the Gemini API (December 3); HubSpot's Pipelines API v1 (December 4); Chroma Cloud: read-only (December 7); AWS Infrastructure Composer's standalone console (December 7; closed to new customers since October 29, 2026); ARM32 job execution on GitHub-hosted runners (December 8); Firecrawl's v1 API and the /extract endpoint (December 10); Val Town: read-only except SQLite and blob storage (December 10; no new signups or plan upgrades since November 10, 2026); custom GPTs in ChatGPT (December 11); gpt-5, gpt-5-mini, gpt-5-nano, gpt-5-pro, o3 and o3-pro (December 11); gemini-live-2.5-flash-native-audio on Gemini Enterprise Agent Platform (formerly Vertex AI) (December 13); Claude Fable 5, Claude Haiku 4.5, Gemini 3.8 Flash, Gemini 4 Argon, Grok 4.6 and Kimi K3 in GitHub Copilot (December 14); Azure OpenAI speech model retirement (December 15); Azure OpenAI gpt-realtime-mini 2025-12-15 retirement (December 15); three Gemini text-to-speech preview models (December 18); Chroma Cloud: stops serving reads (December 22); Inception's Mercury API (December 28); Zeabur: everything stops (December 31); Supabase legacy API keys (December 31); LaunchDarkly REST API versions older than 20240415 (December 31); Google Jules (December 31; read-only since November 11, 2026); HCP Vagrant and the Vagrant Public Registry (December 31; no new boxes or registries since October 1, 2026); HiveMQ Cloud Serverless (December 31).
- **January 2027:** AWS support plans: Developer, Business and Enterprise On-Ramp (January 1; closed to new subscriptions since December 2, 2025); CircleCI's GraphQL Unstable API (January 1); Google's Custom Search JSON API (January 1; closed to new customers); Python 3.10 on Heroku (January 6); creating fine-tuning jobs on OpenAI's self-serve platform (January 6; since July 2, 2026 only organizations that used a fine-tuned model in the past 60 days can create jobs); OpenAI tts-1, tts-1-hd and gpt-4o-mini-tts (January 6); Claude Opus 4.1 on Amazon Bedrock (January 8; Legacy since July 8, 2026: closed to new customers); Val Town: all other services end (January 10); LeanCloud (January 12); Reddit's public Data API (January 12); Azure OpenAI gpt-4o 2024-05-13 retirement (January 14); gpt-4o, gpt-4o-mini, gpt-4.1 and gpt-4.1-mini (January 14); OpenAI legacy realtime and audio models (January 20); Heroku Postgres 15: forced upgrades start (January 28; no new databases since November 11, 2026, except forks and followers); gemini-3.7-flash on Gemini Enterprise Agent Platform (formerly Vertex AI) (January 28); gemini-3.6-flash on Gemini Enterprise Agent Platform (formerly Vertex AI) (January 28); Google App Engine standard: Python 2.7, Java 8, PHP 5.5 and Go 1.11 (January 31; no deploys or redeploys since January 31, 2026); Sourcegraph Cody (January 31).
- **February 2027:** HubSpot's design manager serverless functions (/_hcms/api/) (February 1; no new functions since November 3, 2026, no edits since November 17); CircleCI's v1 and v1.1 trigger and rerun API endpoints (February 1); gpt-5.5-cyber-preview (February 1); Netlify Visual Editor (February 8); gpt-image-2 (February 10); Azure Deployment Environments (February 22); whisper-1, the gpt-4o transcription models and /v1/audio/translations (February 26); Heroku Postgres 15: access restrictions and deletions start (February 28).
- **March 2027:** Gemini 2.5 Flash Image (March 15); Firebase Studio (Full Shutdown) (March 22); GitHub Actions' Ubuntu 22.04 runner images (ubuntu-22.04, ubuntu-22.04-arm): first brownout: jobs on these labels fail from 14:00 UTC (March 23); 26 Amazon Bedrock models from DeepSeek, Google, Meta, Mistral AI, Qwen and others (March 30; Legacy since September 30, 2026: closed to new customers); AWS WorkSpaces Thin Client (March 31; closed to new customers since April 20, 2026; existing customers can use it until then); Lemon Squeezy (March 31); Cloudflare's Zone Settings Batch API (March 31); Azure Cache for Redis Enterprise and Enterprise Flash tiers (March 31; no new caches since April 1, 2026); the Live API preview gemini-2.5-flash-native-audio-preview-12-2025 (March 31); Gadget (March 31; no new apps since November 9, 2026; existing apps keep running until then); Stackify Retrace (March 31); Firebase Extensions (March 31); Pipedream Workflows and String (March 31).
- **April 2027:** Deno Deploy (April 2027); text-embedding-005, text-embedding-004, text-multilingual-embedding-002 and multimodalembedding@001 on Gemini Enterprise Agent Platform (formerly Vertex AI) (April 1); gpt-5.1, gpt-5.3-codex and gpt-5.4-nano (April 1); Python 3.10 on Cloud Functions for Firebase and Cloud Run functions (April 4; Node.js 20 functions cannot be deployed since October 30, 2026); Opsgenie, Atlassian's on-call and alerting service (April 5); Claude Sonnet 4.5 on Amazon Bedrock (April 8; Legacy since October 8, 2026: closed to new customers); Azure OpenAI gpt-4o, gpt-4o-mini and gpt-4.1 version retirement (April 14); GitHub Actions' Ubuntu 22.04 runner images (ubuntu-22.04, ubuntu-22.04-arm): jobs on these labels are terminated with an error (April 17); Mistral Large 3, Mistral Medium 3.5, Voxtral TTS, Voxtral Mini Transcribe 2 and Voxtral Mini Transcribe Realtime (April 30); Auth0 Hooks (April 30; read-only since November 18, 2024); Amazon Q Developer (April 30).
- **May 2027:** Heroku's Heroku-22 stack: no more builds or deploys (May 1; since November 1, 2026 the first build in each 30 days fails, and a retry deploys); Gemini 3.1 Flash-Lite (May 7); Auth0's Node.js 18 extensibility runtime (May 10).
- **June 2027:** Azure Fluid Relay (June 2); Azure OpenAI gpt-image-1.5 retirement (June 16); Microsoft To Do apps (June 30); Google Cloud Deployment Manager (June 30; closed to new users since June 30, 2026; existing deployments can use it until then).
- **July 2027:** Node.js 20 AWS Lambda Runtime (July 29); AWS Lambda's deprecated runtimes: Python 3.8, 3.9 and 3.10, Node.js 16, 18 and 20, .NET 6 and 8, Ruby 3.2 and provided.al2: no new functions on these runtimes (July 29); New Relic REST API v2 and Deployments v0 API (July 31).
- **August 2027:** AWS Lambda's deprecated runtimes: Python 3.8, 3.9 and 3.10, Node.js 16, 18 and 20, .NET 6 and 8, Ruby 3.2 and provided.al2: no more code or configuration updates (August 31).
- **September 2027:** Google Cloud Video Intelligence API (September 14); Functions on Azure Container Apps V1 (September 29); Amazon DevOps Guru (September 30; closed to new customers since October 29, 2026; existing customers can use it until then); Firebase Test Lab (September 30); smartlook.com (September 30).
- **October 2027:** Twilio Notify (October 31; closed to new customers since 2022).
- **November 2027:** Amazon OpenSearch Service domains on Elasticsearch 1.5, 2.3, 5.1, 5.3, 5.5, 6.0, 6.2, 6.3, 6.4, 6.5, 6.7, 7.1, 7.4, 7.7 and 7.8, and OpenSearch 1.0, 1.1, 1.2, 2.3, 2.5, 2.7 and 2.9 (November 7).
- **January 2028:** Auth0 Rules (January 31; read-only since November 18, 2024).
- **May 2028:** the gemini-embedding-001 embedding model (May 14).
- **September 2028:** Microsoft Dev Box (September 18); Azure Cache for Redis Basic, Standard and Premium tiers (September 30; since April 1, 2026 only tenants that already had a cache can create new ones); Azure Communication Services (ACS) standalone services (September 30; since October 23, 2026 new customers cannot get SMS or calling numbers).

## ⚠️ Imminent (26)

    These shutdowns are happening within the next 30 days. Take action now.

    
      
      Cursor keeps running, and models from Anthropic, Google, Meta and Grok stay available. OpenAI's models in Cursor, such as GPT-5.6 Sol, GPT-5.6 Terra and GPT-5.6 Luna, are available through November 25, 2026, after OpenAI moved its November 12 cutoff. OpenAI is ending its contract to supply models to Cursor and gives Cursor no newer models, and Cursor says it will share more before November 25.

      
      
     
    
      
      Solo can be used until November 30, 2026, when all Solo websites and data are deleted, with no way to recover a site, account or data afterwards. Solo announced on 2026-10-01 that it is shutting down. Solo sends each site's owner a ZIP with an HTML export of the site, a CSV of links to its images, and its contact form and newsletter submissions; image source files are not in the ZIP and must be downloaded one by one. Pro and Grow subscriptions get a prorated refund calculated as of October 1, 2026, and do not renew.

      
      
        
        
      

     
    
      
      The OpenAI API keeps running. Agent Builder, the Evals platform and the v1/prompts API can be used until November 30, 2026. Existing evals become read-only on October 31, 2026. ChatKit remains available. Migrate from Agent Builder to the Agents SDK or ChatGPT Workspace Agents. Migrate from Evals to Promptfoo. Move reusable prompt content into application code.

      
      
     
    
      
      Claude Sonnet 4.5 (claude-sonnet-4-5-20250929) retires from the Claude API on 2026-11-30, and requests to it fail after that date. Anthropic deprecated it on 2026-09-30 and names claude-sonnet-5-5 as the replacement. The Claude API and Anthropic's other models keep running. Amazon Bedrock removes Claude Sonnet 4.5 later, on April 8. Google Cloud sets its own retirement date.

      
      
        
        
      

     
    
      
      Cloudflare DNS keeps running. On December 1, 2026, Cloudflare retires the DNS Analytics API and the DNS Firewall Analytics API, its REST APIs for DNS query analytics. Their endpoints are GET /zones/{zone_id}/dns_analytics/ and GET /accounts/{account_id}/dns_firewall/{dns_firewall_id}/dns_analytics/report. Cloudflare tells integrations to move to its GraphQL Analytics API before then to keep working.

      
      
        
        
      

     
    
      
      Railway keeps running. Railway's Config as Code, the railway.json and railway.toml files that configure a service, keeps working for services that already use it until December 1, 2026, a hard cutoff, and stops working then. New services already cannot use Config as Code. The replacement is Infrastructure as Code in a .railway/railway.ts file. The command "railway config migrate --apply" writes that file from existing railway.json or railway.toml files.

      
      
     
    
      
      Zeabur is shutting down. On December 1, 2026, Zeabur pauses every running service on its shared clusters, so shared-cluster apps stop serving that day unless restarted by hand, which Zeabur offers for backing up data. Dedicated servers cannot be renewed from December 1, so each one shuts down on the day its current term expires, even if that is before December 31. On December 31, 2026, all remaining dedicated servers and shared clusters stop. New dedicated servers and domains can no longer be bought since October 17, 2026.

      
      
        
        
      

     
    
      
      On December 1, 2026, at 9:00 CET, remove.bg's self-service API stops accepting requests, and its standalone website and accounts close; unused pay-as-you-go credits expire the same day. remove.bg moves its background removal to Canva, and its API to Leonardo.Ai's API, also part of Canva. Enterprise API contracts continue as normal.

      
      
        
        
      

     
    
      
      Google shuts down Gemini 2.5 Pro (gemini-2.5-pro) and Gemini 2.5 Flash (gemini-2.5-flash) on the Gemini API on December 1, 2026. Both can be used until that date. After the shutdown, requests that still name gemini-2.5-pro are answered by a different model, gemini-3.8-flash, billed at Gemini 3.8 Flash rates, and requests that still name gemini-2.5-flash are answered by gemini-3.5-flash-lite, billed at Gemini 3.5 Flash-Lite rates. Google deprecated both on 2026-11-02 and lists breaking API changes in a migration guide for each replacement. Gemini 2.5 Flash-Lite (gemini-2.5-flash-lite) is not deprecated. The Gemini API keeps running. On Gemini Enterprise Agent Platform (formerly Vertex AI), Google already retired gemini-2.5-pro, gemini-2.5-flash and gemini-2.5-flash-lite on October 20.

      
      
     
    
      
      Prisma stopped offering Accelerate for new setups in its Console by 2026-10-02, and Accelerate, including Prisma Postgres connections that go through it, is retired on 2026-12-01. Existing Accelerate connections keep working until then. Prisma Postgres keeps running, and its databases come with connection pooling.

      
      
        
        
      

     
    
      
      The OpenAI API keeps running. The gpt-image-1-mini, gpt-image-1.5 and chatgpt-image-latest models can be called until December 1, 2026. OpenAI's substitutes are gpt-image-2.6-sunburst or gpt-image-2.6-flare.

      
      
     
    
      
      CocoaPods trunk stops accepting new pods and new versions on December 2, 2026, under the CocoaPods team's read-only plan. Existing builds keep working because the Specs repo and CDN stay up, but dependencies published through trunk get no more updates. Projects with their own specs repos are not affected.

      
      
        
        
      

     
    
      
      Google shuts down Gemini Omni 1.1 Flash (gemini-omni-1.1-flash) on the Gemini API on December 3, 2026. It can be used until that date. After the shutdown, requests that still name gemini-omni-1.1-flash are answered by a different model, gemini-omni-1.2-flash, billed at Gemini Omni 1.2 Flash rates. Google released Gemini Omni 1.2 Flash on 2026-11-05. The Gemini API keeps running.

      
      
     
    
      
      HubSpot sunsets Pipelines API v1 on December 4, 2026. After that date, every call to v1 endpoints stops working. Migrate to Pipelines API 2026-09 or any 2026-03 or newer version.

      
      
        
        
      

     
    
      
      On December 7, 2026, Chroma Cloud becomes read-only: applications can no longer create, update or delete data in it. On December 22, 2026, Chroma Cloud stops serving reads, so data must be exported before then. Open-source ChromaDB keeps running under the Apache 2.0 license. Chroma announced the wind-down on 2026-10-23, as it joins Replit.

      
      
        
        
      

     
    
      
      The standalone AWS Infrastructure Composer console shuts down on December 7, 2026. New customers have not been able to sign up since October 29, 2026. Existing customers can use the console until December 7, 2026. The visual editor remains in the AWS Toolkit for Visual Studio Code. CloudFormation templates and stacks continue to work normally.

      
      
     
    
      
      GitHub Actions keeps running. GitHub-hosted runners end support for ARM32 (32-bit ARM) jobs on Linux and Windows by December 8, 2026, including native AArch32 user-space execution, because Node 24 does not support ARM32. Self-hosted runners on ARM32 have not been supported since Node 20 was removed in September 2026.

      
      
     
    
      
      On December 10, 2026 at 15:00 UTC, Firecrawl's v1 API stops working and returns HTTP 410, and so does /extract on both v1 and v2, including /v2/extract. The v2 endpoints /v2/scrape, /v2/crawl, /v2/batch/scrape, /v2/search and /v2/map keep running. The replacements for /extract are /v2/scrape with a JSON format and /v2/agent. firecrawl-py 2.x calls the v1 API despite its version number. Old SDK versions that call v1 are firecrawl-py 1.x and 2.x, @mendable/firecrawl-js 1.x and firecrawl-mcp 1.x. Upgrade to the latest SDK release. Moving a v1 REST call to v2 means changing the base URL from https://api.firecrawl.dev/v1 to https://api.firecrawl.dev/v2.

      
      
        
        
      

     
    
      
      Val Town can be used until January 10, 2027, when all its services end. Val Town announced on 2026-11-10 that it is winding down: new signups and plan upgrades ended that day, and Val Town becomes read-only on December 10, 2026, except SQLite and blob storage. Billing is paused, and existing subscriptions continue free until the end.

      
      
        
        
      

     
    
      
      On December 11, 2026, OpenAI retires custom GPTs on every ChatGPT plan, and custom GPTs and their GPT pages become inaccessible. FedRAMP workspaces and Enterprise workspaces with an approved deferral keep them until February 11, 2027. Existing GPTs keep working until the retirement date. A creator can migrate a GPT to a ChatGPT plugin, which carries its instructions, knowledge files and connected apps but not its custom actions; replacing a custom action may need a custom MCP server. Conversations with custom GPTs stay accessible after retirement.

      
      
     
    
      
      The OpenAI API keeps running. gpt-5 and gpt-5-2025-08-07 are available until December 11, 2026, replaced by gpt-6.1-sol. gpt-5-mini and gpt-5-mini-2025-08-07 are available until December 11, 2026, replaced by gpt-6.1-terra. gpt-5-nano and gpt-5-nano-2025-08-07 are available until December 11, 2026, replaced by gpt-6-luna. gpt-5-pro and gpt-5-pro-2025-10-06 are available until December 11, 2026, replaced by gpt-6.1-sol with reasoning.mode: pro in the Responses API. o3 and o3-2025-04-16 are available until December 11, 2026, replaced by gpt-6.1-sol. o3-pro and o3-pro-2025-06-10 are available until December 11, 2026, replaced by gpt-6.1-sol with reasoning.mode: pro in the Responses API. o3-mini is a different model from o3, and OpenAI shut it down on October 23.

      
      
     
    
      
      gemini-live-2.5-flash-native-audio can be called on Gemini Enterprise Agent Platform (formerly Vertex AI) until December 13, 2026, when Google retires it. The December 13 date does not apply to the Gemini API, where gemini-2.5-flash-native-audio-preview-12-2025 keeps running until March 31. Google names gemini-3.8-live as the replacement.

      
      
     
    
      
      GitHub Copilot keeps running. On December 14, 2026, GitHub deprecates six models across all Copilot experiences, including Copilot Chat, inline edits, ask and agent modes, and code completions: Claude Fable 5 (GitHub's suggested alternative is Claude Fable 5.2), Claude Haiku 4.5 (Claude Haiku 5.5), Gemini 3.8 Flash (Gemini 4 Hydrogen), Gemini 4 Argon (Gemini 4 Carbon), Grok 4.6 (Grok 4.7) and Kimi K3 (alternative to be announced). Claude Haiku 4.5 stays available to individual Copilot subscribers on annual plans.

      
      
        
        
      

     
    
      
      Azure OpenAI retires whisper, tts and tts-hd (version 001) on December 15, 2026. After that date every request to them returns HTTP 410 Gone. Microsoft names no replacement. OpenAI's own API retires whisper-1 later, in February 2027, and tts-1 and tts-1-hd later, in January 2027. Those are OpenAI's dates, and Azure OpenAI's are Microsoft's.

      
      
     
    
      
      Azure OpenAI retires gpt-realtime-mini version 2025-12-15 on December 15, 2026. gpt-realtime-mini version 2025-10-06 retires later on Azure OpenAI, in April 2027, and is not covered by this date. Microsoft names no replacement. After that date requests to version 2025-12-15 return HTTP 410 Gone. OpenAI's own API retires gpt-realtime-mini later, on January 20, and that is OpenAI's date, not Azure's.

      
      
     
    
      
      The Gemini API keeps running. The text-to-speech preview models gemini-2.5-flash-preview-tts, gemini-2.5-pro-preview-tts and gemini-3.1-flash-tts-preview can be called until December 18, 2026, when Google shuts them down. Google names gemini-3.8-flash-tts or gemini-3.8-flash-lite-tts as the replacement. Google moved this shutdown from an earlier date in November.

      
      
     
  
    ## ⏳ Upcoming (28)

    These shutdowns are 30–90 days away. Plan your migration now.

    
      
      Inception is winding down the Mercury API, which serves Inception's Mercury diffusion language models. The API and the Inception Platform dashboard keep working as they do today through December 28, 2026. After that date the API stops serving requests, and users lose access to the dashboard and to any data submitted to it, such as request logs, billing history and invoices, so those must be saved before then. OpenRouter no longer lists any Mercury model.

      
      
        
        
      

     
    
      
      Supabase says it is deprecating its legacy anon and service_role API keys by the end of 2026 and gives no day they stop working. Legacy keys keep working alongside the new keys until then. Replace the anon key with a publishable key (`sb_publishable_...`) and the service_role key with a secret key (`sb_secret_...`). Supabase's announcement says legacy keys will be deleted in late 2026, a date still to be confirmed. Apps that have not migrated will break then. Send the new keys on the apikey header only. The platform rejects them on the Authorization: Bearer header with Invalid JWT.

      
      
     
    
      
      LaunchDarkly keeps running. On December 31, 2026, LaunchDarkly retires every REST API version except 20240415, and requests made to a retired version stop working. The older versions 20220603, 20210729, 20191212 and 20160426 stayed available during a grace period that ends then. A request uses the API version set on its access token when the token was created, unless the request sends an LD-API-Version header. An integration keeps working only with a token created with version 20240415 or with the header LD-API-Version: 20240415.

      
      
        
        
      

     
    
      
      Jules, Google's asynchronous coding agent, has been read-only since November 11, 2026, and winds down on December 31, 2026. Google announced this on 2026-10-29, giving users until November 11, 2026 to finish their sessions. Code, branches and pull requests Jules created stay in GitHub; settings, memories and session histories can be exported to a local folder.

      
      
        
        
      

     
    
      
      On December 31, 2026, HashiCorp decommissions all remaining HCP Vagrant deployments. After the shutdown dates, boxes can no longer be created or shared through HCP Vagrant or the Vagrant Public Registry: new boxes and registries could not be created from October 1, 2026, and support and maintenance for existing deployments ended on November 2, 2026. The Vagrant CLI and its source on GitHub stay available; boxes must move to a repository the team hosts, which serves the .box files and catalog metadata to the Vagrant CLI.

      
      
        
        
      

     
    
      
      HiveMQ no longer lets anyone create Serverless clusters and retires the Serverless plan (Free and Pay As You Go) on 2026-12-31. Existing clusters run until then. After that date, clients cannot connect and HiveMQ permanently deletes the clusters and their data.

      
      
        
        
      

     
    
      
      AWS discontinues its Developer Support and Business Support plans on January 1, 2027. Accounts on either plan must opt in to Business Support+ before then. A Developer Support account that does not opt in is downgraded to Basic Support. Basic Support gives access to the Resource Center, Service Health Dashboard, Product FAQs and Discussion Forums at no charge. Business Support accounts are not moved to Business Support+ automatically. Business Support+ costs the greater of $29 a month per account or 9% of monthly AWS charges, falling in steps to 3% on larger bills. Neither plan has accepted new subscriptions since December 2, 2025. AWS emails each account 30 days before its plan changes. Enterprise On-Ramp also ends on January 1, 2027, and its customers are moved to Enterprise Support automatically. The three plans stay available in the AWS GovCloud (US) Region.

      
      
     
    
      
      CircleCI keeps running. CircleCI's GraphQL Unstable API (https://circleci.com/graphql-unstable) reaches end of life on January 1, 2027, when integrations, scripts and tools that call it stop working. CircleCI says to move to its V3 REST API and to upgrade the CircleCI CLI to version 1 for anyone still on version 0. CircleCI Server customers are not affected.

      
      
        
        
      

     
    
      
      Existing customers can call Google's Custom Search JSON API until January 1, 2027, when Google discontinues it. It is closed to new customers. Programmable Search Engine engines set to search the entire web, or more than 50 domains, can do so until the same date. Google names Vertex AI Search as the alternative for up to 50 domains.

      
      
        
        
      

     
    
      
      Heroku removes Python 3.10 from its Python buildpacks on January 6, 2027. After that date, apps using Python 3.10 can no longer build or deploy until upgraded to a newer Python version. Existing Python 3.10 apps keep running. This applies on every stack. Python 3.10 reached upstream end of life on October 1, 2026 and gets no security fixes. Heroku deprecated Python 3.10 on November 21, 2025, showing a warning in the build log. Move to Python 3.12, 3.13, 3.14 or 3.15, set in .python-version.

      
      
     
    
      
      The OpenAI API keeps running. From January 6, 2027, no organization can create new fine-tuning jobs on OpenAI's self-serve fine-tuning platform. Since May 7, 2026, organizations that never ran fine-tuning cannot create jobs. Since July 2, 2026, organizations that have not run inference on a fine-tuned model in the past 60 days cannot either. Fine-tuned models keep serving inference until their base model is deprecated.

      
      
     
    
      
      The OpenAI API keeps running. The tts-1, tts-1-hd, gpt-4o-mini-tts-2025-03-20 and gpt-4o-mini-tts-2025-12-15 models can be called until January 6, 2027. OpenAI's replacement for each: gpt-realtime-2.1-mini.

      
      
     
    
      
      Amazon Bedrock removes Claude Opus 4.1 on January 8, 2027. The date covers us-east-1, us-east-2 and us-west-2. Requests to it on Bedrock fail after that date. It has been Legacy on Bedrock since July 8, 2026. New customers cannot use it. Accounts that leave it unused for 15 days may lose access. Since October 8, 2026 it has been in public extended access. Bedrock says to expect higher prices set by Anthropic. Anthropic's own Claude API retired Claude Opus 4.1 on August 5, 2026. Bedrock's dates apply only to Bedrock. Bedrock lists Claude Opus 4.8, Claude Opus 5 and Claude Opus 5.5.

      
      
     
    
      
      LeanCloud shuts down all its services, including app access, data reads and writes, API calls and the console, on January 12, 2027, and then permanently deletes or anonymizes all user data. It has not accepted new user registrations or new apps since January 12, 2026. Existing apps keep running until the shutdown.

      
      
        
        
      

     
    
      
      From January 12, 2027, Reddit begins removing Data API access for apps and users that have not registered or engaged with its Developer Platform team. Remaining public Data API access ends in March 2027, and Reddit gives no day. Apps registered by the end of November 2026 may claim a $1,000 porting bounty. Reddit wants third-party apps to run on its Developer Platform, Devvit.

      
      
        
        
      

     
    
      
      Azure OpenAI retires gpt-4o version 2024-05-13 on January 14, 2027. gpt-4o versions 2024-08-06 and 2024-11-20 and gpt-4o-mini version 2024-07-18 retire later on Azure OpenAI, on April 14, 2027, in all active deployments including Global Standard. Microsoft upgrades Global Standard and Data Zone Standard deployments of version 2024-05-13 to gpt-5.1 version 2025-11-13 that day. Provisioned deployments of version 2024-05-13 are not upgraded. Create a gpt-5.1 deployment and move traffic before the date. Microsoft does not promise automatic upgrades for Global Batch deployments of version 2024-05-13. After that date, requests to version 2024-05-13 return HTTP 410 Gone.

      
      
     
    
      
      The OpenAI API keeps running. The gpt-4o (gpt-4o-2024-08-06 and gpt-4o-2024-11-20), gpt-4o-mini, gpt-4.1 and gpt-4.1-mini models, with their fine-tuned versions, can be called until January 14, 2027, when OpenAI shuts down their access. OpenAI names gpt-6-luna as the replacement for all four.

      
      
     
    
      
      The OpenAI API keeps running. The gpt-realtime, gpt-4o-realtime, gpt-realtime-mini, gpt-4o-mini-realtime, gpt-audio, gpt-4o-audio, gpt-audio-mini, gpt-4o-mini-audio and gpt-4o-mini-transcribe-2025-03-20 models can be called until January 20, 2027. OpenAI's substitutes: gpt-realtime-2.1 for gpt-realtime and gpt-4o-realtime; gpt-realtime-2.1-mini for gpt-realtime-mini and gpt-4o-mini-realtime; gpt-audio-1.5 for gpt-audio, gpt-4o-audio, gpt-audio-mini and gpt-4o-mini-audio; gpt-4o-mini-transcribe-2025-12-15 for gpt-4o-mini-transcribe-2025-03-20.

      
      
     
    
      
      Heroku is retiring Postgres 15 on Heroku Postgres. Databases still on Postgres 15 face forced upgrades from January 28, 2027, and after February 28, 2027, Heroku starts restricting access to them and deleting them, including followers. New Postgres 15 databases cannot be created since November 11, 2026, though forks and followers of existing ones can. Heroku asks users to upgrade to a newer Postgres version as soon as possible. Postgres 16, 17 and 18 remain available on Heroku Postgres.

      
      
     
    
      
      gemini-3.7-flash can be called on Gemini Enterprise Agent Platform (formerly Vertex AI) until January 28, 2027, when Google retires it. Google names gemini-4-hydrogen as the replacement. On the Gemini API it is not shutting down and Google routes its requests to gemini-3.8-flash.

      
      
     
    
      
      The Gemini API keeps serving gemini-3.6-flash with no shutdown date. On Gemini Enterprise Agent Platform (formerly Vertex AI), gemini-3.6-flash can be called until January 28, 2027, when Google retires it. Google names gemini-4-hydrogen as the replacement.

      
      
     
    
      
      Google decommissions the first-generation runtimes of the App Engine standard environment, Python 2.7, Java 8, PHP 5.5 and Go 1.11, on January 31, 2027. After that date, Google says apps still on a decommissioned runtime may be disabled without further notice. Since January 31, 2026, apps on these four runtimes cannot be deployed or redeployed. This applies even in organizations that had re-enabled deployments with an organization policy. Their project configuration cannot be updated. Until January 31, 2027, apps already deployed keep running and receiving traffic. To keep an app running, migrate it to a second-generation App Engine runtime or to Cloud Run. App Engine's second-generation runtimes are not affected by this date.

      
      
        
        
      

     
    
      
      Sourcegraph Cody, Sourcegraph's AI coding assistant, reaches end of life on January 31, 2027. Sourcegraph announced this on 2026-11-09. For code understanding it points Cody users to its Deep Search and MCP integrations, which it says are not one-for-one replacements for every Cody capability.

      
      
        
        
      

     
    
      
      HubSpot's design manager serverless functions stop running on February 1, 2027. Requests to /_hcms/api/ endpoints fail then. No new functions can be created since November 3, 2026. Existing functions cannot be edited from November 17, 2026 but keep running until February 1, 2027. HubSpot cannot migrate them automatically. Rebuild each function in a developer project, served from /hs/serverless/, on platform version 2026.09 with Node.js 24. Workflow custom code actions on Node.js 20 cannot be edited from November 17, 2026, keep running, and should move to Node.js 24.

      
      
        
        
      

     
    
      
      CircleCI keeps running. On February 1, 2027 CircleCI removes 9 legacy v1 and v1.1 API endpoints that trigger and rerun builds, such as POST /api/v1.1/project/:vcs-type/:username/:repo/tree/:branch, and requests to them return HTTP 410 Gone. This applies to CircleCI cloud and CircleCI server. Other v1 and v1.1 endpoints and API v2 and v3 are not affected. To trigger a pipeline, use POST /api/v3/runs or API v2's pipeline endpoints. To rerun, use the v3 or v2 workflow rerun endpoints. CircleCI server has only API v2.

      
      
        
        
      

     
    
      
      The OpenAI API keeps running. The gpt-5.5-cyber-preview model can be called until February 1, 2027, when OpenAI removes it from the API. OpenAI tells users to move to the most capable cyber model available to them.

      
      
     
    
      
      Netlify keeps running. Netlify Visual Editor, previously called Netlify Create, reaches end of life on February 8, 2027, when it is no longer available or supported. As of 2026-11-10, no new teams or projects can enable it. Teams that already have it enabled can keep using it until then. Sites stay live and content stays where it is. To keep editing, use a CMS, point Claude or Codex at netlify.ai, or use Agent Runners. Agent Runners runs on credits, so a credit-based plan or an Enterprise plan that uses credits is needed.

      
      
     
    
      
      The OpenAI API keeps running. The gpt-image-2 model and its gpt-image-2-2026-04-21 snapshot can be called until February 10, 2027, when OpenAI removes them from the API. OpenAI names gpt-image-2.6-sunburst or gpt-image-2.6-flare as the replacement.

      
      
     
  
    ## ✅ Recently Completed (16)

    These shutdowns have already happened. Listed for reference.

    
      
      Workers AI keeps running. Cloudflare removes 24 Workers AI models on November 20, 2026: they can be called until then, and from that date requests to them fail with HTTP 410 Gone (error code 5028). Language models: @cf/moonshotai/kimi-k2.6, @cf/moonshotai/kimi-k2.7-code, @cf/zai-org/glm-5.2, @cf/zai-org/glm-5.3-flash, @cf/deepseek-ai/deepseek-v4-flash-0731, @cf/deepseek-ai/deepseek-v4-pro-0813, @cf/deepseek-ai/deepseek-r1-distill-qwen-32b, @cf/nvidia/nemotron-3-120b-a12b, @cf/qwen/qwq-32b, @cf/qwen/qwen3-30b-a3b-fp8, @cf/meta/llama-3.1-8b-instruct-fp8, @cf/meta/llama-3.2-1b-instruct, @cf/meta/llama-3.2-3b-instruct, @cf/meta-llama/llama-2-7b-chat-hf-lora, @cf/google/gemma-2b-it-lora and @cf/google/gemma-7b-it-lora; Cloudflare recommends @cf/deepseek-ai/deepseek-v4.1-flash-1017 or @cf/zai-org/glm-5.3 for frontier model needs and @cf/google/gemma-4-26b-a4b-it or @cf/qwen/qwen3.8-27b for smaller ones. Other models, each with Cloudflare's replacement: @cf/myshell-ai/melotts (@cf/deepgram/aura-2-en); @cf/stabilityai/stable-diffusion-xl-base-1.0, @cf/runwayml/stable-diffusion-v1-5-inpainting, @cf/bytedance/stable-diffusion-xl-lightning and @cf/lykon/dreamshaper-8-lcm (@cf/black-forest-labs/flux-2-klein-9b); @cf/openai/whisper-tiny-en (@cf/deepgram/nova-3); @cf/llava-hf/llava-1.5-7b-hf (@cf/moondream/moondream3.1-9B-A2B); @cf/google/embeddinggemma-300m (@cf/google/embeddinggemma-2, not yet available).

      
      
        
        
      

     
    
      
      Azure OpenAI retires o1 (version 2024-12-17), o1-pro (2025-03-19), o3 (2025-04-16), o3-deep-research (2025-06-26), o3-mini (2025-01-31), o3-pro (2025-06-10) and o4-mini (2025-04-16) on November 19, 2026. These models are deprecated, so new customers cannot deploy them. After that date a request to a retired model returns HTTP 410 Gone. Microsoft names gpt-5.6-sol as the replacement for o1, o1-pro, o3, o3-deep-research and o3-pro, and gpt-5.6-terra for o3-mini and o4-mini. Microsoft upgrades Global Standard, Data Zone Standard and Standard deployments to the named replacement where an upgrade is supported, and does not upgrade Provisioned deployments. Microsoft Foundry also retires Llama-4-Maverick-17B-128E-Instruct-FP8 that day, with no replacement named.

      
      
     
    
      
      Google Antigravity's Free and Google AI Plus plans keep running. Until November 18, 2026, they include Claude Sonnet 4.6, Claude Opus 4.6 and GPT-OSS-120b; Google removes these models on that date. After November 18, 2026, the $0 plan includes only Gemini 3.8 Flash. Gemini 4 Hydrogen is not on the Free and Google AI Plus plans.

      
      
        
        
      

     
    
      
      MiniMax's API keeps running. MiniMax is retiring the M2.5 and M2.7 model families. These models are no longer available to new users from 2026-10-19 and are retired on 2026-11-18.

      
      
        
        
      

     
    
      
      - Kimi: moonshotai/kimi-k2-instruct and moonshotai/kimi-k2-thinking on 2026-05-12. Source ↗
- GLM: z-ai/glm4.7 on 2026-05-14, z-ai/glm-5.1 on 2026-07-02, z-ai/glm-5.2 on 2026-08-21 and z-ai/glm-5.3 on 2026-10-27 (GLM 5.3 Flash, z-ai/glm-5.3-flash, is still a Free Endpoint). Source ↗
- Embedding models: nvidia/llama-3.2-nv-embedqa-1b-v2 and nvidia/llama-3.2-nemoretriever-300m-embed-v1 on 2026-05-18, and nvidia/nv-embedqa-e5-v5, nvidia/nv-embed-v1, nvidia/llama-nemotron-embed-1b-v2 and baai/bge-m3 on 2026-08-25. Source ↗
- Qwen: qwen/qwen2.5-coder-32b-instruct on 2026-05-12, qwen/qwen3-next-80b-a3b-thinking on 2026-05-21, qwen/qwen3-coder-480b-a35b-instruct on 2026-06-11, qwen/qwen3.5-122b-a10b on 2026-07-20, and qwen/qwen3.5-397b-a17b and qwen/qwen3-next-80b-a3b-instruct on 2026-07-27. Source ↗
- Phi: microsoft/phi-4-mini-instruct and microsoft/phi-4-multimodal-instruct on 2026-07-15. Source ↗
- Mistral: mistralai/mistral-large-3-675b-instruct-2512 on 2026-07-23, mistralai/mixtral-8x7b-instruct-v0.1 on 2026-07-27 and mistralai/mistral-nemotron on 2026-09-28. Source ↗
- Others: stepfun-ai/step-3.5-flash, bytedance/seed-oss-36b-instruct, sarvamai/sarvam-m and abacusai/dracarys-llama-3.1-70b-instruct on 2026-07-27. Source ↗
- DeepSeek: deepseek-ai/deepseek-v4.1-flash on 2026-11-16, deepseek-ai/deepseek-v4-pro and deepseek-ai/deepseek-v4-flash on 2026-08-07, deepseek-ai/deepseek-v4-pro-0813 on 2026-09-14 and deepseek-ai/deepseek-v4-flash-0731 on 2026-09-21. Source ↗
- Llama: meta/llama-3.2-90b-vision-instruct and meta/llama-guard-4-12b on 2026-11-16, meta/llama-3.2-11b-vision-instruct on 2026-11-05, meta/llama-4-maverick-17b-128e-instruct on 2026-07-27, and meta/llama-3.3-70b-instruct, meta/llama-3.1-8b-instruct, meta/llama-3.1-70b-instruct, meta/llama-3.2-1b-instruct and meta/llama-3.2-3b-instruct on 2026-08-26. Source ↗
- Nemotron: nvidia/llama-3.3-nemotron-super-49b-v1, nvidia/llama-3.3-nemotron-super-49b-v1.5, nvidia/llama-3.1-nemotron-nano-8b-v1, nvidia/llama-3.1-nemotron-nano-vl-8b-v1, nvidia/nvidia-nemotron-nano-9b-v2, nvidia/nemotron-nano-12b-v2-vl and nvidia/nemotron-mini-4b-instruct on 2026-08-26, and nvidia/nemotron-3-nano-30b-a3b on 2026-09-01. Source ↗
- NVIDIA no longer serves OpenAI's gpt-oss models as free endpoints: openai/gpt-oss-20b retired 2026-11-18, openai/gpt-oss-120b retired 2026-09-03. Source ↗
- MiniMax: minimaxai/minimax-m2.5 on 2026-05-12, minimaxai/minimax-m2.7 on 2026-07-27 and minimaxai/minimax-m3 on 2026-09-09. Source ↗
- NVIDIA retired Kimi K3 (moonshotai/kimi-k3) on 2026-10-15. Source ↗
- Gemma: google/diffusiongemma-26b-a4b-it on 2026-11-12, google/gemma-3-27b-it on 2026-05-12, and google/gemma-3n-e4b-it and google/gemma-3n-e2b-it on 2026-07-27. Source ↗

        
        
      

     
    
      
      Runpod retires REST API v1 (https://rest.runpod.io/v1) on November 15, 2026. The base URL moves to https://api.runpod.io/v2. REST API v2 reorganizes resource paths and changes request and response shapes. Terraform provider 2.0.0 makes no v1 calls. Runpod's GraphQL API is retired in early 2027 with no date given.

      
      
        
        
      

     
    
      
      Serverless scripting platform, shutting down on January 10, 2027, with no new signups since November 10, 2026.

      
      
        
        
      

     
    
      
      grok-imagine-image-quality was retired on 2026-11-02, and its requests are served by grok-imagine-image-2.0 at low quality.

      
      
        
        
      

     
    
      
      Apiary reaches end of life on 2026-10-31, when Oracle stops the service and deletes remaining instances. Oracle announced it on 2025-10-28. The Free plan is the only one still open, so it ends with the product. Oracle instructed customers using the API Blueprint format to convert to OpenAPI and move the files to tools such as Apimatic, Postman, Swagger Editor, Stoplight, Redocly, or Bump.sh.

      
      
        
        
      

     
    
      
      Z.ai GLM 5.2 (zai-glm-5-2) retired on 2026-10-31, replaced by GLM 5.3.

      
      
        
        
      

     
    
      
      Ona Cloud shuts down on 2026-10-31: Ona's post of 2026-10-13 says Ona environments do not move to Codex automatically, new signups have closed, existing users can keep using it until then, and Ona will refund October payments and unused purchased credits.

      
      
        
        
      

     
    
      
      NVIDIA's free API endpoints keep running, and GLM 5.3 Flash (z-ai/glm-5.3-flash) is not affected. GLM 5.3 (z-ai/glm-5.3) can be called on NVIDIA's free endpoints until October 27, 2026; NVIDIA's model page says it will be deprecated on 10/27/2026 and no longer supported after 10/28/2026.

      
      
        
        
      

     
    
      
      gpt-3.5-turbo (gpt-3.5-turbo-0125, gpt-3.5-turbo-completions), gpt-4 (gpt-4-0613, gpt-4-0613-completions, gpt-4-completions), gpt-4-turbo (gpt-4-turbo-2024-04-09, gpt-4-turbo-completions), gpt-4.1-nano (gpt-4.1-nano-2025-04-14), gpt-4o-2024-05-13, o1 (o1-2024-12-17), o1-pro (o1-pro-2025-03-19), o3-mini (o3-mini-2025-01-31), o4-mini (o4-mini-2025-04-16) and gpt-image-1 shut down in the OpenAI API on October 23, 2026, with fine-tuned versions of gpt-3.5-turbo, gpt-4, gpt-4.1-nano, o4-mini, babbage-002 and davinci-002. OpenAI lists gpt-4-1106-preview for October 23, 2026, and also says access to it ended on March 26, 2026.

      
        Who’s affected: Developers calling any of these model IDs or their fine-tuned versions

        Impact: Apps and fine-tunes pinned to these model IDs stop working

        Migration path: OpenAI's substitutes: gpt-5.6-terra for gpt-3.5-turbo and o4-mini; gpt-5.6-sol for gpt-4, gpt-4-turbo, gpt-4-1106-preview, gpt-4o-2024-05-13, o1 and o3-mini; gpt-5.6-sol with reasoning.mode: pro for o1-pro; gpt-5.6-luna for gpt-4.1-nano; gpt-image-2.5-sunburst or gpt-image-2.5-flare for gpt-image-1. Replacement base models for fine-tunes: gpt-5.6-terra for gpt-3.5-turbo, o4-mini, babbage-002 and davinci-002; gpt-5.6-sol for gpt-4; gpt-5.6-luna for gpt-4.1-nano. The gpt-5.6 models do not support fine-tuning or the legacy /v1/completions endpoint.

       
      
     
    
      
      The Gemini API keeps running. The Deep Research agent preview deep-research-pro-preview-12-2025 can be called until October 23, 2026, when Google shuts it down. Google names deep-research-preview-04-2026 as the replacement.

      
      
     
    
      
      The Together AI API keeps running. meta-llama/Llama-3.3-70B-Instruct-Turbo and deepseek-ai/DeepSeek-V4-Flash-0731 can be called on serverless inference until October 22, 2026, when Together removes them. Together recommends meta-models/Muse-Glimmer-30B or a dedicated endpoint in place of Llama 3.3 70B, and deepseek-ai/DeepSeek-V4.1-Flash in place of DeepSeek-V4-Flash-0731.

      
      
        
        
      

     
    
      
      The Gemini API keeps running. Gemini Omni Flash Preview (gemini-omni-flash-preview) and the Veo 3.1 preview models (veo-3.1-generate-preview, veo-3.1-fast-generate-preview and veo-3.1-lite-generate-preview) can be called until October 22, 2026, when Google shuts them down. Google names gemini-omni-1.1-flash as the replacement for all four.

      
      
     
  
    ## Later (47)

    These shutdowns are more than 90 days away.

    
      
      Azure Deployment Environments retires on February 22, 2027. Creating and deploying environments and other write operations are expected to be blocked on that date. Inventory, read, log and delete stay available for a limited cleanup period. Microsoft Dev Box retires later, in September 2028.

      
      
     
    
      
      The OpenAI API keeps running. The whisper-1, gpt-4o-transcribe, gpt-4o-mini-transcribe and gpt-4o-transcribe-diarize models can be called until February 26, 2027. OpenAI's substitute for each is gpt-live-transcribe or gpt-transcribe. The /v1/audio/translations endpoint also shuts down on February 26, 2027. OpenAI's replacement is /v1/realtime/translations.

      
      
     
    
      
      The Gemini API keeps running. Gemini 2.5 Flash Image (gemini-2.5-flash-image) can be called until March 15, 2027, when Google shuts it down. Google names gemini-3.1-flash-lite-image as the replacement.

      
      
     
    
      
      Complete shutdown — all workspaces deleted, data removed

      
        Who’s affected: Anyone with data still in Firebase Studio workspaces

        Impact: All remaining Firebase Studio data permanently deleted

        Migration path: Google recommends Antigravity or Google AI Studio. Code can also be downloaded as a zip or pushed to GitHub. Apps already deployed to Firebase keep running.

       
      
     
    
      
      GitHub Actions retires its Ubuntu 22.04 runner images, the ubuntu-22.04 and ubuntu-22.04-arm labels, on April 17, 2027, when workflow jobs that use them are terminated with an error. Before that, jobs on these labels fail during brownouts on March 23, March 30, April 6 and April 13, 2027, each from 14:00 UTC to the end of the day UTC. The deprecation began on September 17, 2026, and queue times can be longer at peak hours. Workflows should update to ubuntu-24.04, ubuntu-26.04 or ubuntu-latest, and ubuntu-24.04-arm or ubuntu-26.04-arm for Arm.

      
      
     
    
      
      Amazon Bedrock removes 26 models on March 30, 2027. DeepSeek: DeepSeek-R1, DeepSeek-V3.1. Google: Gemma 3 4B IT, Gemma 3 12B IT, Gemma 3 27B PT. Meta: Llama 3 8B Instruct, Llama 3 70B Instruct, Llama 3.1 8B Instruct, Llama 3.1 70B Instruct, Llama 3.3 70B Instruct, Llama 4 Maverick 17B Instruct, Llama 4 Scout 17B Instruct. MiniMax: MiniMax M2, MiniMax M2.1. Mistral AI: Devstral 2 123B, Mistral 7B Instruct, Mixtral 8x7B Instruct, Voxtral Mini 3B 2507, Voxtral Small 24B 2507. Moonshot AI: Kimi K2 Thinking. NVIDIA: NVIDIA Nemotron Nano 9B v2, NVIDIA Nemotron Nano 12B v2 VL BF16. Qwen: Qwen3-Coder-30B-A3B-Instruct, Qwen3 Coder 480B A35B Instruct. Z.AI: GLM 4.7, GLM 4.7 Flash. They have been Legacy on Bedrock since September 30, 2026. New customers cannot use them. Accounts that leave one unused for 15 days may lose access. No new Provisioned Throughput can be created. Bedrock moves them to public extended access before the end date, with higher prices set by the provider. It has not announced when. After March 30, 2027 requests to them on Bedrock fail. Bedrock does not migrate applications automatically. The date covers the Regions Bedrock lists for each model, us-west-2 among them for all 26. A model can stay available in other Regions. Bedrock names no replacement. Its model cards list newer models: DeepSeek V3.2, GLM 5, Gemma 4, Kimi K3, MiniMax M2.5 and Mistral Large 3.

      
      
     
    
      
      Closed to new customers since 2026-04-20. AWS ends support on 2027-03-31.

      
        Who’s affected: Teams that reach Amazon WorkSpaces through WorkSpaces Thin Client devices

        Impact: After 2027-03-31 the devices stop working: they cannot connect to Amazon WorkSpaces or be managed in the console. Until then existing customers can use them as normal and can still buy devices.

        Migration path: Replace the devices with a partner thin client that supports Amazon WorkSpaces (AWS names Dell) or another supported device. The WorkSpaces themselves keep working.

       
      
     
    
      
      Lemon Squeezy stops processing transactions, including subscription renewals, on March 31, 2027, and stores must move off it by then. It closed new store sign-ups on November 16, 2026. It recommends Stripe Managed Payments, with migration tooling from December 2, 2026 for eligible stores.

      
      
        
        
      

     
    
      
      Cloudflare keeps running. On March 31, 2027, Cloudflare removes the Zone Settings Batch API, the endpoints GET /zones/{zone_id}/settings and PATCH /zones/{zone_id}/settings that read and edit several zone settings in one request. The batch endpoints are no longer available after that date. Integrations must use the per-setting endpoints, GET and PATCH /zones/{zone_id}/settings/{setting_id}, instead. Cloudflare moved the end date once, from a date in 2026.

      
      
        
        
      

     
    
      
      Microsoft retires two tiers of Azure Cache for Redis on March 31, 2027: Enterprise and Enterprise Flash. The Basic, Standard and Premium tiers retire later, on September 30, 2028. From April 1, 2027 every remaining Enterprise cache and every remaining Enterprise Flash cache is disabled. Creating new Enterprise or Enterprise Flash caches has been blocked since April 1, 2026. Reservations for both tiers are supported until March 30, 2027. Microsoft's replacement is Azure Managed Redis. Applications must switch to the new cache's hostname and access credentials. Azure Managed Redis is clustered by default, so applications should check that their client library works with a clustered cache. New customers cannot create Basic, Standard or Premium caches either. Since April 1, 2026, in Azure's public cloud, only a tenant that had any Azure Cache for Redis instance before that date can create them.

      
      
     
    
      
      The Gemini API keeps running. Live API preview model gemini-2.5-flash-native-audio-preview-12-2025 can be called until March 31, 2027, when Google shuts it down. Google names gemini-3.8-live as the replacement. The other Live API preview, gemini-3.1-flash-live-preview, has no shutdown date. Google had dated both in November and moved the dates.

      
      
     
    
      
      Gadget, the hosted platform for building web and Shopify apps, is shutting down. New applications cannot be started since November 9, 2026, and first-time deploys to production are allowed until the end of November 2026. Gadget support remains available until the end of 2026. Existing apps keep running until March 31, 2027, when the platform shuts down.

      
      
        
        
      

     
    
      
      Stackify Retrace, BMC Helix's SaaS application performance monitoring service, is in maintenance mode until its end of life on March 31, 2027: no new features, only critical fixes and security updates. After March 31, 2027, customers cannot log in, send data or access stored data, so anything worth keeping must be exported before then. Stackify Prefix itself is not being shut down on that date; only Premium support for Prefix ends. BMC Helix recommends Retrace customers move to BMC Helix AIOps with OpenTelemetry.

      
      
        
        
      

     
    
      
      Firebase keeps running. The Firebase Extensions service can be used until March 31, 2027. Installed extensions keep running after that date. After March 31, 2027, they cannot be updated, reconfigured or uninstalled, and their configuration cannot be downloaded. Google's replacement is function kits in Cloud Functions for Firebase (2nd gen).

      
      
     
    
      
      Pipedream stopped taking new signups and subscriptions for Workflows on 2026-09-30 and said Workflows and String will shut down on 2027-03-31. Existing workflows keep running until then. Pipedream Connect is not affected; its Free plan covers development only.

      
      
        
        
      

     
    
      
      Deno says Deno Deploy will be sunset in April 2027 and gives no day. Existing applications keep running until then. Deno announced on October 9, 2026 that its team is joining Cloudflare. Deno will help paying customers move to Cloudflare Workers.

      
      
        
        
      

     
    
      
      text-embedding-005, text-embedding-004, text-multilingual-embedding-002 and multimodalembedding@001 can be called on Gemini Enterprise Agent Platform (formerly Vertex AI) until April 1, 2027, when Google retires them. Google’s table names no replacement for them.

      
      
     
    
      
      The OpenAI API keeps running. The gpt-5.3-codex, gpt-5.1 and gpt-5.4-nano models can be called until April 1, 2027. OpenAI's replacements: gpt-6-sol for gpt-5.3-codex and gpt-5.1; gpt-6-luna for gpt-5.4-nano.

      
      
     
    
      
      Google decommissions the Python 3.10 runtime of Cloud Functions for Firebase and Cloud Run functions on April 4, 2027. From that date, functions on Python 3.10 cannot be created or updated. Google says functions that stay on a decommissioned runtime may be disabled. Python 3.10 has been deprecated since October 4, 2026. Until April 4, 2027, functions on Python 3.10 can still be created and updated. Move them to Python 3.11 or a later Python runtime before then. The Node.js 20 runtime of these products was already decommissioned on October 30, 2026. Node.js 20 functions can no longer be deployed. The Firebase CLI refuses with the error "Runtime Node.js 20 was decommissioned on 2026-10-30".

      
      
        
        
      

     
    
      
      Opsgenie shuts down on April 5, 2027, when it stops being accessible and Atlassian deletes any Opsgenie data that has not been moved. Existing customers can use it until then. Atlassian no longer sells Opsgenie. An owner who is also a site admin starts the move to Jira Service Management under Settings > Plan your move.

      
      
        
        
      

     
    
      
      Amazon Bedrock removes Claude Sonnet 4.5 on April 8, 2027. The date covers its commercial AWS Regions. In the AWS GovCloud (US) Regions it is later in April 2027. Requests to it on Bedrock fail after that date. Anthropic's own Claude API retires the model at the end of November 2026. Bedrock says only its own dates apply to Bedrock usage. It has been Legacy on Bedrock since October 8, 2026. New customers cannot use it. Accounts that leave it unused for 15 days may lose access. From January 2027 it is in public extended access. Bedrock says to expect higher prices set by Anthropic. Bedrock lists Claude Sonnet 5 and Claude Sonnet 5.5.

      
      
     
    
      
      Azure OpenAI retires gpt-4o versions 2024-08-06 and 2024-11-20, gpt-4o-mini version 2024-07-18, and gpt-4.1, gpt-4.1-mini and gpt-4.1-nano versions 2025-04-14 on April 14, 2027. Regional Standard deployments that already retired stay retired. Microsoft names gpt-5.1 as the replacement for gpt-4o. No replacement is named for the others. Fine-tuned deployments of these models have later dates on a separate Microsoft schedule. OpenAI's own API shuts down gpt-4o, gpt-4o-mini, gpt-4.1 and gpt-4.1-mini earlier, in January 2027. That is OpenAI's date, not Azure's.

      
      
     
    
      
      Mistral's API keeps running. Mistral Large 3 (mistral-large-2512), Mistral Medium 3.5 (mistral-medium-3-5), Voxtral TTS (voxtral-mini-tts-2603), Voxtral Mini Transcribe 2 (voxtral-mini-2602) and Voxtral Mini Transcribe Realtime (voxtral-mini-transcribe-realtime-2602) can be called until April 30, 2027, when Mistral retires them. Mistral's replacements: Mistral Large 4 for Mistral Large 3 and Mistral Medium 3.5, Voxtral TTS 3 for Voxtral TTS, Voxtral Mini Transcribe 3 for Voxtral Mini Transcribe 2, and Voxtral Mini Transcribe Realtime 3 for Voxtral Mini Transcribe Realtime.

      
      
        
        
      

     
    
      
      Auth0 keeps running. Auth0 Hooks reach end of life on April 30, 2027, when they stop running and are removed. They have been read-only since November 18, 2024, and tenants created from October 16, 2023 never had them. Auth0 says to move to Actions.

      
      
     
    
      
      AWS will end support for the Amazon Q Developer IDE plugins on April 30, 2027, and points IDE users to Kiro.

      
      
        
        
      

     
    
      
      Heroku keeps running. Heroku-22, based on Ubuntu 22.04, reaches end of life on April 30, 2027. After that date, Heroku-22 apps receive no security updates. From May 1, 2027, Heroku-22 apps can no longer be built or deployed, including Review Apps and Heroku CI, and no new Heroku-22 apps can be created, but existing apps keep running. Since November 1, 2026, the first build of each Heroku-22 app in a 30-day period fails with a deprecation error; retrying it deploys as normal. This changes to the first build in each 7-day period from February 1, 2027 and in each 24-hour period from April 1, 2027. Newer stacks are Heroku-24 and Heroku-26.

      
      
     
    
      
      The Gemini API keeps running. Gemini 3.1 Flash-Lite (gemini-3.1-flash-lite) can be called until May 7, 2027, when Google shuts it down. Google names gemini-3.5-flash-lite as the replacement.

      
      
     
    
      
      Auth0 keeps running. Auth0's grace period for the Node.js 18 extensibility runtime ends on May 10, 2027. This deprecation covers Actions, Rules, Hooks, Custom Database Connections, and Custom Social Connections. Before the grace period ends, Auth0 says to change the tenant's default Runtime setting and the runtime of every Action still running on Node.js 18. After the grace period, Auth0 begins the end-of-life phase for the Node.js 18 runtime, which ends with its removal, and code still targeting it will eventually be forced to run on a newer runtime.

      
      
     
    
      
      Azure Fluid Relay retires on June 2, 2027. It can be used until June 1, 2027. The Fluid Framework remains open source. Replace it with a self-hosted Fluid service you run yourself.

      
      
     
    
      
      Azure OpenAI retires gpt-image-1.5 version 2025-12-16 on June 16, 2027. Microsoft names no replacement. After that date requests to it return HTTP 410 Gone. OpenAI's own API retires gpt-image-1.5 earlier, on December 1, and that is OpenAI's date, not Azure's.

      
      
     
    
      
      Microsoft is retiring the Microsoft To Do apps on the web, Windows, Mac, iOS and Android on June 30, 2027, for work or school accounts and for personal Microsoft accounts. Work or school accounts move to Microsoft Planner, but existing To Do tasks and lists do not migrate to it. Planner is not available for personal Microsoft accounts. Microsoft says it will provide a way to export To Do data before the apps retire. The To Do service and its APIs retire later, on dates Microsoft has not announced.

      
      
        
        
      

     
    
      
      Google Cloud Deployment Manager is turned down after June 30, 2027. Support ended April 1, 2026. Since June 30, 2026, new users cannot enable the Deployment Manager API or create a first deployment. Existing deployments can use Deployment Manager until June 30, 2027. After that date, Deployment Manager can no longer be used. Resources it created continue to run and are managed with standard Google Cloud tools. Google recommends Infrastructure Manager.

      
      
        
        
      

     
    
      
      Deprecated 2026-04-30.

      
        Who’s affected: All AWS Lambda functions running on the nodejs20.x runtime

        Impact: Lambda blocks creating functions on nodejs20.x from 2027-07-29 and updating them from 2027-08-31. Functions keep running; AWS never blocks invocations.

        Migration path: Upgrade to Node.js 24 (nodejs24.x), which AWS deprecates on 2028-04-30, or Node.js 26 (nodejs26.x), which has no deprecation date yet. Lambda blocks Node.js 22 (nodejs22.x) on the same dates as nodejs20.x: creating functions from 2027-07-29 and updating them from 2027-08-31. Test for breaking changes between Node.js versions.

       
      
     
    
      
      AWS Lambda blocks creating new functions on its deprecated runtimes from July 29, 2027. It blocks updating the code and configuration of existing functions from August 31, 2027. AWS is delaying these blocks beyond its usual 30 and 60 days after deprecation. Functions on .NET 8, Python 3.10 and the other deprecated runtimes can still be created and updated until those dates through the AWS CLI, AWS SAM or CloudFormation, not the Lambda console. After both dates, existing functions can still be invoked and can still be switched to a supported runtime. Deprecated runtimes include Node.js 16, 18 and 20; Python 3.8, 3.9 and 3.10; Ruby 3.2; .NET 6 and 8; and provided.al2. Python 3.10 was deprecated on October 31, 2026 and .NET 8 on November 10, 2026. These runtimes are deprecated next and get the same two dates: Ruby 3.3 on March 31, 2027, Node.js 22 on May 30, 2027, and Python 3.11 and Java 8, 11 and 17 on Amazon Linux 2 on June 30, 2027. Older runtimes are the exception: functions on Node.js 14, Python 3.7, Ruby 2.7, Go 1.x, Java 8 on Amazon Linux and provided can no longer be created since 2024 and can no longer be updated from August 31, 2027.

      
      
     
    
      
      New Relic keeps running. On July 31, 2027, New Relic retires its REST API v2 (api.newrelic.com/v2 and api.eu.newrelic.com/v2) and its Deployments v0 API (deployments.xml), so integrations, scripts and tools that call them stop working. The Alerts violations, incidents, channels and events endpoints also end, while the Alerts policies and conditions endpoints stay. The NerdGraph GraphQL API and data ingest from supported agents are not affected. NerdGraph replaces REST API v2 and deployment markers move to its change tracking API.

      
      
     
    
      
      Google Cloud Video Intelligence API shuts down on September 14, 2027. It has been deprecated and unsupported since September 14, 2026, and can be used until its shutdown. Google recommends migrating to its Gemini family of models.

      
      
        
        
      

     
    
      
      Functions on Azure Container Apps V1 retires on September 29, 2027. After that date V1 function apps stop running and stop processing requests and triggers. Migrate to Functions on Azure Container Apps V2, a native Container App with kind=functionapp, using the same container image and no function code changes in the standard path. Azure Functions and Azure Container Apps keep running.

      
      
     
    
      
      Amazon DevOps Guru shuts down on September 30, 2027. New customers have not been able to sign up since October 29, 2026. Existing customers can use it until September 30, 2027. After that date, the console and API become unavailable, past insights are no longer retrievable, and stacks declaring AWS::DevOpsGuru::ResourceCollection or AWS::DevOpsGuru::NotificationChannel fail to create or update. Running workloads are not interrupted. AWS recommends Amazon CloudWatch and Amazon DevOps Agent.

      
      
        
        
      

     
    
      
      Firebase keeps running. Firebase Test Lab can be used until September 30, 2027. After that date, no tests can run. Google's replacement is the Developer Device Platform on Google Cloud. The Developer Device Platform requires billing. The no-cost Spark plan includes 10 virtual-device tests a day and 5 physical-device tests a day.

      
      
     
    
      
      Smartlook shuts down on 2027-09-30.

      
      
        
        
      

     
    
      
      Twilio keeps running. Twilio Notify reaches end of life on October 31, 2027 for existing customers. Twilio stopped selling Notify in 2022, so new customers cannot use it. Twilio has said requests made to the Notify API will fail after its end of life, and has moved the date four times since 2022. Push notifications sent through Twilio Verify, Voice, Conversations or Engage are not affected.

      
      
        
        
      

     
    
      
      After November 7, 2027, Amazon OpenSearch Service isolates every domain still running Elasticsearch 1.5, 2.3, 5.1, 5.3, 5.5, 6.0, 6.2, 6.3, 6.4, 6.5, 6.7, 7.1, 7.4, 7.7 or 7.8, or OpenSearch 1.0, 1.1, 1.2, 2.3, 2.5, 2.7 or 2.9. Isolated domains can no longer be accessed. AWS keeps their data for at least 30 days. Data must be restored into a new domain on a supported version. New domains cannot be created on these versions after that date. Since November 7, 2026, these domains have paid an Extended Support surcharge equal to their instance price, doubling the cost. Elasticsearch 1.5 and 2.3 domains cannot be upgraded in place. Upgrading before November 7, 2027 stops the surcharge and avoids isolation.

      
      
     
    
      
      Auth0 keeps running. Auth0 Rules reach end of life on January 31, 2028, when they stop running and are removed. They have been read-only since November 18, 2024, and tenants created from October 16, 2023 never had them. Auth0 says to move to Actions.

      
      
     
    
      
      The Gemini API keeps running. The embedding model gemini-embedding-001 can be called until May 14, 2028, when Google shuts it down. Google names gemini-embedding-2 as the replacement.

      
      
     
    
      
      Microsoft Dev Box retires on September 18, 2028. Its closing-down period began on September 14, 2026. It is in maintenance mode with no new features planned. Microsoft recommends Windows 365 as the replacement.

      
      
        
        
      

     
    
      
      Microsoft retires the Basic, Standard and Premium tiers of Azure Cache for Redis on September 30, 2028. From October 1, 2028 every remaining Basic, Standard or Premium cache is disabled. Since April 1, 2026 new customers in Azure's public cloud cannot create caches in these tiers. A tenant that had any Azure Cache for Redis instance before April 1, 2026 can keep creating them until the retirement. Reservations are supported until September 30, 2028. Microsoft's replacement is Azure Managed Redis. Applications must switch to the new cache's hostname and access key. Azure Managed Redis is clustered by default, so the client library must be set up for a clustered cache.

      
      
     
    
      
      Azure Communication Services' standalone services retire on September 30, 2028. Retired services include Email, SMS, WhatsApp messaging, Chat, Rooms, number management, Job Router, Direct Routing and the UI Library SDKs. Customers without an ACS resource created before October 23, 2026 cannot acquire phone numbers for SMS or PSTN calling. Voice and video calling stay available after 2028 only with Microsoft Teams integrations, on new SDK major versions.

      
      
     
  
    ## Completed (26)

    These shutdowns have already happened. Listed for reference.

    
      
      On Gemini Enterprise Agent Platform (formerly Vertex AI), Google retired gemini-2.5-pro, gemini-2.5-flash and gemini-2.5-flash-lite on October 20, 2026. Google names gemini-3.8-flash as a replacement for all three. On the Gemini API, Google shuts down gemini-2.5-pro and gemini-2.5-flash on December 1, 2026. gemini-2.5-flash-lite has no shutdown date on the Gemini API.

      
      
     
    
      
      NVIDIA's free API endpoints keep running. NVIDIA Nemotron 3 Super (nvidia/nemotron-3-super-120b-a12b) can be called on NVIDIA's free endpoints until October 19, 2026; NVIDIA's model page says it will no longer be supported after 10/19/2026.

      
      
        
        
      

     
    
      
      ChatGPT and Codex keep running. Only the GPT-5.5 model retires from ChatGPT, ChatGPT Work and Codex on all plans on 2026-10-14. For Codex with ChatGPT sign-in, OpenAI names GPT-6.1 Sol as the replacement on Plus, Pro, Business, Enterprise and Edu plans (its page named GPT-6 Sol when we read it on 2026-10-06) and GPT-6 Luna on Free and Go plans. The OpenAI API is not affected.

      
      
        
        
      

     
    
      
      Complete service shutdown — infrastructure-as-code provisioning service being retired

      
        Who’s affected: Platform engineering teams using Proton for developer self-service infrastructure provisioning

        Impact: The Proton console and Proton resources are no longer accessible after 2026-10-07. Deployed CloudFormation stacks and the resources they manage keep running.

        Migration path: Migrate to CloudFormation Git Sync, Harmonix on AWS, CodePipeline with CodeBuild, or GitHub Actions

       
      
     
    
      
      grok-voice-transcribe-1.0 reached end of life on 2026-10-02; its requests are served by grok-voice-transcribe-2.0 at the same price.

      
      
        
        
      

     
    
      
      Complete service shutdown — App Mesh service mesh for ECS/EKS being retired

      
        Who’s affected: Teams using AWS App Mesh for microservice networking on ECS or EKS

        Impact: After September 30, 2026, you will no longer be able to access the AWS App Mesh console or AWS App Mesh resources.

        Migration path: Migrate to Amazon ECS Service Connect, Istio, Linkerd, or Amazon EKS native service mesh capabilities

       
      
     
    
      
      gpt-3.5-turbo-instruct, babbage-002, davinci-002 and gpt-3.5-turbo-1106 shut down in the OpenAI API on September 28, 2026. Fine-tuned babbage-002 and davinci-002 models (ft-babbage-002, ft-davinci-002) are listed separately, for October 23, 2026.

      
        Who’s affected: Developers calling these model IDs. gpt-3.5-turbo-instruct, babbage-002 and davinci-002 run on the legacy Completions endpoint (/v1/completions).

        Impact: Calls to these model IDs stop working.

        Migration path: OpenAI names gpt-5.6-terra as the replacement for all four. gpt-5.6-terra does not support /v1/completions, so calls on that endpoint must move to the Chat Completions or Responses API.

       
      
     
    
      
      Videos API (Sora) deprecated March 24, 2026 — complete removal September 24, 2026. No direct OpenAI replacement for video generation.

      
        Who’s affected: Developers using the Videos API (POST /v1/videos and the other /v1/videos endpoints) and the sora-2 and sora-2-pro models, including their dated snapshots.

        Impact: Calls to the Videos API and the Sora 2 models no longer work. OpenAI says there is no one-to-one replacement API.

        Migration path: No direct OpenAI replacement — evaluate third-party video generation APIs (Runway, Pika, Luma)

       
      
     
    
      
      Complete API shutdown — Assistants, Threads, Runs, and Messages endpoints removed

      
        Who’s affected: AI agent builders using Assistants API for threads, code interpreter, file search, and persistent assistants

        Impact: The Assistants API shut down. The call that retrieves thread messages no longer works.

        Migration path: OpenAI names the Responses API and the Conversations API as replacements. Its migration guide turns assistants into reusable prompts, but the v1/prompts API and reusable prompt objects are scheduled to shut down November 30, 2026; OpenAI's advice for prompts is to move their content into your application code.

       
      
     
    
      
      The service will shut down on 10th August 2026.

      
      
        
        
      

     
    
      
      GitHub retired GitHub Models on 2026-07-30, so there is no free tier.

      
      
        
        
      

     
    
      
      No free tier since 2026-07-30, when Phase Two shut down its free shared realms.

      
      
        
        
      

     
    
      
      To call the API today, use deepseek-flash or deepseek-v4-pro: DeepSeek retired the older model names deepseek-chat and deepseek-reasoner on 2026-07-24 (its change log of 2026-04-24 says they "will be discontinued in three months (2026-07-24)").

      
      
        
        
      

     
    
      
      Google shut down the Tenor API on 2026-06-30; new API keys stopped on 2026-01-13. Only the API ended: Tenor.com, the GIF Keyboard apps and Gboard still serve Tenor GIFs. Every Tenor API request fails with an error after 2026-06-30, so apps, bots and sites that searched GIFs through it need another source.

      
        Migration path: Migrate to Klipy or GIPHY; both offer Tenor-compatible v2 endpoints

       
      
     
    
      
      End of support — tasks on Platform Version 1.3.0 will stop running — ECS Fargate tasks on PV 1.3.0 will fail to launch

      
        Migration path: Update task definition to Platform Version 1.4.0+ — test for networking and storage behavior changes

       
      
     
    
      
      New workspace creation and user signup for Firebase Studio are disabled. Existing workspaces keep working until March 22, 2027.

      
        Migration path: Google recommends migrating existing workspaces to Google AI Studio or Google Antigravity. GitHub Codespaces (120 free core hours a month on a GitHub Free personal account) and Replit's free Starter plan are other options.

       
      
     
    
      
      Gemini 2.0 Flash and 2.0 Flash-Lite shut down on June 1, 2026. Image generation via 2.0 Flash shut down November 14, 2025. Calls to the gemini-2.0-flash and gemini-2.0-flash-lite model IDs no longer work.

      
        Migration path: Google recommends gemini-3.6-flash for 2.0 Flash and gemini-3.1-flash-lite for 2.0 Flash-Lite.

       
      
     
    
      
      Google Maps Platform client IDs have been deprecated since May 26, 2025. Google says they can no longer be used after May 31, 2026. Since May 2026, Google has restricted access periodically as a phased deprecation, and it has published no final termination date.

      
        Migration path: Switch to API key authentication — generate key in Google Cloud Console, update all API calls Client ID requests fail during Google's periodic restrictions, which Google says will increase in frequency and duration. Service usually returns within a few hours; project Owners or Editors can restore it sooner by unpausing the client ID in the Cloud Console, and developers who cannot migrate quickly can ask Google Maps Platform Support for a temporary exemption.

       
      
        
        
      

     
    
      
      Mistral retired these models in 2026, and a request to a retired model fails with a 404 error: Mistral Large 2.1 (mistral-large-2411), Pixtral Large (pixtral-large-2411) and Devstral Medium 1.0 (devstral-medium-2507) on 2026-05-31, replaced by Mistral Medium 3.5; Devstral Small 1.1 (devstral-small-2507) on 2026-05-31, replaced by Mistral Small 4; Voxtral Mini and Voxtral Mini Transcribe (voxtral-mini-2507) on 2026-05-31, replaced by Voxtral Mini Transcribe 2; OCR 2 (mistral-ocr-2505) on 2026-05-31 and OCR 4.0 (mistral-ocr-4-0) on 2026-09-30, replaced by OCR 4.1; Mistral Moderation (mistral-moderation-2411) on 2026-06-30, replaced by Mistral Moderation 2; Mistral Small 3.2 (mistral-small-2506) and Magistral Small 1.2 (magistral-small-2509) on 2026-07-31, replaced by Mistral Small 4; Mistral Nemo 12B (open-mistral-nemo-2407) on 2026-07-31, replaced by Ministral 3 8B; Devstral 2 (devstral-2512) and Magistral Medium 1.2 (magistral-medium-2509) on 2026-07-31, and Mistral Medium 3 and 3.1 (mistral-medium-2505, mistral-medium-2508) on 2026-08-31, replaced by Mistral Medium 3.5.

      
      
        
        
      

     
    
      
      On 2026-05-15 xAI retired eight model names, which still resolve, so code that calls them keeps working: requests to Grok 4.1 Fast (grok-4-1-fast-reasoning, grok-4-1-fast-non-reasoning), Grok 4 Fast (grok-4-fast-reasoning, grok-4-fast-non-reasoning), grok-4-0709 and grok-3 are served by grok-4.3 (low reasoning effort for the reasoning names and grok-4-0709, none for the non-reasoning names and grok-3) and billed at grok-4.3 rates; grok-imagine-image-pro is served by grok-imagine-image-2.0 at low quality; and xAI's retirement page routes grok-code-fast-1 to grok-build-0.1, which xAI has since removed from its pricing and models pages without a notice and OpenRouter no longer offers.

      
      
        
        
      

     
    
      
      The Realtime API Beta (requests with the OpenAI-Beta: realtime=v1 header) was removed from the API on May 12, 2026. The GA Realtime API was not affected.

      
        Migration path: Move to the GA Realtime API: remove the OpenAI-Beta: realtime=v1 header, create ephemeral credentials for browser or mobile clients with POST /v1/realtime/client_secrets, use /v1/realtime/calls for WebRTC sessions, set session.type, move output audio settings under session.audio.output, and use the GA response event names, such as response.output_text.delta, response.output_audio.delta and response.output_audio_transcript.delta.

       
      
     
    
      
      dall-e-2 and dall-e-3 removed from the API — OpenAI lists gpt-image-2, gpt-image-1 or gpt-image-1-mini as substitutes, and gpt-image-1 itself shuts down on 2026-10-23 (gpt-image-1-mini on 2026-12-01) — Developers calling dall-e-2 or dall-e-3, including image generation calls (POST /v1/images/generations) that leave out the model parameter, which OpenAI's reference says default to dall-e-2 unless a parameter specific to the GPT image models is used, and any use of the image variations endpoint, which supports only dall-e-2.

      
        Migration path: OpenAI's substitutes are gpt-image-2, gpt-image-1 or gpt-image-1-mini, and its DALL·E model pages now recommend GPT-Image-2.5 Sunburst. The GPT image models return base64-encoded images only (response_format is not supported), do not take style, and take quality low, medium or high instead of standard or hd; GPT-Image-2.5 Sunburst also takes xhigh and max. OpenAI may require API Organization Verification.

       
      
     
    
      
      Existing customers can keep using it as normal, including creating new services. App Runner closed to new customers — existing users can continue, but no new features. AWS recommends ECS Express Mode. AWS lists it as in maintenance, with no new features.

      
        Migration path: Migrate to ECS Express Mode (AWS recommended), Google Cloud Run, Railway, Render, or Fly.io

       
      
     
    
      
      Legacy Contact Lists API v1 sunset — replaced by Lists API v3 — Contact Lists API v1 endpoints return HTTP 404. HubSpot's changelog said three endpoints listing all, recently updated or recently created contacts would keep working, but its current migration guide lists them as returning 404 too. Six Contacts API v1 read endpoints (a contact or a batch of contacts by visitor ID, email address or user token) still work but no longer return list memberships.

      
        Migration path: Move to the Lists v3 API or a date-versioned Lists API (/crm/lists/2026-03 in HubSpot's migration guide; 2026-09 is the latest version). Map each v1 list ID (legacyListId) to its listId first: HubSpot warns that a v1 ID used on the new endpoints may update or delete the wrong list.

       
      
        
        
      

     
    
      
      The Node.js 20 Lambda runtime (nodejs20.x) was deprecated on 2026-04-30. Lambda blocks creating nodejs20.x functions from 2027-07-29 and updating them from 2027-08-31; AWS's runtime page says "you can continue to invoke your functions indefinitely".

      
      
     
    
      
      No free tier: Qwen Code's documentation says "The Qwen OAuth free tier was discontinued on 2026-04-15" and points users to Alibaba Cloud Coding Plan, OpenRouter, Fireworks AI or another provider.

      
      
        
        
      

     
  
  ## Related Pricing Changes

  Recent pricing and policy changes from vendors on our shutdown list — from our deal changes database.

  | Date | Vendor | Change | Impact | 
|---|---|---|---|
    
      
          | effective Nov 7, 2026 | AWS | Amazon OpenSearch Service charges an Extended Support surcharge equal to the instance price on every domain still running Elasticsearch 1.5, 2.3, 5.1, 5.3, 5.5, 6.0, 6.2, 6.3, 6.4, 6.5, 6.7, 7.1, 7.4, 7.7 or 7.8, or OpenSearch 1.0, 1.1, 1.2, 2.3, 2.5, 2.7 or 2.9 from November 7, 2026. The surcharge doubles the instance cost. Storage costs are not affected. Before November 7, 2026 these OpenSearch Service domains paid $0.0065 per Normalized Instance Hour (NIH) in US East (N. Virginia). Amazon OpenSearch Service's example: an m7g.medium.search instance at $0.068 an hour on demand costs $1.632 a day. The surcharge adds another $1.632 a day. The surcharge applies automatically and stops when the domain is upgraded to a supported version. This rate applies until November 7, 2027. Elasticsearch 5.6 domains still pay $0.0065 per NIH. Elasticsearch 6.8, 7.9, 7.10 and OpenSearch 1.3, 2.11, 2.13, 2.15, 2.17 and 2.19 pay no surcharge before November 7, 2027. Source ↗ | MEDIUM | 
      
          | effective Oct 23, 2026 | OpenAI | The OpenAI API keeps running. The gpt-3.5-turbo, gpt-4, gpt-4-turbo, gpt-4-1106-preview, gpt-4.1-nano, gpt-4o-2024-05-13, gpt-image-1, o1, o1-pro, o3-mini and o4-mini models, with their fine-tuned versions, can be called until October 23, 2026, when OpenAI shuts down their access. OpenAI names a substitute for each model: gpt-3.5-turbo to gpt-5.6-terra; gpt-4, gpt-4-turbo, gpt-4-1106-preview and gpt-4o-2024-05-13 to gpt-5.6-sol; gpt-4.1-nano to gpt-5.6-luna; gpt-image-1 to gpt-image-2.5-sunburst or gpt-image-2.5-flare; o1 to gpt-5.6-sol; o1-pro to gpt-5.6-sol with reasoning.mode: pro; o3-mini to gpt-5.6-sol; o4-mini to gpt-5.6-terra. Source ↗ | MEDIUM | 
      
          | effective Oct 23, 2026 | Google Gemini API | The Gemini API keeps running. The Deep Research agent preview deep-research-pro-preview-12-2025 can be called until October 23, 2026, when Google shuts it down. Google names deep-research-preview-04-2026 as the replacement. Source ↗ | LOW | 
      
          | effective Oct 22, 2026 | Google Gemini API | The Gemini API keeps running. Gemini Omni Flash Preview (gemini-omni-flash-preview) and the Veo 3.1 preview models (veo-3.1-generate-preview, veo-3.1-fast-generate-preview and veo-3.1-lite-generate-preview) can be called until October 22, 2026, when Google shuts them down. Google names gemini-omni-1.1-flash as the replacement for all four. Source ↗ | LOW | 
      
          | effective Oct 20, 2026 | Google Gemini API | On Gemini Enterprise Agent Platform (formerly Vertex AI), Google retired gemini-2.5-pro, gemini-2.5-flash and gemini-2.5-flash-lite on October 20, 2026. Google names gemini-3.8-flash as a replacement for all three. On the Gemini API, Google shuts down gemini-2.5-pro and gemini-2.5-flash on December 1, 2026. gemini-2.5-flash-lite has no shutdown date on the Gemini API. Source ↗ | MEDIUM | 
      
          | effective Oct 7, 2026 | AWS | AWS Proton reaches end of support on 2026-10-07, when its console and resources stop being accessible. AWS: "Your deployed CloudFormation stacks and the resources they manage will remain intact and continue to function." New customers could not sign up after 2025-10-07. Source ↗ | MEDIUM | 
      
          | effective Sep 30, 2026 | AWS | AWS App Mesh reached end of support on 2026-09-30. AWS: "After September 30, 2026, you will no longer be able to access the AWS App Mesh console or AWS App Mesh resources." AWS points App Mesh users to Amazon ECS Service Connect. Source ↗ | MEDIUM | 
      
          | effective Sep 28, 2026 | OpenAI | gpt-3.5-turbo-instruct, babbage-002, davinci-002 and gpt-3.5-turbo-1106 shut down in the OpenAI API on September 28, 2026. OpenAI names gpt-5.6-terra as the replacement. Source ↗ | LOW | 
      
          | effective Sep 24, 2026 | OpenAI | Sora 2 video generation in the OpenAI API shut down on September 24, 2026: OpenAI removed the Videos API and the sora-2 and sora-2-pro models, and names no replacement. Source ↗ | MEDIUM | 
      
          | effective Sep 18, 2026 | Google Gemini API | Since 2026-09-18 Google serves the Gemini 2.5 models only to users who have used them before, and points new projects to 3.5 Flash-Lite or 3.8 Flash. Source ↗ | MEDIUM | 
    
  

  
  ## Upcoming Changes

  These are announced changes that have not taken effect yet, listed soonest first.

  | Date | Vendor | Change | Impact | 
|---|---|---|---|
    
      
          | effective Nov 30, 2026 | OpenAI | The OpenAI API keeps running. Agent Builder, the Evals platform and the v1/prompts API can be used until November 30, 2026. Existing evals become read-only on October 31, 2026. ChatKit remains available. Migrate from Agent Builder to the Agents SDK or ChatGPT Workspace Agents. Migrate from Evals to Promptfoo. Move reusable prompt content into application code. Source ↗ | LOW | 
      
          | effective Nov 30, 2026 | Solo | Solo can be used until November 30, 2026, when all Solo websites and data are deleted, with no way to recover a site, account or data afterwards. Solo announced on 2026-10-01 that it is shutting down. Solo sends each site's owner a ZIP with an HTML export of the site, a CSV of links to its images, and its contact form and newsletter submissions; image source files are not in the ZIP and must be downloaded one by one. Pro and Grow subscriptions get a prorated refund calculated as of October 1, 2026, and do not renew. Source ↗ | HIGH | 
      
          | effective Dec 1, 2026 | OpenAI | The OpenAI API keeps running. The gpt-image-1-mini, gpt-image-1.5 and chatgpt-image-latest models can be called until December 1, 2026. OpenAI's substitutes are gpt-image-2.6-sunburst or gpt-image-2.6-flare. Source ↗ | LOW | 
      
          | effective Dec 1, 2026 | OpenAI | OpenAI's promotional pricing for gpt-5.6-sol on the OpenAI API ends on November 30, 2026. Until then, gpt-5.6-sol costs $4.00 per million input tokens, $0.40 per million cached input tokens and $20.00 per million output tokens on Standard processing, and $2.00, $0.20 and $10.00 on Flex and Batch. OpenAI has not said what gpt-5.6-sol costs from December 1, 2026. OpenAI's note covers gpt-5.6-sol on the OpenAI API only and says nothing about Azure OpenAI. Source ↗ | MEDIUM | 
      
          | effective Dec 1, 2026 | Prisma Accelerate | Prisma stopped offering Accelerate for new setups in its Console by 2026-10-02, and Accelerate, including Prisma Postgres connections that go through it, is retired on 2026-12-01. Existing Accelerate connections keep working until then. Prisma Postgres keeps running, and its databases come with connection pooling. Source ↗ | HIGH | 
      
          | effective Dec 1, 2026 | Google Gemini API | Google shuts down Gemini 2.5 Pro (gemini-2.5-pro) and Gemini 2.5 Flash (gemini-2.5-flash) on the Gemini API on December 1, 2026. Both can be used until that date. After the shutdown, requests that still name gemini-2.5-pro are answered by a different model, gemini-3.8-flash, billed at Gemini 3.8 Flash rates, and requests that still name gemini-2.5-flash are answered by gemini-3.5-flash-lite, billed at Gemini 3.5 Flash-Lite rates. Google deprecated both on 2026-11-02 and lists breaking API changes in a migration guide for each replacement. Gemini 2.5 Flash-Lite (gemini-2.5-flash-lite) is not deprecated. The Gemini API keeps running. On Gemini Enterprise Agent Platform (formerly Vertex AI), Google already retired gemini-2.5-pro, gemini-2.5-flash and gemini-2.5-flash-lite on October 20. Source ↗ | MEDIUM | 
      
          | effective Dec 1, 2026 | Google Gemini API | Gemini 4 Argon on the Gemini API has introductory pricing through November 30, 2026. Standard input is $2.00 per million tokens through November 30, 2026, then $4.00 from December 1, 2026. Output is $10.00 then $20.00. Batch and Flex input is $1.00 then $2.00, output $5.00 then $10.00. Priority input is $3.60 then $7.20, output $18.00 then $36.00. It is not available on the Free tier. Source ↗ | MEDIUM | 
      
          | effective Dec 1, 2026 | Firebase | Firebase Spark projects that enabled Remote Config before September 1, 2026 can use it under the old terms until December 1, 2026. From December 1, 2026, the 100,000-fetches-a-day limit applies to them, with 30 days of full service from the first day they go over, then throttling unless they upgrade to Blaze. A Spark project that upgrades to Blaze before November 15, 2026 is not billed for Remote Config until February 1, 2027. Blaze projects from before September 1, 2026 start Remote Config billing on February 1, 2027. Source ↗ | MEDIUM | 
      
          | effective Dec 1, 2026 | Google Gemini API | Gemini 4 Carbon (gemini-4-carbon), generally available on the Gemini API since November 10, 2026, has introductory pricing through November 30, 2026. Standard input is $2.00 per million tokens through November 30, 2026, then $4.00 from December 1, 2026. Output is $10.00 then $20.00. Batch and Flex input is $1.00 then $2.00, output $5.00 then $10.00. Priority input is $3.60 then $7.20, output $18.00 then $36.00. It is not available on the Free tier. Source ↗ | MEDIUM | 
      
          | effective Dec 3, 2026 | Google Gemini API | Google shuts down Gemini Omni 1.1 Flash (gemini-omni-1.1-flash) on the Gemini API on December 3, 2026. It can be used until that date. After the shutdown, requests that still name gemini-omni-1.1-flash are answered by a different model, gemini-omni-1.2-flash, billed at Gemini Omni 1.2 Flash rates. Google released Gemini Omni 1.2 Flash on 2026-11-05. The Gemini API keeps running. Source ↗ | LOW | 
      
          | effective Dec 4, 2026 | HubSpot | HubSpot sunsets Pipelines API v1 on December 4, 2026. After that date, every call to v1 endpoints stops working. Migrate to Pipelines API 2026-09 or any 2026-03 or newer version. Source ↗ | MEDIUM | 
      
          | effective Dec 11, 2026 | OpenAI | The OpenAI API keeps running. gpt-5 and gpt-5-2025-08-07 are available until December 11, 2026, replaced by gpt-6.1-sol. gpt-5-mini and gpt-5-mini-2025-08-07 are available until December 11, 2026, replaced by gpt-6.1-terra. gpt-5-nano and gpt-5-nano-2025-08-07 are available until December 11, 2026, replaced by gpt-6-luna. gpt-5-pro and gpt-5-pro-2025-10-06 are available until December 11, 2026, replaced by gpt-6.1-sol with reasoning.mode: pro in the Responses API. o3 and o3-2025-04-16 are available until December 11, 2026, replaced by gpt-6.1-sol. o3-pro and o3-pro-2025-06-10 are available until December 11, 2026, replaced by gpt-6.1-sol with reasoning.mode: pro in the Responses API. o3-mini is a different model from o3, and OpenAI shut it down on October 23. Source ↗ | MEDIUM | 
      
          | effective Dec 13, 2026 | Google Gemini API | gemini-live-2.5-flash-native-audio can be called on Gemini Enterprise Agent Platform (formerly Vertex AI) until December 13, 2026, when Google retires it. The December 13 date does not apply to the Gemini API, where gemini-2.5-flash-native-audio-preview-12-2025 keeps running until March 31. Google names gemini-3.8-live as the replacement. Source ↗ | LOW | 
      
          | effective Dec 18, 2026 | Google Gemini API | The Gemini API keeps running. The text-to-speech preview models gemini-2.5-flash-preview-tts, gemini-2.5-pro-preview-tts and gemini-3.1-flash-tts-preview can be called until December 18, 2026, when Google shuts them down. Google names gemini-3.8-flash-tts or gemini-3.8-flash-lite-tts as the replacement. Google moved this shutdown from an earlier date in November. Source ↗ | LOW | 
      
          | discovered Dec 31, 2026 · effective date unknown | HiveMQ | HiveMQ no longer lets anyone create Serverless clusters and retires the Serverless plan (Free and Pay As You Go) on 2026-12-31. Existing clusters run until then. After that date, clients cannot connect and HiveMQ permanently deletes the clusters and their data. Source ↗ | HIGH | 
      
          | effective Jan 1, 2027 | Google Gemini API | Gemini 3.8 Flash and Gemini 3.6 Flash on the Gemini API have introductory pricing through December 31, 2026 on Google AI Studio and Gemini Enterprise Agent Platform. Standard input is $0.75 per million tokens through December 31, 2026, then $1.50 from January 1, 2027. Output is $3.75 then $7.50. Batch and Flex input is $0.375 then $0.75, output $1.875 then $3.75. Priority input is $1.35 then $2.70, output $6.75 then $13.50. The Free tier stays free of charge. Source ↗ | MEDIUM | 
      
          | effective Jan 1, 2027 | AWS | AWS discontinues its Developer Support and Business Support plans on January 1, 2027. Accounts on either plan must opt in to Business Support+ before then. A Developer Support account that does not opt in is downgraded to Basic Support. Basic Support gives access to the Resource Center, Service Health Dashboard, Product FAQs and Discussion Forums at no charge. Business Support accounts are not moved to Business Support+ automatically. Business Support+ costs the greater of $29 a month per account or 9% of monthly AWS charges, falling in steps to 3% on larger bills. Neither plan has accepted new subscriptions since December 2, 2025. AWS emails each account 30 days before its plan changes. Enterprise On-Ramp also ends on January 1, 2027, and its customers are moved to Enterprise Support automatically. The three plans stay available in the AWS GovCloud (US) Region. Source ↗ | MEDIUM | 
