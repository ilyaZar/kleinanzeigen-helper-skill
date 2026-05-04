---
name: kleinanzeigen-helper-skill
description: Manage Kleinanzeigen listings with the required KleinClaw plugin.
license: MIT-0
---

# Kleinanzeigen Helper

Use this skill when the user wants help preparing, verifying, publishing,
updating, deleting, downloading, or extending Kleinanzeigen listings through
their already configured local `kleinanzeigen-bot` setup.

## Prerequisites / Install

- Install and configure
  [kleinanzeigen-bot](https://github.com/Second-Hand-Friends/kleinanzeigen-bot)
  locally first.
- Install [KleinClaw](https://clawhub.ai/plugins/kleinclaw), then enable it:

  ```bash
  openclaw plugins install clawhub:kleinclaw
  openclaw plugins enable kleinclaw
  openclaw gateway restart
  ```

- Configure KleinClaw under `plugins.entries.kleinclaw.config`.
- Keep bot credentials, browser profiles, cookies, and full config files out of
  chat.

## Non-negotiables

- Use only the Kleinanzeigen plugin tools for operations.
- Do not use `exec`, browser automation, direct HTTP requests, or shell commands
  for Kleinanzeigen work.
- Do not ask for, read, print, summarize, store, inspect, or infer Kleinanzeigen
  usernames, passwords, SMS or 2FA codes, cookies, browser profiles, session
  data, or credential-bearing config files.
- Do not ask the user to paste `config.yaml`, browser settings, cookies, logs,
  or credential-like snippets.
- All KleinClaw tools may require OpenClaw approval because they run a local
  command. Account-changing tools also require explicit user confirmation.

## Workflow

1. Clarify the listing intent and selector using only non-secret details:
   listing title, category, price, description text, image readiness, and
   whether the target is all ads, new ads, changed ads, due ads, or specific ad
   IDs.
2. Run `kleinanzeigen_status` first when checking installation, plugin config,
   bot availability, or supported bot commands.
3. Run `kleinanzeigen_verify` before any account-changing operation when
   practical.
4. For publish, update, delete, download, or extend, summarize the exact
   non-secret action and wait for explicit user confirmation.
5. If the Kleinanzeigen plugin tools are unavailable, stop and tell the user the
   `kleinclaw` OpenClaw plugin is required.
6. Call the matching optional plugin tool only after confirmation:
   `kleinanzeigen_publish`, `kleinanzeigen_update`, `kleinanzeigen_delete`,
   `kleinanzeigen_download`, or `kleinanzeigen_extend`.
7. Report only the sanitized result returned by the tool. If the result has
   `needsUserAction`, tell the user to run the local bot directly in a
   terminal/browser, finish the account step, then retry `kleinanzeigen_status`
   or `kleinanzeigen_verify`. If the result says configuration or credentials
   need attention, ask the user to fix that outside the chat and rerun
   verification.

## Listing Discovery and Scoping

- When `adRoots` are configured and the user asks for a specific listing, run
  `kleinanzeigen_list_ads` before publishing, updating, deleting, downloading,
  or extending. Use it to resolve the exact local ad directory, config path,
  title, ID, and active state.
- Prefer scoped operations with `adDirectories` or `adConfigPaths` after
  discovery. Avoid broad selectors like `all` unless the user explicitly wants a
  broad operation.
- For a single listing request such as "publish Boxen", first list with a query,
  then verify or publish only the matching directory/config path.
- `kleinanzeigen_delete` still requires explicit `adIds`, even when the
  operation is also scoped to a directory or config path.

## Browser Behaviour

- Use `kleinanzeigen_browser_status` when diagnosing login, browser launch,
  profile, private/incognito mode, or bot/browser mismatch issues.
- Report only sanitized effective browser settings, such as browser, binary
  location, private-window mode, user data directory, and profile name.
- If publishing fails during login-state detection or browser interaction, check
  whether `usePrivateWindow` is enabled. Private/incognito windows may interfere
  with expected login or profile persistence.
- Use `kleinanzeigen_browser_check` to test proposed browser settings without
  changing the real bot config.
- Only change browser configuration with `kleinanzeigen_browser_configure` after
  explicit user confirmation.

## Publish Result Caveats

- A browser-driven publish may complete successfully on Kleinanzeigen even if
  the plugin receives no clean final success feedback from `kleinanzeigen-bot`.
  If the tool result is empty, times out, reports a noisy process exit, or stops
  after browser interaction, do not assume failure automatically.
- Check the structured `outcome` first. Success evidence can include `DONE:`
  counts, published or updated IDs, or selected ad config changes observed after
  the bot exits.
- If the result is unclear, tell the user what happened and suggest checking the
  listing on Kleinanzeigen or rerunning a scoped list, verify, or download when
  appropriate.
- Treat `Approval timed out` as an OpenClaw/tool approval timing issue, not as a
  Kleinanzeigen bot failure. Retry only after the user confirms again.

## Tool Selection

- `kleinanzeigen_status`: check bot availability, config wiring, workspace mode,
  version, and supported commands without reading the config contents.
- `kleinanzeigen_list_ads`: discover configured local ad folders and resolve
  titles, IDs, active state, config paths, and directories before scoped
  operations.
- `kleinanzeigen_browser_status`: inspect sanitized effective browser settings
  and detected local browsers.
- `kleinanzeigen_browser_check`: test current or proposed browser settings
  through bot diagnostics without changing the real config.
- `kleinanzeigen_browser_configure`: change local bot browser settings only
  after explicit confirmation.
- `kleinanzeigen_verify`: validate the already configured local bot setup.
- `kleinanzeigen_publish`: publish or republish due, new, changed, all, combined
  selectors, or explicitly selected ads.
- `kleinanzeigen_update`: update changed, all, or explicitly selected ads.
- `kleinanzeigen_delete`: delete only explicitly selected ad IDs.
- `kleinanzeigen_download`: download new, all, or explicitly selected ads.
- `kleinanzeigen_extend`: extend all eligible ads or explicitly selected IDs.

Do not mix explicit ad IDs with selectors. For publish combinations, use
`selectors`, for example `["changed", "due"]`.

If a tool is unavailable, tell the user that KleinClaw must be installed and
enabled in OpenClaw before that action can run.
