# Kyndlo Codex Plugin

Codex-native access to Kyndlo through the production MCP endpoint.

## What It Provides

- MCP server: `kyndlo` at `https://api.kyndlo.com/mcp`
- Skills:
  - `kyndlo-dashboard` for dashboard/admin parity workflows
  - `kyndlo-events` for event creation, tasks, validation, activities, and images
- Preview-confirm guidance for high-risk dashboard mutations
- Purpose-built admin event tools for monitoring, enable/disable, cancellation, activity assignment, guided experience generation, and guided assignment

## Authentication

The plugin uses a scoped Kyndlo API token. Codex reads that token from an environment variable named `KYNDLO_API_TOKEN` and sends it to Kyndlo MCP as a bearer token.

Create the token in Kyndlo first:

1. Open the Kyndlo dashboard.
2. Go to Agent Access or API Tokens.
3. Create a token with the access group/scopes the agent should have.
4. Copy the token when it is shown. It will not be shown again.

Then make the token available to Codex.

For the Codex desktop app on macOS, set it in the macOS launch environment, quit Codex completely, and reopen it:

```bash
launchctl setenv KYNDLO_API_TOKEN "kyndlo_..."
```

To remove it later:

```bash
launchctl unsetenv KYNDLO_API_TOKEN
```

For Codex CLI, pass the token only to that command:

```bash
KYNDLO_API_TOKEN="kyndlo_..." codex
```

Or save it in your shell profile if you want every future Terminal session to inherit it:

```bash
echo 'export KYNDLO_API_TOKEN="kyndlo_..."' >> ~/.zshrc
source ~/.zshrc
```

Do not paste full API tokens into chat messages, README files, Git commits, screenshots, or support tickets.

## CLI Fallback

When the `gokyn` CLI is installed, you can validate the same token without storing it globally. `gokyn` can be installed from npm or as a standalone binary for users who do not have Node.js:

```bash
curl -fsSL https://kyndlo.com/install.sh | sh
```

Windows PowerShell:

```powershell
powershell -c "irm https://kyndlo.com/install.ps1 | iex"
```

```bash
npx gokyn --base-url https://api.kyndlo.com --token "kyndlo_..." whoami --json
```

For local API validation, point the CLI at your local API:

```bash
npx gokyn --base-url http://localhost:3001 --token "kyndlo_..." whoami --json
npx gokyn --base-url http://localhost:3001 --token "kyndlo_..." event-admin monitor --issues-only --json
npx gokyn --base-url http://localhost:3001 --token "kyndlo_..." guided generate --name "Arrival Guide" --event-id <eventId> --json
```

## Validation

```bash
python3 /Users/pirumpi/.codex/skills/.system/plugin-creator/scripts/validate_plugin.py ./plugins/kyndlo
codex plugin marketplace add kyndlo/kyndlo-codex-plugins --ref main
codex plugin add kyndlo@kyndlo
codex plugin list
codex mcp list
```
