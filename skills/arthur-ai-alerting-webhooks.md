---
name: Configure alerting and webhook notifications
description: Create an alert rule on a model, register a workspace webhook, test delivery, and route fired alerts to Slack/Jira/PagerDuty on the Arthur Platform.
api: openapi/arthur-ai-platform-openapi.json
operations:
  - post_model_alert_rule
  - post_alert_rule_query_validation
  - post_webhook
  - post_test_webhook
  - get_model_alerts
---

# Configure alerting and webhook notifications

## Auth
OAuth2 bearer token (Keycloak). See `authentication/arthur-ai-authentication.yml`.

## Steps
1. `post_alert_rule_query_validation` — validate the alert-rule query against the model before saving (`POST /api/v1/models/{model_id}/alert_rule_query_validation`).
2. `post_model_alert_rule` — create the alert rule on the model (`POST /api/v1/models/{model_id}/alert_rules`).
3. `post_webhook` — create a workspace webhook pointing at your destination URL (`POST /api/v1/workspaces/{workspace_id}/webhooks`). The body is a Jinja2 template Arthur renders with alert context.
4. `post_test_webhook` — send a test POST and inspect the status/body before relying on it (`POST /api/v1/workspaces/{workspace_id}/webhooks/test`).
5. Attach the webhook to the alert rule (via `patch_alert_rule`) so `alert.fired` deliveries route to Slack/Jira/PagerDuty.
6. `get_model_alerts` — read fired alerts for the model (`GET /api/v1/models/{model_id}/alerts`).

## Notes
- Webhooks are workspace-level and reusable across many alert rules; delivery is outbound HTTP POST. See `asyncapi/arthur-ai-webhooks.yml`.
- Errors and pagination follow the shared conventions (`conventions/arthur-ai-conventions.yml`).
