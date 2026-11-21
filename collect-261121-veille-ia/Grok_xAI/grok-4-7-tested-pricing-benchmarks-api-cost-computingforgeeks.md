---
id: collect-261121-veille-ia/grok-xai/grok-4-7-tested-pricing-benchmarks-api-cost-computingforgeeks
title: "Grok 4.7 Tested: Pricing, Benchmarks, API Cost | ComputingForGeeks"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: ["2026-09-25"]
keywords: ["grok", "openrouter", "tokens", "reasoning", "cost", "model", "price", "pricing"]
source: https://computingforgeeks.com/grok-4-7-tested/
source_anchor: ""
source_lines: [1, 251]
section: "Grok_xAI"
---
Grok 4.7 wrote some of the shortest files of any model in our DevOps harness, then billed for nearly eight times as many tokens as it showed. Most of what you pay for is reasoning you never see. Once you know that, the price on the launch page stops meaning much until you set `reasoning_effort` yourself.

This review covers what xAI changed in Grok 4.7, the real API pricing (reasoning tokens, a hidden prompt prefix, and a cheaper OpenRouter rate), xAI’s benchmark table next to the independent Artificial Analysis score, and the same nine infrastructure prompts we gave GPT-6 Sol and Luna and Claude Opus 5.5. Every file was linted and then run: manifests on a real cluster, backup scripts against stubbed tools, Terraform through `tofu validate`. The calls ran on 25 September 2026 against xAI’s own API, and the manifests were deployed to k3s v1.36.4 on Ubuntu 26.04.

## What changed in Grok 4.7

xAI (now styled SpaceXAI in its docs) shipped Grok 4.7 on 21 September 2026 as a direct replacement for Grok 4.6 at the same price. The launch post lists four changes: a new and larger base model rather than a retrained 4.6, a longer reinforcement learning run weighted toward tasks that take hours, better self-verification and long-context handling, and native training on the Grok Bot agent harness. It also ships a rebuilt safety stack, which xAI says lets only 3.3% of risky dual-use prompts through on its own HackerBench v0.3.

The API facts below come from xAI’s model page and release notes, cross-checked against live responses:

| Attribute | Grok 4.7 | 
|---|---|
| API model ID | `grok-4.7` (OpenRouter upstream:`grok-4.7-20260916` ) | 
| Context window | 500,000 tokens | 
| Modalities | Text and image in, text out | 
| Output limit | None on the xAI API; OpenRouter caps it at 450,000 | 
| Reasoning effort | low, medium, high (default), xhigh | 
| Batch API | Not supported | 
| Rate limits | 150 requests per second, 50M tokens per minute | 
| Fast variant | Twice the output speed at 2x the price, Cursor and Grok Build only (not the Grok Build free tier) | 
| Where it runs | xAI API, US regional endpoint, Grok Build, Cursor, OpenRouter, Vercel, Cloudflare | 

Two details matter if you are migrating code. On the Responses API, `grok-4.7` always returns `reasoning.encrypted_content`, even when `include` does not ask for it, so multi-turn clients should pass those items back unchanged. Chat Completions is unaffected. Also, the Fast variant is not served on the public API at all, so an xAI API key cannot buy it.

## Grok 4.7 pricing, and what a call actually costs

The list price matches Grok 4.6: $2 input, $0.50 cached input and $6 output per million tokens. Once a prompt reaches 200K tokens, the whole request bills at double. The US regional endpoint adds 10%, and priority processing doubles everything.

| USD per 1M tokens | Input | Cached input | Output (incl. reasoning) | 
|---|---|---|---|
| xAI API, prompt under 200K | $2.00 | $0.50 | $6.00 | 
| xAI API, prompt 200K or more | $4.00 | $1.00 | $12.00 | 
| US regional endpoint ( `us.api.x.ai` ) | $2.20 | $0.55 | $6.60 | 
| OpenRouter (xAI provider) | $1.60 | $0.40 | $4.80 | 
| Grok 4.7 Fast (Cursor, Grok Build) | $4.00 | $1.00 | $12.00 | 

The OpenRouter row is not a typo. OpenRouter lists Grok 4.7 at 20% under xAI’s own price, and a test call through it billed exactly at $1.60, $0.40 and $4.80, with `upstream_inference_cost` equal to the charged cost. Routing through OpenRouter was the cheaper way to buy Grok 4.7 on the day we tested.

Every xAI response carries the exact charge in `usage.cost_in_usd_ticks` (10 billion ticks to the dollar), so there is no reason to estimate. This call asks a one-line question at low effort and prints the token counts and the billed amount:

```
curl -s https://api.x.ai/v1/chat/completions \
  -H "Authorization: Bearer ${XAI_API_KEY}" \
  -H "Content-Type: application/json" \
  -d '{"model":"grok-4.7","reasoning_effort":"low","messages":[{"role":"user","content":"Name the kubectl command that shows why a pod is in CrashLoopBackOff."}]}' \
  | jq -r '"prompt tokens: \(.usage.prompt_tokens) (cached \(.usage.prompt_tokens_details.cached_tokens))", "completion tokens: \(.usage.completion_tokens)", "reasoning tokens: \(.usage.completion_tokens_details.reasoning_tokens)", "cost USD: \(.usage.cost_in_usd_ticks / 1e10)"'
```
A twelve-word question came back with more than twelve hundred prompt tokens:

```
prompt tokens: 1258 (cached 1152)
completion tokens: 62
reasoning tokens: 121
cost USD: 0.001886
```
That output explains the bill. The math is 106 uncached input tokens at $2, plus 1,152 cached at $0.50, plus 183 output tokens (62 visible and 121 reasoning) at $6. That comes to $0.001886, which matches the ticks exactly. Two things follow from it.

### Grok 4.7 adds a hidden prompt of about 1,240 tokens

Send `hi` to Grok 4.7 and the usage block reports 1,243 prompt tokens. The same message to Grok 4.6 reports 637. xAI prepends a server-side prompt to every request, and on 4.7 it roughly doubled. Your own system message goes on top of it (a three-word system prompt took the count to 1,250). The saving grace is caching: 1,152 or more of those tokens came back as cached on eight of the nine low-effort harness calls and on every single call we made by hand, so the prefix costs about $0.0008 per request rather than $0.0025. It still sets a floor. A bare `hi` at low effort still cost $0.00097.

### completion_tokens does not include reasoning on the xAI API

xAI reports reasoning separately in `completion_tokens_details.reasoning_tokens` and leaves it out of `completion_tokens`, but the charge in `cost_in_usd_ticks` bills reasoning at the full $6 output rate (the arithmetic above matches to the tick). OpenRouter folds reasoning into `completion_tokens` (our OpenRouter call returned 266 completion tokens, of which 258 were reasoning). A cost dashboard that multiplies `completion_tokens` by $6 will undercount the xAI bill badly. On our nine harness calls, it would have reported $0.043 against a real list-price cost of $0.164.

## Grok 4.7 benchmarks: xAI’s table and the independent score

xAI’s launch table puts Grok 4.7 at xhigh effort against Grok 4.6, GPT-5.6 Sol and Fable 5.1. It does not include GPT-6 Sol or Claude Opus 5.5, both of which shipped the following day. The DeepSWE figure is at high effort, not xhigh:

| Benchmark | Grok 4.7 xHigh | Grok 4.6 High | GPT-5.6 Sol Max | Fable 5.1 Max | 
|---|---|---|---|---|
| CursorBench 4.0 | 46.3% | 40.4% | 41.7% | 51.8% | 
| DeepSWE v1.1 | 71.0% (high) | 65.2% | 72.7% | 70.0% | 
| EEBench | 64.0% | 53.0% | 39.4% | 56.4% | 
| AA Briefcase v1.1 | 1,657 | 1,546 | 1,487 | 1,678 | 
| Terminal-Bench 4.0 | 37.6% | 20.3% | 37.3% | 57.9% | 
| Harvey Legal Agent | 19.6% | 15.8% | 2.5% | 6.7% | 
| HealthBench Professional | 56.7% | 48.5% | 60.5% | 62.1% | 
| Price in / out per 1M | $2 / $6 | $2 / $6 | $4 / $20 | $10 / $50 | 

The Terminal-Bench 4.0 jump from 20.3% to 37.6% is the real generational gain over 4.6, and it lines up with the “longer tasks” training claim. Fable 5.1 still beats Grok by 20 points on that benchmark, and GPT-5.6 Sol keeps a small edge on DeepSWE. The leads Grok does hold are in legal and electrical engineering work, which are not the problems most readers here are solving.

Artificial Analysis scores Grok 4.7 (xhigh) at 46.4 on its Intelligence Index. That ties MiMo-V2.6-Pro at 46.3, sits just under GPT-6 Sol at max effort (47.5), and sits well under Opus 5.5 at max (57.6). The sub-scores it publishes include 43.1% on Humanity’s Last Exam, 76.7% on AA-LCR long-context reasoning and a 70.7% non-hallucination rate on AA-Omniscience. It is a solid frontier score for $2 and $6 pricing, but it does not lead the field.

## Grok 4.7 on real DevOps tasks

The harness is unchanged from the GPT-6 and MiMo tests, and it follows the same grading approach as our coding agent DevOps tests. There are three prompts, each run three times. The first asks for a Kubernetes Deployment plus Service for nginx with `runAsNonRoot`. The second asks for a bash script that dumps PostgreSQL to S3 and prunes old dumps. The third asks for an AWS VPC in Terraform.

Linting is the easy part. The manifests went through `kubeconform -strict` and were then applied to k3s and curled through the Service. The scripts went through `shellcheck` at warning and info level, then ran against stub `pg_dump` and `aws` binaries that check what they are given. The Terraform had to pass `tofu validate` and `tofu fmt -check`. We ran Grok twice: once at the default high effort, and once with `reasoning_effort: low`.

| Model | K8s serves HTTP 200 | Bash behaves correctly | tofu validate | Output tokens (9 calls) | Cost (9 calls) | 
|---|---|---|---|---|---|
| Grok 4.7 (high, default) | 0 of 3 | 3 of 3 | 3 of 3 | 23,083 | $0.1637 | 
| Grok 4.7 (low) | 0 of 3 | 3 of 3 | 3 of 3 | 6,339 | $0.0632 | 
| GPT-6 Sol | 3 of 3 | 3 of 3 | 3 of 3 | 8,115 | $0.0841 | 
| GPT-6 Luna | 0 of 3 | 3 of 3 | 3 of 3 | 6,936 | $0.0036 | 
| Claude Opus 5.5 | 3 of 3 | 3 of 3 | 3 of 3 | 13,225 | $0.2732 | 
| MiMo-V2.6-Pro | 1 of 3 | 3 of 3 | 3 of 3 | 18,867 | $0.0171 | 

All costs are list price with no caching, so the rows compare fairly. Output tokens include reasoning for every model. The low-effort run actually billed $0.0479 once the hidden prefix hit the cache. At the default effort, 20,102 of Grok’s 23,083 output tokens were reasoning. The files you actually get back totalled 2,981 tokens, so 87% of the output tokens, and about three quarters of the bill, was thinking.

### nginx: [emerg] mkdir() “/var/cache/nginx/client_temp” failed (13: Permission denied)

All six Grok manifests passed `kubeconform -strict`, and all six crash-looped on the cluster. The cause was identical every time. Every Grok manifest pins the pod to UID 101 with `runAsNonRoot` and mounts nothing writable (two of them also drop all capabilities). The stock `nginx:1.27-alpine` image then tries to create its temp directories under `/var/cache/nginx` as a user who cannot write there. GPT-6 Luna and two of the three MiMo-V2.6-Pro manifests failed the same way. GPT-6 Sol and Opus 5.5 both added `emptyDir` volumes without being asked.

The pods never come up, and the log names the directory:

```
NAME                   READY   STATUS             RESTARTS      AGE
web-5f6977447c-66fmn   0/1     CrashLoopBackOff   3 (30s ago)   70s
web-5f6977447c-8r8t8   0/1     CrashLoopBackOff   3 (32s ago)   70s
web-5f6977447c-crwlb   0/1     CrashLoopBackOff   3 (36s ago)   70s
2026/09/25 13:59:27 [emerg] 1#1: mkdir() "/var/cache/nginx/client_temp" failed (13: Permission denied)
nginx: [emerg] mkdir() "/var/cache/nginx/client_temp" failed (13: Permission denied)
```
Lowering the effort changed none of this. Two of the six set `readOnlyRootFilesystem: false` explicitly, so Grok was thinking about writability and still missed the cache path.

### Grok fixed its own manifest once it saw the log

xAI says Grok 4.7 is better at checking its own work, so we tested that directly. We sent back the first manifest along with the two-line error and asked for a fix. The call took 87 seconds, spent 4,990 reasoning tokens and billed $0.0345, which is what six or seven low-effort harness calls cost. The manifest it returned deploys cleanly. This is the file exactly as Grok produced it:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web
  labels:
    app: web
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
    spec:
      securityContext:
        runAsNonRoot: true
        runAsUser: 101
        runAsGroup: 101
        fsGroup: 101
        seccompProfile:
          type: RuntimeDefault
      containers:
        - name: web
          image: nginx:1.27-alpine
          ports:
            - name: http
              containerPort: 80
          resources:
            requests:
              cpu: 100m
              memory: 128Mi
            limits:
              cpu: 250m
              memory: 256Mi
          readinessProbe:
            httpGet:
              path: /
              port: 80
          livenessProbe:
            httpGet:
              path: /
              port: 80
          securityContext:
            runAsNonRoot: true
            runAsUser: 101
            runAsGroup: 101
            allowPrivilegeEscalation: false
            capabilities:
              drop:
                - ALL
              add:
                - NET_BIND_SERVICE
          volumeMounts:
            - name: nginx-cache
              mountPath: /var/cache/nginx
            - name: nginx-run
              mountPath: /var/run
            - name: nginx-tmp
              mountPath: /tmp
      volumes:
        - name: nginx-cache
          emptyDir: {}
        - name: nginx-run
          emptyDir: {}
        - name: nginx-tmp
          emptyDir: {}
---
apiVersion: v1
kind: Service
metadata:
  name: web
  labels:
    app: web
spec:
  type: ClusterIP
  selector:
    app: web
  ports:
    - name: http
      port: 80
      targetPort: 80
      protocol: TCP
```
The same cluster, with the original manifest and the fixed one side by side:

All three pods run, and the Service answers 200 (it prints twice because `kubectl run -i` echoes the attached output). One line in the fix does nothing, though. `add: NET_BIND_SERVICE` reads like the reason port 80 binds as UID 101, but inside the running pod `CapEff` is `0000000000000000`. The capability sits in the bounding set (`0x400`), but a non-root process never gets it without ambient capabilities or file capabilities on the binary. Port 80 works because containerd sets `net.ipv4.ip_unprivileged_port_start=0` in the pod’s network namespace, and the pod reported `0` when we read it. containerd only enables that by default from 2.0 onward (`enable_unprivileged_ports`). On a 1.x runtime, or any runtime that leaves the sysctl at 1024, this manifest would fail again, and the capability line would not save it.

### The bash scripts were short and correct

All six scripts passed `shellcheck` at info level, which is the level where MiMo-V2.6-Flash’s `${DBUSER}` typo showed up. All six refused to run with unset variables, produced a valid gzip, called `aws s3 cp` once, deleted the 10-day-old dump and kept the 3-day-old one. The high-effort scripts were 14 or 15 lines, built on the `: "${DB_NAME:?DB_NAME is unset}"` idiom. This is the third high-effort run as generated:

```
#!/bin/bash
set -euo pipefail
: "${DB_NAME:?DB_NAME is unset}"
: "${DB_USER:?DB_USER is unset}"
: "${S3_BUCKET:?S3_BUCKET is unset}"
timestamp="$(date '+%Y%m%d%H%M%S')"
dump_file="${DB_NAME}_${timestamp}.sql.gz"
pg_dump -U "${DB_USER}" -d "${DB_NAME}" | gzip -c > "${dump_file}"
aws s3 cp "${dump_file}" "s3://${S3_BUCKET}/${dump_file}"
find . -type f -name "${DB_NAME}_*.sql.gz" -mtime +7 -exec rm -f "{}" \;
```
It works. It also writes to and prunes whatever directory cron happens to start in, because Grok never picked a backup directory. The prompt didn’t ask for one, but Opus 5.5 still put dumps under a configurable `BACKUP_DIR` defaulting to `/var/backups/postgres`, and GPT-6 Sol at least kept them in a dedicated subdirectory. Only one of the six Grok scripts (a low-effort run) added a `DUMP_DIR` variable, and it defaults to `.`. Grok does what the prompt says and little else, which is exactly what went wrong in the Kubernetes task too.

### Terraform passed clean at both efforts

Six of six configurations passed `tofu validate` and `tofu fmt -check`. Five named the security group `main` and one left the name to the provider, so none hit the `sg-` prefix that the AWS provider rejects and that broke one of MiMo-V2.6-Flash’s files. Every model in both harnesses pinned the AWS provider to the 5.x line, although the registry’s current major is 6. Pin to 6 yourself if you are starting fresh.

## Grok 4.7 reasoning effort: low vs high

High is the default, and on this workload it bought nothing. Both passes produced the same pass rate and the same Kubernetes failure, and the low-effort files were no worse by any check we ran:

| Measure (9 calls) | High (default) | Low | 
|---|---|---|
| Reasoning tokens | 20,102 | 3,204 | 
| Visible output tokens | 2,981 | 3,135 | 
| List-price cost | $0.1637 | $0.0632 | 
| Median latency, k8s / bash / tofu | 28.2 s / 56.0 s / 22.7 s | 11.8 s / 11.9 s / 7.6 s | 
| Slowest single call | 65.3 s | 12.6 s | 
| Checks passed | 6 of 9 | 6 of 9 | 

High effort spent 5,140 reasoning tokens on a 14-line bash script. The bash task shows the widest gap: a 56-second median at high against 12 seconds at low, for scripts that passed the same behaviour test. Against GPT-6 Sol at its default medium effort, Grok 4.7 at low costs about three quarters as much on this harness. At its own default, it costs roughly double Sol and still fails the task Sol passes.

## Set the effort before you judge Grok 4.7

For the short, well-specified generation jobs that make up most infrastructure work, send `"reasoning_effort": "low"` on every request and route through OpenRouter if 20% off matters to you. That combination puts Grok 4.7 in the same price class as GPT-6 Sol, with the same blind spot as GPT-6 Luna: security settings that satisfy the linter and crash the pod. Keep high or xhigh for the long agent loops xAI trained it for, where the Terminal-Bench gain is real. Whatever effort you pick, deploy what it writes to a real cluster before you trust it, because `kubeconform` passed every manifest that failed here. Our earlier Grok 4.5 DevOps test found the same pattern: files that validate cleanly and still need a human to check they run.
