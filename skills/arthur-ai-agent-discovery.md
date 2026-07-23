---
name: Discover and register AI agents for governance
description: Scan the organization for unregistered AI agents, review their tools/models/data sources, and register them into a workspace under Arthur governance.
api: openapi/arthur-ai-platform-openapi.json
operations:
  - list_unregistered_agents_for_organization
  - list_unregistered_agents_for_workspace
  - put_agents
  - get_registered_agent_by_id
  - list_tools_for_workspace
---

# Discover and register AI agents for governance

Arthur's Agent Discovery automatically finds AI agents running outside your governance controls so you can register or mute them.

## Auth
OAuth2 bearer token (Keycloak). See `authentication/arthur-ai-authentication.yml`.

## Steps
1. `list_unregistered_agents_for_organization` — find agents discovered across the org but not yet governed (`GET /api/v1/organization/unregistered_agents`).
2. `list_unregistered_agents_for_workspace` — narrow to a workspace (`GET /api/v1/workspaces/{workspace_id}/agents/unregistered`).
3. `put_agents` — register selected agents into the workspace (`PUT /api/v1/workspaces/{workspace_id}/agents`).
4. `get_registered_agent_by_id` — confirm registration (`GET /api/v1/agents/registered/{agent_id}`).
5. `list_tools_for_workspace` — audit the tools the registered agents expose (`GET /api/v1/workspaces/{workspace_id}/agents/registered/tools`).

## Notes
- Pagination via `page`/`page_size`; filter helpers include `sub_agent_names`, `tool_names`, `llm_model_names`, `data_source_urls`.
- See `conventions/arthur-ai-conventions.yml` and `errors/arthur-ai-problem-types.yml`.
