---
name: cftunnel
description: Operate and maintain Kyndlo's cftunnel Cloudflare Tunnel helper from Codex. Use when the user asks about kyndlo/cftunnel, Cloudflare tunnel setup, tunnel config review, local tunnel troubleshooting, cloudflared automation, or safe changes to tunnel/DNS/Access resources.
---

# Cftunnel

Use this skill when working with Kyndlo's `cftunnel` repository or any local checkout of that tool.

## Orientation

1. Locate the checkout before assuming commands. Prefer the current workspace, then `/Volumes/DATA/kyndlo/cftunnel`, then ask the user for the path or repository access.
2. Read the repository's `README`, CLI help output, and relevant source files before invoking create, delete, route, DNS, or Access operations.
3. If the GitHub repository is inaccessible from Codex, say so clearly and work from a local checkout or pasted files. Do not fabricate exact command flags.
4. Treat `cloudflared` as the underlying tunnel client unless the repository documentation says otherwise.

## Safety Rules

- Do not print Cloudflare API tokens, tunnel credentials, origin certs, or private key material. Show only variable names, token prefixes, file paths, or redacted values.
- Before changing Cloudflare resources, summarize the target account/zone/domain, tunnel name, hostname, and action.
- Ask for explicit approval before deleting tunnels, deleting DNS records, rotating credentials, changing Access policies, or exposing a new public hostname.
- Prefer dry-run, preview, list, validate, or config-only commands when the tool supports them.
- Keep local generated config, credential, and log files out of git unless the repository explicitly tracks templates for them.

## Common Workflow

1. Inspect the environment:
   - Confirm `cloudflared` is installed with `cloudflared --version`.
   - Check for required environment variables using redacted output.
   - Confirm the local app port is listening before debugging a tunnel.
2. Inspect cftunnel:
   - Read project docs and command help.
   - Identify whether the implementation is shell, Python, Node, or another runtime.
   - Use the repository's own tests, linters, or validation commands when present.
3. Make narrow changes:
   - Edit only the cftunnel repo or requested config files.
   - Preserve existing tunnel names, domains, subdomains, and Access policy intent unless the user asks to change them.
   - Prefer structured config edits over shell string rewrites when the config format supports it.
4. Verify:
   - Run the relevant test or lint command.
   - For runtime troubleshooting, verify local service reachability first, then tunnel status, then DNS/hostname routing.
   - Report any Cloudflare-side action that still needs the user's authenticated environment.

## Troubleshooting Heuristics

- Local service failure usually appears before tunnel failure. Check the local port and protocol first.
- DNS or hostname issues usually require confirming the Cloudflare zone and route for the public hostname.
- Access/Zero Trust failures usually require checking the Access application, policy selectors, and bypass paths.
- Credential errors usually require re-authentication or a token with the permissions documented by the repository.

## Output Expectations

- Lead with what was inspected or changed.
- Include the exact local files edited and commands run.
- For blocked Cloudflare operations, state the missing credential, permission, or repository access without exposing secrets.
