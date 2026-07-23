---
name: Onboard a model into Arthur monitoring
description: Create a workspace/project, register a model, connect a data source, and wire alerting so an AI/ML application is monitored end-to-end on the Arthur Platform.
api: openapi/arthur-ai-platform-openapi.json
operations:
  - post_workspace
  - post_project
  - post_model
  - post_connector
  - post_connector_dataset
  - post_model_alert_rule
---

# Onboard a model into Arthur monitoring

Use the Arthur Platform API (`https://platform.arthur.ai/api`) to stand up monitoring for a new model.

## Auth
OAuth2 via Keycloak (`https://platform-auth.arthur.ai/realms/arthur`). For automation use the service-account **client credentials** flow; pass the bearer token on every request. See `authentication/arthur-ai-authentication.yml`.

## Steps
1. `post_workspace` — create a workspace under your organization (`POST /api/v1/organization/workspaces`).
2. `post_project` — create a project inside the workspace (`POST /api/v1/workspaces/{workspace_id}/projects`).
3. `post_model` — register the model in the project (`POST /api/v1/projects/{project_id}/models`).
4. `post_connector` — attach a data source to the project (`POST /api/v1/projects/{project_id}/connectors`) so Arthur can read inference/ground-truth data (BigQuery, S3, GCS).
5. `post_connector_dataset` — bind a dataset to the connector (`POST /api/v1/connectors/{connector_id}/datasets`).
6. `post_model_alert_rule` — add an alert rule to the model (`POST /api/v1/models/{model_id}/alert_rules`).

## Conventions
- List endpoints paginate with `page` / `page_size` (+ `sort`, `order`). See `conventions/arthur-ai-conventions.yml`.
- Errors: 422 returns a Pydantic `detail[]` validation array; other failures return `{"detail": "..."}`. See `errors/arthur-ai-problem-types.yml`.
- No idempotency-key contract — do not retry non-idempotent POSTs blindly.
