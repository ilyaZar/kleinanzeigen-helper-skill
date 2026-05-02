---
name: kleinanzeigen-helper-skill
description: Manage Kleinanzeigen listings with the required helper plugin.
license: MIT-0
---

# Kleinanzeigen Helper

Use this skill when the user wants help preparing, verifying, publishing,
updating, deleting, downloading, or extending Kleinanzeigen listings through
their already configured local `kleinanzeigen-bot` setup.

## Prerequisites

- This is an instruction-only companion skill for the
  `kleinanzeigen-helper` OpenClaw plugin.
- Install and enable the `kleinanzeigen-helper` OpenClaw plugin before running
  listing operations.
- The local `kleinanzeigen-bot` setup stays outside this skill bundle. Users
  configure it in OpenClaw or plugin config, not in chat.

## Security Model

- The skill ships no scripts, installers, background processes, or credential
  requirements.
- The plugin owns subprocess execution, configured path handling, confirmation
  gates, and output redaction.
- This skill is unofficial and is not affiliated with Kleinanzeigen. The user
  remains responsible for platform terms and listing content.

## Non-negotiables

- Use only the Kleinanzeigen plugin tools for bot operations.
- Do not use `exec`, browser automation, direct HTTP requests, or shell commands
  for Kleinanzeigen work.
- Do not ask for, read, print, summarize, store, inspect, or infer
  Kleinanzeigen usernames, passwords, SMS or 2FA codes, cookies, browser
  profiles, session data, or credential-bearing config files.
- Do not ask the user to paste `config.yaml`, browser settings, cookies, logs,
  or credential-like snippets.
- Treat any account-changing operation as sensitive. Get explicit user
  confirmation before using a side-effect tool.

## Workflow

1. Clarify the listing intent and selector using only non-secret details:
   listing title, category, price, description text, image readiness, and
   whether the target is all ads, new ads, changed ads, due ads, or specific ad
   IDs.
2. Run `kleinanzeigen_verify` before any side-effect operation when practical.
3. For publish, update, delete, download, or extend, summarize the exact
   non-secret action and wait for explicit user confirmation.
4. If the Kleinanzeigen plugin tools are unavailable, stop and tell the user
   the `kleinanzeigen-helper` OpenClaw plugin is required.
5. Call the matching optional plugin tool only after confirmation:
   `kleinanzeigen_publish`, `kleinanzeigen_update`, `kleinanzeigen_delete`,
   `kleinanzeigen_download`, or `kleinanzeigen_extend`.
6. Report only the sanitized result returned by the tool. If the result says
   configuration or credentials need attention, ask the user to fix that outside
   the chat and rerun verification.

## Tool Selection

- `kleinanzeigen_verify`: validate the already configured local bot setup.
- `kleinanzeigen_publish`: publish or republish due, new, changed, all, or
  explicitly selected ads.
- `kleinanzeigen_update`: update changed or explicitly selected ads.
- `kleinanzeigen_delete`: delete only explicitly selected ad IDs.
- `kleinanzeigen_download`: download new, all, or explicitly selected ads.
- `kleinanzeigen_extend`: extend all eligible ads or explicitly selected IDs.

If a side-effect tool is unavailable, tell the user that the optional tool must
be enabled in OpenClaw before that account-changing action can run.
