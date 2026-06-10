---
name: kyndlo-events
description: Create, inspect, validate, and maintain Kyndlo events through MCP tools or the gokyn CLI.
---

# Kyndlo Events

Use this skill when the user asks Codex to create events, validate events, work event creation tasks, upload or generate event images, inspect activities, or manage event/task/validation workflows.

## Admin Operations

When the request involves event operations after creation, prefer these MCP tools:

- `admin-monitor-events` to find under-enrolled, inactive, missing-activity, cancelled, or missing-guided-experience issues.
- `admin-get-event-operations-summary` before changing a specific event.
- `admin-enable-event` or `admin-disable-event` to change availability.
- `admin-assign-event-activities` to replace assigned activities.
- `admin-generate-guided-experience-template` to draft a guided experience.
- `admin-assign-guided-experience-to-event` or `admin-remove-guided-experience-from-event` to manage event overrides.
- `admin-cancel-event` to operationally cancel an event, cancel active attendance, attempt refunds, and notify attendees.

All high-risk admin event tools require preview-confirm. Preview first, explain the impact, then execute with the returned `confirmationId` only after approval.

## Preferred Flow

1. Call `whoami` first when identity or permissions are uncertain.
2. Use read tools to gather context before mutating data.
3. For event creation tasks, resolve campaign, state, county, city, activity, location, timezone, and recurrence before creating an event.
4. For validation tasks, preserve evidence and status details in the validation payload.
5. Use preview-confirm for high-risk dashboard parity actions, including event deletion/cancellation, attendance overrides, bulk task changes, and moderation or payment operations.

## Useful CLI Fallbacks

Use the `gokyn` CLI when MCP tools are unavailable or when a shell workflow is faster:

```bash
gokyn whoami --json
gokyn activity list --search "yoga" --json
gokyn event list --json
gokyn event get <eventId> --json
gokyn task campaigns --json
gokyn task context --campaign "<campaign>" --city "<city>" --json
gokyn task next --campaign "<campaign>" --city "<city>" --assign --name "<agent-name>" --json
gokyn validation next --assign --json
gokyn event-admin monitor --issues-only --json
gokyn event-admin summary <eventId> --json
gokyn event-admin assign-activities <eventId> --activity <activityId>:0 --preview --json
gokyn event-admin cancel <eventId> --reason admin --preview --json
gokyn guided generate --name "Arrival Guide" --event-id <eventId> --json
gokyn guided assign <eventId> --template-id <templateId> --preview --json
```

Create physical events with explicit flags rather than large JSON blobs when possible:

```bash
gokyn event create \
  --title "Community Yoga" \
  --description "A beginner-friendly class." \
  --activity <activityId>:1 \
  --start-date-time "2026-07-01T15:00:00Z" \
  --end-date-time "2026-07-01T16:00:00Z" \
  --timezone "America/Denver" \
  --location-type physical \
  --location-place "Community Center" \
  --location-address "123 Main St, Denver, CO" \
  --location-lat 39.7392 \
  --location-lng -104.9903 \
  --json
```

Complete or release tasks after creating events:

```bash
gokyn task complete <taskId> --event-id <eventId> --venue "<venue name>" --json
gokyn task release <taskId> --json
```

## Operational Rules

- Do not fabricate venue coordinates, URLs, or activity IDs. Resolve them from Kyndlo data or explicit user input.
- If event creation succeeds but task completion fails, report the event ID and release or flag the task according to the user’s instruction.
- Keep payloads structured and prefer `--json` output for Codex-readable command results.
- Use `KYNDLO_API_TOKEN` for auth and `KYNDLO_API_URL` for local API validation.
