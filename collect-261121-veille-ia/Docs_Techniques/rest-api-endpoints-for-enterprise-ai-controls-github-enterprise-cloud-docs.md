---
id: collect-261121-veille-ia/docs-techniques/rest-api-endpoints-for-enterprise-ai-controls-github-enterprise-cloud-docs
title: "REST API endpoints for enterprise AI controls - GitHub Enterprise Cloud Docs"
domain: docs-techniques
role: reference
task: article
actors: []
dates: []
keywords: ["enterprise", "github", "name", "policy", "model", "apps", "endpoint", "controls"]
source: https://docs.github.com/en/enterprise-cloud@latest/rest/enterprise-admin/ai-controls
source_anchor: ""
source_lines: [1, 708]
section: "Docs_Techniques"
---
# REST API endpoints for enterprise AI controls

Use the REST API to manage Copilot model policies for your enterprise, enterprise teams, and enterprise apps.

Use this endpoint to retrieve Copilot model policies for agent apps. The response shows which model settings can be changed and which are controlled by enterprise policy.

For models whose settings can be changed, the response shows the configured
setting or `optional` if no setting has been configured. For models
controlled by enterprise policy, it shows the enterprise setting or the
applicable default. These settings do not necessarily determine whether
an individual user can access a model.

Enterprise owners and users with the `read_enterprise_ai_controls` permission can use this endpoint.

OAuth app tokens and personal access tokens (classic) need the `read:enterprise` scope to use this endpoint.

This endpoint works with the following fine-grained token types:

The fine-grained token must have the following permission set:

- "Enterprise AI controls" enterprise permissions (read)

| Headers | 
|---|
| Name, Type, Description | 
|---|
| `accept` stringSetting to `application/vnd.github+json` is recommended. | 

| Path parameters | 
|---|
| Name, Type, Description | 
|---|
| `enterprise` string RequiredThe slug version of the enterprise name. | 

| Status code | Description | 
|---|---|
| `200` | OK | 
| `401` | Requires authentication | 
| `403` | Forbidden | 
| `404` | Resource not found | 
| `503` | Service unavailable | 

If you access GitHub at GHE.com, replace `api.github.com` with your enterprise's dedicated subdomain at `api.SUBDOMAIN.ghe.com`.

#### Request examples

```
curl -L \
  -H "Accept: application/vnd.github+json" \
  -H "Authorization: Bearer <YOUR-TOKEN>" \
  -H "X-GitHub-Api-Version: 2026-03-10" \
  https://api.github.com/enterprises/ENTERPRISE/copilot/apps/model-policies
```
OK

`Status: 200````
{
  "models": [
    {
      "model_name": "gpt-5.6-sol",
      "display_name": "OpenAI GPT-5.6 Sol",
      "policy_state": "optional",
      "enterprise_policy_state": "delegate_to_teams_and_apps",
      "editable": true
    }
  ]
}
```
Use this endpoint to set Copilot model policies for agent apps. Set a
model to `enabled` to configure an apps policy, or to `optional` to remove
the stored apps policy for that model. Locked models cannot be updated.

Returns the apps policy state representation, including enterprise policy and editability. These states are not a final access decision for a user.

Enterprise owners and users with the `write_enterprise_ai_controls` permission can use this endpoint.

OAuth app tokens and personal access tokens (classic) need the `admin:enterprise` scope to use this endpoint.

This endpoint works with the following fine-grained token types:

The fine-grained token must have the following permission set:

- "Enterprise AI controls" enterprise permissions (write)

| Headers | 
|---|
| Name, Type, Description | 
|---|
| `accept` stringSetting to `application/vnd.github+json` is recommended. | 

| Path parameters | 
|---|
| Name, Type, Description | 
|---|
| `enterprise` string RequiredThe slug version of the enterprise name. | 

| Body parameters | 
|---|
| Name, Type, Description | 
|---|
| `models` array of objects Required | 
| Properties of `models` | 

| Name, Type, Description | 
|---|
| `model_name` string Required | 
| `policy_state` string RequiredUse `optional` to remove the apps policy. Can be one of: `enabled` ,`disabled` ,`optional` | 

| Status code | Description | 
|---|---|
| `200` | OK | 
| `401` | Requires authentication | 
| `403` | Forbidden | 
| `404` | Resource not found | 
| `422` | Validation failed, or the endpoint has been spammed. | 
| `503` | Service unavailable | 

If you access GitHub at GHE.com, replace `api.github.com` with your enterprise's dedicated subdomain at `api.SUBDOMAIN.ghe.com`.

#### Request example

```
curl -L \
  -X PATCH \
  -H "Accept: application/vnd.github+json" \
  -H "Authorization: Bearer <YOUR-TOKEN>" \
  -H "X-GitHub-Api-Version: 2026-03-10" \
  https://api.github.com/enterprises/ENTERPRISE/copilot/apps/model-policies \
  -d '{"models":[{"model_name":"gpt-5.6-sol","policy_state":"enabled"}]}'
```
OK

`Status: 200````
{
  "models": [
    {
      "model_name": "gpt-5.6-sol",
      "display_name": "OpenAI GPT-5.6 Sol",
      "policy_state": "enabled",
      "enterprise_policy_state": "delegate_to_teams_and_apps",
      "editable": true
    }
  ]
}
```
Note

This endpoint is in public preview and is subject to change.

Use this endpoint to retrieve the Copilot default availability policy for new features and clients
for an enterprise. A policy state of `delegate_to_organizations` means each organization sets its
own default.

Enterprise owners and users with the `read_enterprise_ai_controls` permission can use this endpoint.

OAuth app tokens and personal access tokens (classic) need the `read:enterprise` scope to use this endpoint.

This endpoint works with the following fine-grained token types:

The fine-grained token must have the following permission set:

- "Enterprise AI controls" enterprise permissions (read)

| Headers | 
|---|
| Name, Type, Description | 
|---|
| `accept` stringSetting to `application/vnd.github+json` is recommended. | 

| Path parameters | 
|---|
| Name, Type, Description | 
|---|
| `enterprise` string RequiredThe slug version of the enterprise name. | 

| Status code | Description | 
|---|---|
| `200` | OK | 
| `401` | Requires authentication | 
| `403` | Forbidden | 
| `404` | Resource not found | 

If you access GitHub at GHE.com, replace `api.github.com` with your enterprise's dedicated subdomain at `api.SUBDOMAIN.ghe.com`.

#### Request example

```
curl -L \
  -H "Accept: application/vnd.github+json" \
  -H "Authorization: Bearer <YOUR-TOKEN>" \
  -H "X-GitHub-Api-Version: 2026-03-10" \
  https://api.github.com/enterprises/ENTERPRISE/copilot/feature-policies
```
OK

`Status: 200````
{
  "default_availability": {
    "policy_state": "enabled"
  }
}
```
Note

This endpoint is in public preview and is subject to change.

Use this endpoint to update the Copilot default availability policy for new features and clients
for an enterprise. Use `delegate_to_organizations` to let each organization set its own default.
Enterprises without organizations cannot use `delegate_to_organizations`.

Changing this policy does not change any Copilot feature or client policy that has already been set.

Enterprise owners and users with the `write_enterprise_ai_controls` permission can use this endpoint.

OAuth app tokens and personal access tokens (classic) need the `admin:enterprise` scope to use this endpoint.

This endpoint works with the following fine-grained token types:

The fine-grained token must have the following permission set:

- "Enterprise AI controls" enterprise permissions (write)

| Headers | 
|---|
| Name, Type, Description | 
|---|
| `accept` stringSetting to `application/vnd.github+json` is recommended. | 

| Path parameters | 
|---|
| Name, Type, Description | 
|---|
| `enterprise` string RequiredThe slug version of the enterprise name. | 

| Body parameters | 
|---|
| Name, Type, Description | 
|---|
| `default_availability` object Required | 
| Properties of `default_availability` | 

| Name, Type, Description | 
|---|
| `policy_state` string RequiredThe enterprise default policy applied to newly released Copilot features and clients. `delegate_to_organizations` is only accepted for enterprises that manage Copilot through organizations. Can be one of: `enabled` ,`disabled` ,`delegate_to_organizations` | 

| Status code | Description | 
|---|---|
| `200` | OK | 
| `401` | Requires authentication | 
| `403` | Forbidden | 
| `404` | Resource not found | 
| `422` | Validation failed, or the endpoint has been spammed. | 

If you access GitHub at GHE.com, replace `api.github.com` with your enterprise's dedicated subdomain at `api.SUBDOMAIN.ghe.com`.

#### Request example

```
curl -L \
  -X PATCH \
  -H "Accept: application/vnd.github+json" \
  -H "Authorization: Bearer <YOUR-TOKEN>" \
  -H "X-GitHub-Api-Version: 2026-03-10" \
  https://api.github.com/enterprises/ENTERPRISE/copilot/feature-policies \
  -d '{"default_availability":{"policy_state":"enabled"}}'
```
OK

`Status: 200````
{
  "default_availability": {
    "policy_state": "enabled"
  }
}
```
Gets model policy states for the enterprise GitHub Copilot Fusion layer and the separately read enterprise-layer policies. Identifies which models the Copilot Fusion layer can edit based on the enterprise policy.

Editable models show the Copilot Fusion layer's configured value. When the Copilot Fusion layer has not set a value, they show `enabled`,
because Copilot Fusion only restricts models it explicitly disables.
Locked models show the enterprise's explicit value or the applied default availability for eligible models.
When the default availability is applied and delegated to organizations, models that follow it show `enabled`.
Models not supported by Copilot Fusion are omitted.
These states are not a final access decision for a user. An `enabled` Copilot Fusion policy does not grant access to a model
that the user's applicable model policies deny.

Enterprise owners and users with the `read_enterprise_ai_controls` permission can use this endpoint.

OAuth app tokens and personal access tokens (classic) need the `read:enterprise` scope to use this endpoint.

This endpoint works with the following fine-grained token types:

The fine-grained token must have the following permission set:

- "Enterprise AI controls" enterprise permissions (read)

| Headers | 
|---|
| Name, Type, Description | 
|---|
| `accept` stringSetting to `application/vnd.github+json` is recommended. | 

| Path parameters | 
|---|
| Name, Type, Description | 
|---|
| `enterprise` string RequiredThe slug version of the enterprise name. | 

| Status code | Description | 
|---|---|
| `200` | OK | 
| `401` | Requires authentication | 
| `403` | Forbidden | 
| `404` | Resource not found | 
| `503` | Service unavailable | 

If you access GitHub at GHE.com, replace `api.github.com` with your enterprise's dedicated subdomain at `api.SUBDOMAIN.ghe.com`.

#### Request examples

```
curl -L \
  -H "Accept: application/vnd.github+json" \
  -H "Authorization: Bearer <YOUR-TOKEN>" \
  -H "X-GitHub-Api-Version: 2026-03-10" \
  https://api.github.com/enterprises/ENTERPRISE/copilot/fusion/model-policies
```
Explicit Fusion disable under enterprise delegation

`Status: 200````
{
  "models": [
    {
      "model_name": "gpt-5.6-sol",
      "display_name": "OpenAI GPT-5.6 Sol",
      "policy_state": "disabled",
      "enterprise_policy_state": "delegate_to_teams_and_apps",
      "editable": true
    }
  ]
}
```
Updates editable model policies for the enterprise GitHub Copilot Fusion layer.

`enabled` and `disabled` set a Copilot Fusion policy.
An `enabled` policy adds no Copilot Fusion restriction; it does not grant access to a model that the user's applicable model policies deny.
A `disabled` policy prevents Copilot Fusion from using that model.
Locked models and models not supported by Copilot Fusion cannot be updated. Models not supported by Copilot Fusion are omitted from the response.

Returns the Copilot Fusion policy state representation, including enterprise policy and editability. These states are not a final access decision for a user.

Enterprise owners and users with the `write_enterprise_ai_controls` permission can use this endpoint.

OAuth app tokens and personal access tokens (classic) need the `admin:enterprise` scope to use this endpoint.

This endpoint works with the following fine-grained token types:

The fine-grained token must have the following permission set:

- "Enterprise AI controls" enterprise permissions (write)

| Headers | 
|---|
| Name, Type, Description | 
|---|
| `accept` stringSetting to `application/vnd.github+json` is recommended. | 

| Path parameters | 
|---|
| Name, Type, Description | 
|---|
| `enterprise` string RequiredThe slug version of the enterprise name. | 

| Body parameters | 
|---|
| Name, Type, Description | 
|---|
| `models` array of objects Required | 
| Properties of `models` | 

| Name, Type, Description | 
|---|
| `model_name` string Required | 
| `policy_state` string RequiredCan be one of: `enabled` ,`disabled` | 

| Status code | Description | 
|---|---|
| `200` | OK | 
| `401` | Requires authentication | 
| `403` | Forbidden | 
| `404` | Resource not found | 
| `422` | Validation failed, or the endpoint has been spammed. | 
| `503` | Service unavailable | 

If you access GitHub at GHE.com, replace `api.github.com` with your enterprise's dedicated subdomain at `api.SUBDOMAIN.ghe.com`.

#### Request examples

```
curl -L \
  -X PATCH \
  -H "Accept: application/vnd.github+json" \
  -H "Authorization: Bearer <YOUR-TOKEN>" \
  -H "X-GitHub-Api-Version: 2026-03-10" \
  https://api.github.com/enterprises/ENTERPRISE/copilot/fusion/model-policies \
  -d '{"models":[{"model_name":"claude-sonnet-4","policy_state":"enabled"}]}'
```
OK

`Status: 200````
{
  "models": [
    {
      "model_name": "claude-sonnet-4",
      "display_name": "Anthropic Claude Sonnet 4",
      "policy_state": "enabled",
      "enterprise_policy_state": "delegate_to_teams_and_apps",
      "editable": true
    }
  ]
}
```
Use this endpoint to retrieve Copilot model policies for an enterprise.
For enterprises that manage Copilot through organizations, delegated models use the
policy state of `delegate_to_organizations`.

Enterprise owners and users with the `read_enterprise_ai_controls` permission can use this endpoint.

OAuth app tokens and personal access tokens (classic) need the `read:enterprise` scope to use this endpoint.

This endpoint works with the following fine-grained token types:

The fine-grained token must have the following permission set:

- "Enterprise AI controls" enterprise permissions (read)

| Headers | 
|---|
| Name, Type, Description | 
|---|
| `accept` stringSetting to `application/vnd.github+json` is recommended. | 

| Path parameters | 
|---|
| Name, Type, Description | 
|---|
| `enterprise` string RequiredThe slug version of the enterprise name. | 

| Status code | Description | 
|---|---|
| `200` | OK | 
| `401` | Requires authentication | 
| `403` | Forbidden | 
| `404` | Resource not found | 
| `503` | Service unavailable | 

If you access GitHub at GHE.com, replace `api.github.com` with your enterprise's dedicated subdomain at `api.SUBDOMAIN.ghe.com`.

#### Request example

```
curl -L \
  -H "Accept: application/vnd.github+json" \
  -H "Authorization: Bearer <YOUR-TOKEN>" \
  -H "X-GitHub-Api-Version: 2026-03-10" \
  https://api.github.com/enterprises/ENTERPRISE/copilot/model-policies
```
OK

`Status: 200````
{
  "default_availability": {
    "policy_state": "enabled"
  },
  "models": [
    {
      "model_name": "gpt-5.6-sol",
      "display_name": "OpenAI GPT-5.6 Sol",
      "policy_state": "enabled"
    }
  ]
}
```
Use this endpoint to update Copilot model policies for an enterprise.
For enterprises that manage Copilot through organizations, use
`delegate_to_organizations` to delegate a model or default availability.
Enterprises without organizations cannot use `delegate_to_organizations`.

Enterprise owners and users with the `write_enterprise_ai_controls` permission can use this endpoint.

OAuth app tokens and personal access tokens (classic) need the `admin:enterprise` scope to use this endpoint.

This endpoint works with the following fine-grained token types:

The fine-grained token must have the following permission set:

- "Enterprise AI controls" enterprise permissions (write)

| Headers | 
|---|
| Name, Type, Description | 
|---|
| `accept` stringSetting to `application/vnd.github+json` is recommended. | 

| Path parameters | 
|---|
| Name, Type, Description | 
|---|
| `enterprise` string RequiredThe slug version of the enterprise name. | 

| Body parameters | 
|---|
| Name, Type, Description | 
|---|
| `default_availability` object | 
| Properties of `default_availability` | 
| `models` array of objects | 
| Properties of `models` | 

| Name, Type, Description | 
|---|
| `policy_state` string Required`delegate_to_organizations` is only accepted for enterprises that manage Copilot through organizations. Can be one of: `enabled` ,`disabled` ,`delegate_to_organizations` | 

| Name, Type, Description | 
|---|
| `model_name` string Required | 
| `policy_state` string Required`delegate_to_organizations` is only accepted for enterprises that manage Copilot through organizations. Can be one of: `enabled` ,`disabled` ,`delegate_to_teams_and_apps` ,`delegate_to_organizations` | 

| Status code | Description | 
|---|---|
| `200` | OK | 
| `401` | Requires authentication | 
| `403` | Forbidden | 
| `404` | Resource not found | 
| `422` | Validation failed, or the endpoint has been spammed. | 
| `503` | Service unavailable | 

If you access GitHub at GHE.com, replace `api.github.com` with your enterprise's dedicated subdomain at `api.SUBDOMAIN.ghe.com`.

#### Request example

```
curl -L \
  -X PATCH \
  -H "Accept: application/vnd.github+json" \
  -H "Authorization: Bearer <YOUR-TOKEN>" \
  -H "X-GitHub-Api-Version: 2026-03-10" \
  https://api.github.com/enterprises/ENTERPRISE/copilot/model-policies \
  -d '{"default_availability":{"policy_state":"enabled"},"models":[{"model_name":"gpt-5.6-sol","policy_state":"enabled"}]}'
```
OK

`Status: 200````
{
  "default_availability": {
    "policy_state": "enabled"
  },
  "models": [
    {
      "model_name": "gpt-5.6-sol",
      "display_name": "OpenAI GPT-5.6 Sol",
      "policy_state": "enabled"
    }
  ]
}
```
Use this endpoint to retrieve Copilot model policy states for an enterprise team, along with the corresponding enterprise-level policies. The response shows which model policies the team can update based on enterprise policy and the available team policy controls.

For models the team can update, the response shows the team’s configured
value or `optional` if the team has not set one. For locked models, it shows
the enterprise’s explicit value or the applicable default availability for
eligible models. These states are not a final access decision for a user.

Enterprise owners and users with the `read_enterprise_ai_controls` permission can use this endpoint.

OAuth app tokens and personal access tokens (classic) need the `read:enterprise` scope to use this endpoint.

This endpoint works with the following fine-grained token types:

The fine-grained token must have the following permission set:

- "Enterprise AI controls" enterprise permissions (read)

| Headers | 
|---|
| Name, Type, Description | 
|---|
| `accept` stringSetting to `application/vnd.github+json` is recommended. | 

| Path parameters | 
|---|
| Name, Type, Description | 
|---|
| `enterprise` string RequiredThe slug version of the enterprise name. | 
| `team_id` integer RequiredThe unique identifier of the team. | 

| Status code | Description | 
|---|---|
| `200` | OK | 
| `401` | Requires authentication | 
| `403` | Forbidden | 
| `404` | Resource not found | 
| `503` | Service unavailable | 

If you access GitHub at GHE.com, replace `api.github.com` with your enterprise's dedicated subdomain at `api.SUBDOMAIN.ghe.com`.

#### Request examples

```
curl -L \
  -H "Accept: application/vnd.github+json" \
  -H "Authorization: Bearer <YOUR-TOKEN>" \
  -H "X-GitHub-Api-Version: 2026-03-10" \
  https://api.github.com/enterprises/ENTERPRISE/teams/TEAM_ID/copilot/model-policies
```
OK

`Status: 200````
{
  "models": [
    {
      "model_name": "gpt-5.6-sol",
      "display_name": "OpenAI GPT-5.6 Sol",
      "policy_state": "optional",
      "enterprise_policy_state": "delegate_to_teams_and_apps",
      "editable": true
    }
  ]
}
```
Use this endpoint to update Copilot model policies for an enterprise team. Only editable model policies can be updated.

Set a model to `enabled` to configure a team policy, or to `optional` to clear
the team’s policy for that model. Locked models cannot be updated.

The response contains the same policy state information as the corresponding GET endpoint, including enterprise policies and whether each model policy can be updated. These states do not necessarily determine whether an individual user can access a model.

Enterprise owners and users with the `write_enterprise_ai_controls` permission can use this endpoint.

OAuth app tokens and personal access tokens (classic) need the `admin:enterprise` scope to use this endpoint.

This endpoint works with the following fine-grained token types:

The fine-grained token must have the following permission set:

- "Enterprise AI controls" enterprise permissions (write)

| Headers | 
|---|
| Name, Type, Description | 
|---|
| `accept` stringSetting to `application/vnd.github+json` is recommended. | 

| Path parameters | 
|---|
| Name, Type, Description | 
|---|
| `enterprise` string RequiredThe slug version of the enterprise name. | 
| `team_id` integer RequiredThe unique identifier of the team. | 

| Body parameters | 
|---|
| Name, Type, Description | 
|---|
| `models` array of objects Required | 
| Properties of `models` | 

| Name, Type, Description | 
|---|
| `model_name` string Required | 
| `policy_state` string RequiredUse `optional` to remove the team's policy. Can be one of: `enabled` ,`disabled` ,`optional` | 

| Status code | Description | 
|---|---|
| `200` | OK | 
| `401` | Requires authentication | 
| `403` | Forbidden | 
| `404` | Resource not found | 
| `422` | Validation failed, or the endpoint has been spammed. | 
| `503` | Service unavailable | 

If you access GitHub at GHE.com, replace `api.github.com` with your enterprise's dedicated subdomain at `api.SUBDOMAIN.ghe.com`.

#### Request example

```
curl -L \
  -X PATCH \
  -H "Accept: application/vnd.github+json" \
  -H "Authorization: Bearer <YOUR-TOKEN>" \
  -H "X-GitHub-Api-Version: 2026-03-10" \
  https://api.github.com/enterprises/ENTERPRISE/teams/TEAM_ID/copilot/model-policies \
  -d '{"models":[{"model_name":"gpt-5.6-sol","policy_state":"enabled"}]}'
```
OK

`Status: 200````
{
  "models": [
    {
      "model_name": "gpt-5.6-sol",
      "display_name": "OpenAI GPT-5.6 Sol",
      "policy_state": "enabled",
      "enterprise_policy_state": "delegate_to_teams_and_apps",
      "editable": true
    }
  ]
}
```
