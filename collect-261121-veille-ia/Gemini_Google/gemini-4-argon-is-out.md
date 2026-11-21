---
id: collect-261121-veille-ia/gemini-google/gemini-4-argon-is-out
title: "Gemini 4 Argon is OUT"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["gemini", "argon", "google", "ultra", "october", "available", "cyber", "tokens"]
source: https://dev.to/hassann/gemini-4-argon-is-out-5cpp
source_anchor: ""
source_lines: [1, 138]
section: "Gemini_Google"
---
Gemini 4 Argon reached developers on October 13, 2026. Google made `gemini-4-argon` generally available on the paid tier of the Gemini API, brought it to Google AI Pro and Ultra subscribers in the Gemini app and Search, and made it available to Ultra subscribers in Google Antigravity. The original announcement came on September 30, 2026, for a limited set of Fairwind Program cyber defenders. The next major date is December 1, 2026, when API pricing doubles from $2/$10 to $4/$20 per 1M tokens.

Here is the full release timeline, Argon availability by channel, how the Fairwind phase worked, what Google has not clarified, and how to verify that your API key can call the model. For a model overview, start with what Gemini 4 Argon is. For pricing eligibility, see whether Gemini 4 Argon is free. If you call it from code, keep your requests in Apidog and store the model name in an environment variable.

Argon emerged from a summer of cyber-tuned Flash models distributed to a small group of defenders, then became broadly available 13 days after its announcement.

| Date | Event | Source | 
|---|---|---|
| 2026-07-21 | Gemini 3.5 Flash Cyber launches as a “limited-access pilot” for governments and trusted partners | Google DeepMind blog | 
| 2026-09-02 | Fairwind Program launches with Gemini 3.8 Flash Cyber and CodeMender, with 650+ partners | blog.google | 
| 2026-09-24 | Kavukcuoglu says Gemini 4 is in post-training and coming “as soon as possible” | 9to5Google | 
| 2026-09-30 | Gemini 4 Argon announced; rolling out only to a set of Fairwind partners | blog.google | 
| 2026-10-13 | GA: `gemini-4-argon` on the paid Gemini API; Pro and Ultra in the Gemini app and Search; Ultra in Antigravity | Gemini API changelog, Sundar Pichai | 
| 2026-10-13 | Cursor adds Argon at $2/$10 per 1M tokens | Cursor | 
| 2026-10-14 | OpenCode adds Argon to OpenCode Go | OpenCode | 
| 2026-11-30 | Last day of the $2/$10 API rate | Gemini API pricing page | 
| 2026-12-01 | API price rises to $4/$20 per 1M tokens | Gemini API pricing page | 

Earlier models in this line followed a defender-first pattern: Gemini 3.5 Flash Cyber never received a public API, while Gemini 3.8 Flash Cyber remained behind an allowlist as of October 1. Argon began similarly but reached public availability within two weeks.

On September 24, 9to5Google reported that Kavukcuoglu told The Information’s AI Agenda Live summit that Google hoped to ship Gemini 4 “much earlier” than the end of 2026. It did.

Reuters described Argon as arriving “after months of delays,” because Sundar Pichai had previously said Gemini 3.5 Pro would ship in June before Google dropped that plan.

The Gemini API changelog entry dated October 13, 2026 lists `gemini-4-argon` as “generally available,” and the models page labels it “New Stable.” There is one version, with no `-preview` suffix.

GA also clarified previously unavailable specifications:

- Argon has a unified 1,048,576-token context window shared by input, thinking, and output.
- Synchronous requests return 65,536 output tokens by default.
- Long decode in background mode on the Interactions API can reach approximately 1M tokens.
- Supported thinking levels are `low` ,`medium` , and`high` ;`medium` is the default.
- If you are porting Gemini 3.x code, remove `temperature` ,`top_p` , and`top_k` because Argon ignores them.
- Remove explicit caching configuration; explicit caching is not supported, while implicit caching is enabled by default.

On October 13, Sundar Pichai said Argon is “available globally today for developers and enterprises in the paid tier of the Gemini API and the Gemini platform, for Ultra subscribers in Google Antigravity, and Ultra and Pro subscribers in the Gemini app and Search.”

| Channel | Who | Status as of October 15, 2026 | 
|---|---|---|
| Gemini API | Paid-tier projects | GA since October 13, model `gemini-4-argon` | 
| Gemini API free tier | Everyone else | Not available | 
| Gemini app | Google AI Pro and Ultra subscribers | Available since October 13 | 
| Search | Google AI Pro and Ultra subscribers | Available since October 13 | 
| Google Antigravity | Ultra subscribers | Available since October 13 | 
| Enterprises | “The Gemini platform” | Named by Google; Vertex AI not confirmed | 
| Cursor, OpenCode Go | Their users | Added October 13 and 14 | 
| Fairwind Program | A set of partners | Since September 30 | 

Google’s pricing page states plainly: “Gemini 4 Argon is not available on the Free tier or on the Google AI Pro plan.” That statement refers to API plans; Pro subscribers still receive Argon in the Gemini app and Search. A consumer subscription is not an API key.

Through November 30, 2026, Argon API usage costs $2 per 1M input tokens and $10 per 1M output tokens. Starting December 1, those rates become $4 and $20. See the Argon pricing guide for detailed cost calculations.

Before GA, Fairwind was the only entry point. The program provides “high-priority defenders (like governments, healthcare providers, and telecommunications services) early access to advanced models.” Google says it works “with over 650 partners globally.” At launch, the Fairwind page stated: “A set of Fairwind Program partners get exclusive access to Gemini 4 Argon, and can use this model in CodeMender.” Membership did not automatically include Argon access.

Google summarized the phased rollout in one line: “Safely releasing frontier capabilities at this level requires a phased approach.” At announcement, it said trusted defenders and internal teams would receive Argon without cyber guardrails. Our Argon cyber defense breakdown explains what that means for the APIs you operate.

The Fairwind FAQ lists three eligible groups, without ranking them:

- Governments and national cyber authorities
- Critical infrastructure operators in healthcare, telecommunications, energy, and financial networks
- Core technology platforms that secure widely used software foundations

All applicants need “a proven track record of ethical operations and research.” Academic labs focused on defensive benchmarking may also apply. Students are directed to CodeMender on Google Cloud. Private companies are not excluded because many infrastructure operators and technology platforms are private.

Fairwind is not a simple waitlist. Google conducts background checks on applying organizations, and approved partners accept operating requirements:

1. Use “user-level authentication, phishing-resistant MFA, and applicable access controls.”
2. Limit Argon access to “internal cybersecurity, incident response, or penetration testing teams.”
3. Track employee access and use.
4. Do not share, redistribute, or resell access.

Apply through the Fairwind interest form. Google says it will “review and respond to eligible partners who meet our criteria as soon as we can,” but it has not published a response time. Approved partners use Argon on Gemini Enterprise: “When accessed directly as a managed model on Gemini Enterprise, Gemini 4 Argon supports zero data retention.”

As of October 15, 2026, these points remain unclear:

- 
**Vertex AI and Gemini Enterprise Agent Platform:** Pichai named “the Gemini platform” for enterprises, but Google has not confirmed an Argon listing on Agent Platform. One report says its release notes do not yet list Argon.
- 
**Gemini app limits:** Google has not published Argon message limits for Pro and Ultra subscribers.
- 
**Per-model API rate limits:** RPM, TPM, and RPD limits are not in the docs; AI Studio shows the limits for your project.
- 
**Knowledge cutoff:** Not published.
- 
**Gemini CLI:** Google has not confirmed Argon support there.
- 
**Fairwind review time and partner count:** Neither has been disclosed.

Launch day has passed, but the same checks now tell you whether your key can call Argon and what model metadata it exposes.

**Check the changelog and models page.** Both list `gemini-4-argon` as of October 13.

**Poll `models.list`.** The Gemini API returns every model visible to your key. On October 1, 2026, this request returned 61 models and no Argon match:


```
curl -s "https://generativelanguage.googleapis.com/v1beta/models?pageSize=1000" \
  -H "x-goog-[REDACTED CREDENTIAL] \
  | jq -r '.models[] | select(.name | test("argon"; "i")) | "\(.name) out=\(.outputTokenLimit)"'
```
Each model entry includes `name`, `inputTokenLimit`, `outputTokenLimit`, `supportedGenerationMethods`, and `thinking`. If the request returns no Argon result, first confirm that the key belongs to a billing-enabled project because Argon is not available on the free tier.

**Turn the request into an automated check.** Save the same GET request in Apidog, store `GEMINI_API_KEY` as an environment variable, and add a post-processor script that fails when Argon is missing:


```
const models = pm.response.json().models || [];
const hits = models.filter(model => /argon/i.test(model.name));
pm.test("Argon is visible to this key", () => {
  pm.expect(hits.length).to.be.above(0);
});
if (hits.length) {
  console.log(
    hits
      .map(model => `${model.name} out=${model.outputTokenLimit}`)
      .join("\n")
  );
}
```
Add the request to a test scenario, run it through Apidog scheduled tasks or CI with the Apidog CLI, and connect a notification. A revoked key or billing problem then appears as a failed test rather than a production error.

Store `GEMINI_MODEL=gemini-4-argon` alongside the key. Price saved requests at the December rates so the upcoming change appears in tests before it appears on an invoice. The Argon API guide covers request shapes in detail.

October 13, 2026, for developers on the paid Gemini API and for Pro and Ultra subscribers in the Gemini app. Google announced it on September 30, 2026, for Fairwind partners only.

Yes, with a paid-tier Gemini API key, a Google AI Pro or Ultra subscription in the Gemini app and Search, or an Ultra subscription for Antigravity. Cursor and OpenCode Go also offer it. There is no free API tier; for a model with one, the Gemini 3.8 Flash API guide walks through a first call.

November 30, 2026 is the last day at $2/$10 per 1M tokens. From December 1, the API rate is $4/$20.

Yes, in the Gemini app and Search. They do not get API access through Pro, and Google named only Ultra subscribers for Antigravity.

`gemini-4-argon`. It is the stable version and has no `-preview` suffix.

If you build on the Gemini API, move a test workload to `gemini-4-argon` while the $2/$10 rate remains active, measure real token consumption, and model future costs at $4/$20 so the December bill contains no surprises. If you manage security for a qualifying organization, Fairwind remains the route to Argon on Gemini Enterprise with zero data retention.

Download Apidog to keep the access check, regression requests, and cost assertions in one workspace.

## Top comments (0)
