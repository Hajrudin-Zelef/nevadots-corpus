---
id: collect-261121-veille-ia/sora-openai/when-will-sora-shut-down-timeline-and-alternatives
title: "When Will Sora Shut Down? Timeline and Alternatives"
domain: sora-openai
role: reference
task: article
actors: []
dates: ["2026-04-26"]
keywords: ["sora", "openai", "export", "help", "page", "video", "down", "shut"]
source: https://www.layer3labs.io/guides/is-sora-shutting-down
source_anchor: ""
source_lines: [1, 112]
section: "Sora_OpenAI"
---
# When Did Sora Shut Down?

The dated timeline for the Sora app and API, plus the export route OpenAI's help page describes.

Sora has already shut down, with OpenAI closing the consumer web and app interface on April 26, 2026, and turning off the Application Programming Interface (API) on September 24, 2026. Teams looking for generation access cannot purchase credits or submit new rendering jobs through any official OpenAI channel today.

The discontinuation leaves automated media pipelines and production teams needing direct replacements for prompt-driven video generation. OpenAI announced the discontinuation on March 24, 2026, six months before the API shutdown.

At Layer3Labs, we build workflow automation for small and mid-sized businesses, and a discontinued model API is one of the few changes that stops a working pipeline outright.

## Sora Shutdown Timeline and Current Operational Status

As of September 27, 2026, there is no way to generate new video through Sora or Sora 2, by app or API. OpenAI closed the Sora web and app experiences on April 26, 2026, and the Sora API on September 24, 2026.

NewsBytes reported the cutoff dates at the time.

If you still need new video for a production schedule, send those requests to one of the replacement models priced below.

- March 24, 2026: Official public announcement of the Sora deprecation schedule
- April 26, 2026: Closure of the consumer web platform and application interfaces
- September 24, 2026: Permanent shutdown of developer API access

## Operational Impact Across Application and API Shutdown Dates

The discontinuation took effect across two distinct operational milestones that separated consumer interfaces from developer endpoints. On April 26, 2026, OpenAI discontinued the Sora web and app experiences, so no one could generate Sora video there after that date.

On September 24, 2026, OpenAI discontinued the Sora API. Any request to it now fails, whether it comes from a script, a scheduled job or a third-party integration.

Teams with scheduled cron jobs or serverless functions invoking Sora endpoints must update their integration logic to prevent unhandled service exceptions.

## Data Retention and Video Export Procedures

OpenAI established an export mechanism allowing account holders to recover previously generated video assets before permanent deletion. According to the official OpenAI help center article on Sora discontinuation, users can request their archive by visiting the sunset portal at sora.chatgpt.com/sunset and clicking the Export option. The help page says OpenAI emails you when the export is ready.

The help page says that if OpenAI offers a final export window, users are notified by email. Following the conclusion of any final export window, OpenAI will permanently delete all stored data associated with Sora.

You should verify your current export status directly on OpenAI's help page rather than assuming automated access remains open. Once files are retrieved locally, ensure your team backs up the raw video files to dedicated internal cloud storage.

- Check OpenAI's help page to see whether the export route at sora.chatgpt.com/sunset is open
- Trigger the export request and monitor your registered email for completion notices
- Download all asset archives directly to secure internal storage

## Subscription Refunds and Unused Credit Allocations

OpenAI's help page points refund requests to the standard ChatGPT subscription refund instructions.

For organizations holding purchased balances, OpenAI confirmed that credits bought for ChatGPT and Sora remain usable for Codex. This policy allows teams to repurpose remaining prepayments toward software development and code completion rather than forfeiting unspent funds. If you have no use for Codex, read OpenAI's help page for the refund route it describes.

Last published rates for Sora 2 before pricing was taken down, as reported by tracking sites like CostGoat, listed Sora 2 Standard at $0.10 per second at 720p resolution. The Pro tier was reported at $0.30 per second at 720p, $0.50 per second at 1024p (1792 by 1024), and $0.70 per second at 1080p. No one can pay these rates now.

## Alternative Video Generation Models and Published Pricing

Google offers Google Veo 3.1 in Preview on the paid tier of the Gemini API, detailed on the Google Veo documentation. Google Veo 3.1 Fast bills at $0.10 per second for 720p video, $0.12 per second for 1080p, and $0.30 per second for 4K. Google Veo 3.1 Standard charges $0.40 per second for 720p and 1080p, or $0.60 per second for 4K, with native audio included by default.

Google Veo generates clips of 4, 6, or 8 seconds, requiring 8 seconds for 1080p, 4K, or reference image conditioning. An 8-second clip on Google Veo 3.1 Fast costs $0.80 at 720p, $0.96 at 1080p, and $2.40 at 4K. The same 8-second clip on Google Veo 3.1 Standard costs $3.20 at 720p or 1080p, and $4.80 at 4K.

On the Google video documentation, Google advises developers to use Gemini Omni Flash as the default model for video generation and multimodal editing. As published on the Google Gemini API pricing page, Gemini Omni Flash bills at 5,792 tokens per second of 720p video, which is approximately $0.10 per second under Standard terms. Google notes that Veo 3.1 remains the target recommendation when a workflow specifically requires scene extension, last-frame control, or integration with older pipelines.

ByteDance offers Dreamina Seedance through BytePlus ModelArk, supporting longer continuous generation windows. As outlined in the BytePlus ModelArk specifications, Seedance 2.5 supports single generations between 4 and 30 seconds at 480p, 720p, and 1080p across seven aspect ratios. According to the BytePlus ModelArk pricing guide, a 5-second text-to-video clip at 720p without input video costs $1.156, which equals $0.231 per second, while 1080p costs $2.843, or $0.569 per second.

Seedance 2.0 generates clips up to 15 seconds, including 4K at $3.89 for 5 seconds ($0.78 per second). Seedance 2.5 costs more per second than Google Veo 3.1 Fast at 720p and more than Google Veo 3.1 Standard at 1080p. Video-model prices change without notice, so verify current figures on the Google Gemini API pricing page and BytePlus ModelArk pricing guide before budgeting.

Developers can also access models through third-party platforms such as Runway, which publishes tiered credit packages on the Runway pricing page. Runway provides a free tier with 125 one-time credits, a Standard plan at $15 monthly for 625 credits, Pro at $35 monthly for 2,250 credits, and Max at $95 monthly for 9,500 credits. To explore detailed feature breakdowns, compare models in our guide to Sora vs Veo, examine historic rates at Sora pricing, or evaluate broader tools in our analysis of Sora alternatives.

## Migration Checklist for Automated Video Workflows

Swapping Sora out of a working pipeline takes three passes: code, prompts and constraints. Begin by searching code repositories for obsolete API keys, endpoint URLs, and Software Development Kit (SDK) packages that reference Sora. Once identified, replace client initialization calls with your selected alternative service, such as the Gemini developer API or BytePlus ModelArk.

Prompt templates must be re-evaluated against the target model syntax and parameter constraints. Different models interpret camera direction, lighting prompts, and character continuity differently. Run identical prompt suites across the replacement endpoint to identify any shifts in formatting compatibility.

You must also verify technical constraints including clip durations, aspect ratios, and media retention policies. Google Veo 3.1 restricts aspect ratios to 16:9 and 9:16, generates clips of 4, 6, or 8 seconds, and automatically deletes files from Google servers after 2 days. Google Veo also embeds a SynthID watermark in every video. Seedance on BytePlus ModelArk adds aspect ratios Veo lacks, including 21:9, 4:3, 1:1 and 3:4.

- Scan codebases to remove deprecated API routes and SDK calls
- Set up a paid-tier Gemini API key, or a BytePlus ModelArk account (BytePlus recommends a balance above USD 30 before enabling Seedance 2.5)
- Adjust prompt structures to align with the new model parameter requirements
- Validate aspect ratio compatibility across target production formats
- Download Google Veo outputs within 2 days, before Google removes them from its servers
- Inspect watermarking compliance, noting that all Google Veo outputs include SynthID

## Comparison of Replacement Video Generation Platforms

Evaluating replacements requires weighing cost per second against clip duration and rendering flexibility. Veo 3.1 Lite, at $0.05 per second at 720p, is the cheapest route in the Gemini API. Veo 3.1 Fast and Gemini Omni Flash match Sora 2 Standard's last reported $0.10 per second at 720p. Seedance 2.5 generates single takes up to 30 seconds, at $0.231 per second at 720p.

If your application relies on scene extension, Google Veo Standard and Fast allow up to 20 iterative extensions of 7 seconds each, reaching up to 148 seconds total at 720p. Google does not state how extension seconds are billed, so confirm on the Google Gemini API pricing page.

Who this recommendation does not serve: Teams requiring offline, on-premise, or self-hosted video engines will not find a direct solution among these managed cloud endpoints and should evaluate open-source local frameworks instead. What would change this assessment: if Veo 3.1 added single takes longer than 8 seconds, Seedance 2.5 would lose its main advantage as a Sora replacement.

- Google Veo 3.1 Fast: $0.10 per second at 720p, the same as Sora 2 Standard's last reported rate
- Gemini Omni Flash: Google's recommended default, about $0.10 per second at 720p, with conversational video editing
- Google Veo 3.1 Standard: $0.40 per second at 720p and 1080p, $0.60 at 4K (Fast also does 4K and reference images for less; test both)
- ByteDance Seedance 2.5: Best for continuous single-take clips up to 30 seconds across varied aspect ratios
- Runway: Best for creative teams desiring a credit-based web dashboard with access to multiple models

## Keep the Next Model Swap Small

In the implementations we run for clients, a model swap stays cheap when every model call sits behind one internal function, so prompts and business logic never reference a vendor's parameters directly. A team built that way changes one function and re-tests its prompts.

To start, search your codebase for Sora API calls, check your export status on OpenAI's help page, and run one production prompt on the replacement you picked.

## Frequently Asked Questions

- Yes, Sora AI is shut down. OpenAI discontinued the Sora web and app interfaces on April 26, 2026, and permanently deactivated the developer API on September 24, 2026. Sora is no longer available for commercial or consumer use.
- Yes, Sora has completely shut down. The Sora web and app experiences stopped on April 26, 2026, and the Application Programming Interface (API) was taken offline on September 24, 2026. Users can no longer generate new video content through any OpenAI platform.
- Sora 2 has already shut down. OpenAI closed the consumer interfaces on April 26, 2026, and officially decommissioned developer API access on September 24, 2026. There are no remaining active operating hours for the service.
- Yes, Sora 2 has been removed from active service. OpenAI disabled both consumer generation portals and backend API endpoints, meaning new generation tasks cannot be submitted. OpenAI's help page describes an export route at sora.chatgpt.com/sunset. Check that page for whether the route is open.
- Sora AI is permanently discontinued in its current form. OpenAI stated that after any final export window offered to users, all data associated with Sora will be permanently deleted.

### Plan Your Video Workflow Migration

Book a 30-minute consultation. We review your current video generation pipeline, identify the right replacement API, and design an architecture that survives future model deprecations.

Book a Consultation
