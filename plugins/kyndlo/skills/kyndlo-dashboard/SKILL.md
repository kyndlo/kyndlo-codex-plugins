---
name: kyndlo-dashboard
description: Operate Kyndlo dashboard and admin workflows through the Kyndlo MCP tools with preview-confirm safety.
---

# Kyndlo Dashboard

Use this skill when the user asks Codex to inspect or operate Kyndlo dashboard/admin data, including users, access groups, agent access, API tokens, OAuth clients, events, event tasks, event validations, activities, quizzes, settings, moderation, contributors, notifications, or dashboard parity workflows.

## Primary Interface

Prefer the bundled Kyndlo MCP server. It connects to `https://api.kyndlo.com/mcp` and uses `KYNDLO_API_TOKEN` as the bearer token.

Start by calling `whoami` when identity, scope, or environment is uncertain. The MCP server only registers tools allowed by the authenticated token, so visible tools are the effective access level.

Use `dashboard-read` for permitted dashboard reads:

```json
{
  "path": "/admin/users",
  "query": {
    "limit": 25
  }
}
```

Use `dashboard-mutate` for dashboard writes. High-risk mutations must be previewed before execution:

```json
{
  "method": "POST",
  "path": "/admin/users/admin-invite",
  "body": {
    "email": "admin@example.com"
  },
  "dryRun": true
}
```

Then execute with the returned `confirmationId` using the exact same method, path, query, and body:

```json
{
  "method": "POST",
  "path": "/admin/users/admin-invite",
  "body": {
    "email": "admin@example.com"
  },
  "confirmationId": "..."
}
```

## Safety Rules

- Treat backend permissions as the source of truth; do not rely on dashboard UI visibility.
- For high-risk actions, never skip preview-confirm. Deletes, suspensions, permission changes, access-group changes, token changes, OAuth secret rotation, settings changes, attendance overrides, payment operations, moderation resolutions, event cancellation/deletion, bulk task/campaign deletion, and notifications should preview first.
- Do not echo full API tokens or secrets. Show token prefixes, names, scopes, and expiration only.
- If a tool returns `confirmationRequired`, explain the preview briefly and ask for explicit user approval before sending the confirmation unless the user already requested execution.
- If a dashboard path is rejected as not exposed through MCP, stop and report the missing route rather than inventing a request.

## CLI Fallback

If MCP tools are unavailable but shell access is available, use `gokyn --json` with `KYNDLO_API_TOKEN`.

```bash
gokyn whoami --json
gokyn dashboard read /admin/users --json
gokyn dashboard mutate /admin/users/admin-invite --method POST --body '{"email":"admin@example.com"}' --preview --json
gokyn dashboard mutate /admin/users/admin-invite --method POST --body '{"email":"admin@example.com"}' --confirm <confirmationId> --json
```

Use `KYNDLO_API_URL` to point the CLI at a local API, for example `http://localhost:3001`.
