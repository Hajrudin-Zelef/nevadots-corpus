---
id: collect-261121-veille-ia/gemini-google/how-to-use-the-gemini-3-7-flash-api
title: "How to Use the Gemini 3.7 Flash API ?"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-08-13"]
keywords: ["flash", "gemini", "google", "input", "model", "deepswe", "code", "output"]
source: https://apidog.com/blog/how-to-use-gemini-3-7-flash-api/
source_anchor: ""
source_lines: [1, 246]
section: "Gemini_Google"
---
Google shipped Gemini 3.7 Flash on August 13, 2026, three weeks after 3.6 Flash, and calls it “our most intelligent workhorse model.” The headline for developers: agentic coding scores jumped hard (DeepSWE v1.1 went from 49.0% to 65.3%), the intro price is half of what 3.6 Flash launched at, and the API surface is unchanged. If you already call Gemini, you swap one model ID. If you don’t, this is the cheapest entry point Google has ever offered for a model this capable.

This guide is the hands-on quickstart. You’ll get an API key, make your first call in cURL, port it to Python and Node.js, stream responses, tune `generationConfig`, and wire the whole thing into Apidog so you can iterate on prompts without burning tokens in a code loop. The specs from the official announcement: 1M token context, 64k output, multimodal input, function calling, search as a tool, and computer use.

If you built against the previous generation, the request shape carries over from our Gemini 3 Flash Preview API guide; this article covers everything that’s new in the 3.7 workflow.

## TL;DR

- Model ID is `gemini-3.7-flash` . Endpoint:`POST https://generativelanguage.googleapis.com/v1beta/models/gemini-3.7-flash:generateContent` with header`x-goog-api-key: <KEY>` .
- Intro pricing runs $0.75 per 1M input tokens and $3.75 per 1M output tokens through December 31, 2026. From January 1, 2027 it doubles to $1.50 and $7.50.
- Specs: 1M token input context, 64k token output limit. Input accepts text, image, video, audio, and PDF. Output is text.
- Benchmark deltas over 3.6 Flash: DeepSWE 49.0% to 65.3%, FrontierCode 34.4% to 43.6%, AutomationBench 17.0% to 30.4%, WebDev Arena Elo 1538 to 1588.
- Streaming uses `:streamGenerateContent?alt=sse` . The request body keeps Google’s`contents` plus`generationConfig` schema.
- Test the endpoint in Apidog before you write application code: import the spec, store the key as an environment variable, and watch SSE chunks render live.

## What Gemini 3.7 Flash is good for

Flash models trade a little peak intelligence for speed and price, and 3.7 narrows that trade more than any release before it. The benchmark deltas over 3.6 Flash are unusually large for a three-week gap: DeepSWE v1.1 jumped from 49.0% to 65.3%, FrontierCode 1.1 Main from 34.4% to 43.6%, and AutomationBench from 17.0% to 30.4%. WebDev Arena Elo climbed 50 points, from 1538 to 1588.

Read those numbers as a signal about workload fit. Reach for 3.7 Flash when:

- **You run agent loops.** The AutomationBench score nearly doubled, and Google says the model “thinks more diligently on multi-step planning and tool calls.” Agent pipelines with many short, tool-heavy turns are the target use case.
- **You generate or debug code.** Google claims 3.7 is better at debugging and more capable of producing deployable code on the first try. The DeepSWE and FrontierCode gains back that up.
- **You process documents.** GDP.pdf jumped from 22.0% to 34.0%, and PDF is a first-class input type. Long-context retrieval holds up too: 97.0% on the 128k-needle test.
- **You need multimodal input on a budget.** Text, image, video, audio, and PDF all go in through the same`contents` array.

For the full feature rundown, including the legal-domain Harvey LAB-AA score of 90.7% and the updated CBRN and cyber safeguards, see what’s new in Gemini 3.7 Flash. Context worth knowing: Gemini 3.5 Pro is still delayed, and Axios reports Google is deliberately shipping Flash updates ahead of its next flagship.

## Get an API key

Two paths, and they aren’t equivalent.

**AI Studio (fast path).** Open aistudio.google.com/apikey, click *Get API key*, pick a Google Cloud project, and copy the string. The key works against `generativelanguage.googleapis.com` immediately, and the free tier gives you enough quota to prototype. Gemini 3.7 Flash is available in 160+ countries.

**Vertex AI (production path).** If your infrastructure lives on GCP, use Vertex. Authentication switches from an API key to OAuth (service accounts or short-lived tokens), calls route through `aiplatform.googleapis.com`, and you gain IAM, audit logs, and regional endpoints. The model ID and request body stay identical; only the URL and auth mechanism change.

Prototype on AI Studio, move to Vertex before production traffic. Either way, export the key once:

```
export GEMINI_API_KEY="AIza..."
```
Never hardcode the key or pass it as a `?key=` query parameter in production; query strings end up in server logs.

## Endpoint and authentication

The base endpoint for a synchronous call:

```
POST https://generativelanguage.googleapis.com/v1beta/models/gemini-3.7-flash:generateContent
```
Streaming swaps the method suffix and adds the SSE flag:

```
POST https://generativelanguage.googleapis.com/v1beta/models/gemini-3.7-flash:streamGenerateContent?alt=sse
```
Auth is one header: `x-goog-api-key: $GEMINI_API_KEY`. That’s the entire handshake. No bearer tokens, no signature scheme, no session setup.

## Your first request in cURL

Here’s a complete working call:

```
curl "https://generativelanguage.googleapis.com/v1beta/models/gemini-3.7-flash:generateContent" \
  -H "x-goog-api-key: $GEMINI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "contents": [{
      "role": "user",
      "parts": [{ "text": "Review this SQL for injection risk: SELECT * FROM orders WHERE id = ${orderId}" }]
    }],
    "generationConfig": {
      "temperature": 0.3,
      "maxOutputTokens": 1024
    }
  }'
```
The response returns a `candidates` array. Each candidate carries a `content` object with `parts` (text, or function calls if you declared tools) and a `finishReason`. Token counts live in `usageMetadata` at the top level; watch that block, because output tokens cost five times what input tokens do at the intro rate.

Note the schema: Google uses `contents` with `role` and `parts`, not OpenAI’s `messages` shape. Get that mapping right first if you’re porting from another provider.

## Python quickstart

Install or upgrade the official SDK:

```
pip install --upgrade google-generativeai
```
A basic call with a system instruction:

```
import os
import google.generativeai as genai
genai.configure(api_key=os.environ["GEMINI_API_KEY"])
model = genai.GenerativeModel(
    model_name="gemini-3.7-flash",
    system_instruction="You are a code reviewer. Flag issues as blocking or non-blocking.",
    generation_config={
        "temperature": 0.3,
        "max_output_tokens": 2048,
    },
)
response = model.generate_content(
    "Review this Flask route for security issues:\n\n"
    "@app.route('/user/<id>')\n"
    "def get_user(id):\n"
    "    return db.execute(f'SELECT * FROM users WHERE id = {id}')"
)
print(response.text)
print("input tokens:", response.usage_metadata.prompt_token_count)
print("output tokens:", response.usage_metadata.candidates_token_count)
```
Multimodal input rides in the same `contents` array. To send a PDF, upload it through the Files API and reference it as a part:

```
invoice = genai.upload_file("q3-invoice.pdf")
response = model.generate_content([
    invoice,
    "Extract the invoice number, total, and due date as JSON.",
])
print(response.text)
```
The GDP.pdf benchmark gain (22.0% to 34.0%) shows up in exactly this workload: structured extraction from messy real-world documents.

## Node.js quickstart

The Node SDK is `@google/generative-ai` and mirrors the Python shape:

```
import { GoogleGenerativeAI } from "@google/generative-ai";
const genAI = new GoogleGenerativeAI(process.env.GEMINI_API_KEY);
const model = genAI.getGenerativeModel({
  model: "gemini-3.7-flash",
  generationConfig: {
    temperature: 0.3,
    maxOutputTokens: 2048,
    responseMimeType: "application/json",
    responseSchema: {
      type: "object",
      properties: {
        severity: { type: "string", enum: ["blocking", "non-blocking"] },
        issues: { type: "array", items: { type: "string" } },
      },
      required: ["severity", "issues"],
    },
  },
});
const result = await model.generateContent(
  "Review this Express handler: app.get('/search', (req, res) => res.send(eval(req.query.q)))"
);
console.log(JSON.parse(result.response.text()));
```
The `responseSchema` line matters more than it looks. It forces the candidate into a parseable object, so downstream code never touches free-form text. Pair it with `responseMimeType: "application/json"` or it’s ignored.

## Streaming

For chat UIs and anything user-facing, stream. In Python, add `stream=True`:

```
stream = model.generate_content(
    "Explain the N+1 query problem with a concrete ORM example.",
    stream=True,
)
for chunk in stream:
    if chunk.text:
        print(chunk.text, end="", flush=True)
```
Over raw HTTP, hit `:streamGenerateContent?alt=sse` and parse server-sent events. Each `data:` line carries a partial `candidates` payload; the final chunk includes `usageMetadata`, so token accounting is only accurate after the stream closes.

## Tuning generationConfig

The parameters you’ll touch most, in rough order of impact:

| Parameter | Type | What it does | 
|---|---|---|
| `maxOutputTokens` | integer | Hard cap on output, up to the model’s 64k limit. Your main cost lever. | 
| `temperature` | number | 0 to 2. Use 0.2 to 0.4 for code and extraction, 0.7+ for creative text. | 
| `responseMimeType` | string | Set `application/json` to force JSON output. | 
| `responseSchema` | object | Enforces a strict shape when paired with the JSON mime type. | 
| `topP` | number | Nucleus sampling cutoff. Leave at default unless you’re tuning deliberately. | 
| `stopSequences` | array | Strings that halt generation early. Useful for delimiter-based parsing. | 

Output tokens run $3.75 per million at the intro rate and $7.50 from January 2027, so cap output at what your use case needs, not the 64k ceiling. The full token math, with worked examples per workload, is in our Gemini 3.7 Flash pricing breakdown.

Beyond `generationConfig`, the request body also accepts `tools` (function declarations, search as a tool, computer use) and `toolConfig` for forcing tool calls. Tool use is where 3.7 Flash improved most, and it deserves its own walkthrough: see the Gemini 3.7 Flash function calling tutorial for declarations, parallel calls, and the response-loop pattern.

## Test the endpoint in Apidog before you write app code

Prompt iteration inside a Python script is slow and expensive: edit, rerun, scroll, repeat, and every cycle bills tokens. The faster loop is to lock the request shape in an API client first, then port to code once responses look right.

Apidog handles the Gemini request schema natively. The setup:

1. **Create a project** and import the Generative Language API OpenAPI spec from Google’s API docs. The collection arrives pre-named, so`generateContent` is one search away.
2. **Add an environment variable** named`GEMINI_API_KEY` and bind it to the`x-goog-api-key` header at the environment level. Every request inherits it, and the key never appears in a saved request body.
3. **Store the model ID as a variable** set to`gemini-3.7-flash` . When you want to A/B against`gemini-3.6-flash` , you change one variable instead of editing URLs across a dozen saved requests.
4. **Build the `contents` array in the visual JSON editor.** Nested parts render cleanly, and schema validation catches a malformed body before you spend a single token on a 400.
5. **Hit the streaming endpoint.** Apidog renders SSE chunks live, so you watch the answer assemble exactly the way your SDK will see it, latency included.
6. **Save good responses as examples.** Later test runs hit the fixture instead of the live API. This is the single biggest token saver in the whole workflow.

Once requests are saved, chain them into test scenarios with assertions on `finishReason`, response schema, and `usageMetadata` token counts. That turns a manual smoke test into a regression suite you can run on every prompt change; the same pattern QA teams use is covered in our API testing guide for QA engineers.

## Error handling and rate limits

Gemini errors return a top-level `error` object with `code`, `status`, and `message`. The ones you’ll meet:

| Code | Status | Meaning | Fix | 
|---|---|---|---|
| 400 | `INVALID_ARGUMENT` | Malformed body, bad role, empty `contents` . | Validate the body in Apidog before sending. | 
| 401 | `UNAUTHENTICATED` | Missing or revoked key. | Re-export `GEMINI_API_KEY` ; confirm the key is active in AI Studio. | 
| 403 | `PERMISSION_DENIED` | Project lacks access or billing. | Check project settings and billing status. | 
| 429 | `RESOURCE_EXHAUSTED` | Rate limit or daily quota hit. | Back off with jitter, batch requests, or upgrade tiers. | 
| 500 | `INTERNAL` | Transient server fault. | Retry with exponential backoff. | 
| 503 | `UNAVAILABLE` | Service overloaded. | Retry after a few seconds; on Vertex, try another region. | 

Three habits keep production stable:

- **Wrap every call in a retry helper** that handles 429 and 5xx with jittered exponential backoff. The SDKs retry a few times on their own, but a thin wrapper gives you logging and circuit-breaking you control.
- **Don’t invent rate-limit numbers.** Limits vary by tier and change over time; read the live values on the Gemini API pricing and limits page and alert at 80% of quota.
- **Pin the model ID behind an environment variable.** If a 3.7 behavior change breaks a prompt, rolling back to`gemini-3.6-flash` becomes a config change instead of a deploy.

## FAQ

### Is Gemini 3.7 Flash free to use?

AI Studio ships a free tier with daily quota that’s enough for prototyping, and the paid intro rate is $0.75 per 1M input tokens through December 31, 2026. If you want to stretch the no-cost path further, our guide to free Gemini API access covers the tiers and their limits.

### What’s the difference between calling it via AI Studio and Vertex AI?

Same model, same request body, different plumbing. AI Studio uses an API key against `generativelanguage.googleapis.com`; Vertex uses OAuth against `aiplatform.googleapis.com` and adds IAM, audit logging, and regional endpoints. Start on AI Studio, graduate to Vertex when traffic gets real.

### Can I send images, audio, and PDFs to Gemini 3.7 Flash?

Yes. Input is multimodal: text, image, video, audio, and PDF all travel as parts in the `contents` array, inline as base64 or by reference through the Files API. Output is text only.

### How big are the context window and output limit?

1M tokens in, 64k tokens out. The 128k-needle retrieval score of 97.0% suggests long-context recall is dependable well past what most apps need, but chunking long inputs still saves money since every input token bills.

### Should I upgrade from Gemini 3.6 Flash?

For agent and coding workloads, the benchmark gaps are large enough that the answer is usually yes, and the model ID swap is one line. Behavior differences worth regression-testing before you flip production traffic are covered in the 3.6 to 3.7 Flash migration guide.

## Where 3.7 Flash fits in your stack

Gemini 3.7 Flash is the rare release where the price went down while the capability went up. Through the end of 2026 you’re paying half of 3.6 Flash’s launch rate for a model that scores 16 points higher on DeepSWE and nearly double on AutomationBench. The sensible default: route agent loops, code tasks, and document extraction to 3.7 Flash now, keep the intro-rate window in mind for budget planning, and hold a rollback path to 3.6 behind an environment variable.

Start with the cURL call above, confirm the response shape, then move the request into an API client before you write application code. Download Apidog to import the Gemini spec, bind your key once, and test synchronous, streaming, and tool-calling requests from one workspace. When the prompt is right, the Python or Node port takes minutes because you already know what the wire traffic looks like.
