# Kyndlo Codex Plugins

Public Codex plugin marketplace for Kyndlo.

## Install

Add the marketplace:

```bash
codex plugin marketplace add kyndlo/kyndlo-codex-plugins --ref main
```

Install the Kyndlo plugin:

```bash
codex plugin add kyndlo@kyndlo
```

Confirm it is installed:

```bash
codex plugin list
codex mcp list
```

## Authenticate

Create a scoped Kyndlo API token in the Kyndlo dashboard under Agent Access or API Tokens.

For Codex desktop on macOS, set the token in the macOS launch environment, quit Codex completely, and reopen it:

```bash
launchctl setenv KYNDLO_API_TOKEN "kyndlo_..."
```

For Codex CLI, pass the token only to the Codex process:

```bash
KYNDLO_API_TOKEN="kyndlo_..." codex
```

## Plugin

- `kyndlo`: dashboard, events, validation, moderation, agent access, and admin workflows through `https://api.kyndlo.com/mcp`

High-risk mutations use Kyndlo's preview-confirm flow when exposed by MCP.
