---
id: collect-261121-veille-ia/copilot-microsoft/rest-api-endpoints-for-copilot-model-policy-management-github-enterprise-cloud-docs
title: "REST API endpoints for Copilot model policy management - GitHub Enterprise Cloud Docs"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["model", "organization", "policy", "github", "access", "name", "enterprise", "tokens"]
source: https://docs.github.com/en/enterprise-cloud@latest/rest/copilot/copilot-model-policy-management
source_anchor: ""
source_lines: [1, 175]
section: "Copilot_Microsoft"
---
# REST API endpoints for Copilot model policy management

Use the REST API to manage Copilot model policies for an organization.

Use this endpoint to retrieve the effective Copilot model policy for every model available to an organization, whether explicitly configured or determined by default availability.

For a model without an effective explicit policy that is eligible for default availability, the enterprise default is used when it is configured. Otherwise, the organization's default is used. A model that is not eligible for default availability and has no effective explicit policy is disabled. For more information, see About default availability of Copilot models.

The organization must either be independent or belong to an enterprise that is not using enterprise teams for model policy.

Organization owners can use this endpoint.

OAuth app tokens and personal access tokens (classic) need the `admin:org` or `repo` scope to use this endpoint.
Organization SAML authorization and OAuth app access restrictions also apply.

This endpoint works with the following fine-grained token types:

- GitHub App user access tokens
- GitHub App installation access tokens
- Fine-grained personal access tokens

The fine-grained token must have the following permission set:

- "Administration" organization permissions (write)

| Headers | 
|---|
| Name, Type, Description | 
|---|
| `accept` stringSetting to `application/vnd.github+json` is recommended. | 

| Path parameters | 
|---|
| Name, Type, Description | 
|---|
| `org` string RequiredThe organization name. The name is not case sensitive. | 

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
  https://api.github.com/orgs/ORG/copilot/model-policies
```
OK

`Status: 200````
{
  "default_availability": {
    "policy_state": "disabled",
    "editable": true
  },
  "models": [
    {
      "model_name": "gpt-5.6-sol",
      "display_name": "OpenAI GPT-5.6 Sol",
      "policy_state": "enabled"
    },
    {
      "model_name": "gpt-5",
      "display_name": "GPT-5",
      "policy_state": "disabled"
    }
  ]
}
```
Use this endpoint to set explicit Copilot model policies and default availability for an organization. All policies in the request are updated atomically, meaning if any change in the request cannot be applied, none of the changes are saved.

Use the `model_name` values returned by the "Get model policies for an organization"
endpoint. Models with an inherited enterprise policy are locked and cannot be updated.
Default availability cannot be updated when its `editable` value is `false`.

The response contains the effective policies for all models available to the organization, not only the policies updated by the request.

The organization must either be independent or belong to an enterprise that is not using enterprise teams for model policy.

Organization owners can use this endpoint.

OAuth app tokens and personal access tokens (classic) need the `admin:org` or `repo` scope to use this endpoint.
Organization SAML authorization and OAuth app access restrictions apply.

This endpoint works with the following fine-grained token types:

- GitHub App user access tokens
- GitHub App installation access tokens
- Fine-grained personal access tokens

The fine-grained token must have the following permission set:

- "Administration" organization permissions (write)

| Headers | 
|---|
| Name, Type, Description | 
|---|
| `accept` stringSetting to `application/vnd.github+json` is recommended. | 

| Path parameters | 
|---|
| Name, Type, Description | 
|---|
| `org` string RequiredThe organization name. The name is not case sensitive. | 

| Body parameters | 
|---|
| Name, Type, Description | 
|---|
| `default_availability` objectThe organization's default availability for eligible models without an explicit policy. It cannot be updated when it is inherited from the enterprise. | 
| Properties of `default_availability` | 
| `models` array of objectsThe model policies to update. Each `model_name` must be unique. | 
| Properties of `models` | 

| Name, Type, Description | 
|---|
| `policy_state` string RequiredCan be one of: `enabled` ,`disabled` | 

| Name, Type, Description | 
|---|
| `model_name` string RequiredThe public identifier of a model returned by the get endpoint. | 
| `policy_state` string RequiredThe explicit policy state to set for the organization. Can be one of: `enabled` ,`disabled` | 

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
  https://api.github.com/orgs/ORG/copilot/model-policies \
  -d '{"default_availability":{"policy_state":"disabled"},"models":[{"model_name":"gpt-5.6-sol","policy_state":"enabled"},{"model_name":"gpt-5","policy_state":"disabled"}]}'
```
OK

`Status: 200````
{
  "default_availability": {
    "policy_state": "disabled",
    "editable": true
  },
  "models": [
    {
      "model_name": "gpt-5.6-sol",
      "display_name": "OpenAI GPT-5.6 Sol",
      "policy_state": "enabled"
    },
    {
      "model_name": "gpt-5",
      "display_name": "GPT-5",
      "policy_state": "disabled"
    }
  ]
}
```
