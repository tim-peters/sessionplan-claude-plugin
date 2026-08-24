# Sessionplan Claude plugin: next steps

Status: 24 August 2026

## Goal

Make the Sessionplan plugin reliable, easy to install, and broadly discoverable across Claude Code and other Claude products that support remote MCP connectors.

## Current status

- [x] Public marketplace repository created
- [x] Marketplace catalog added under `.claude-plugin/marketplace.json`
- [x] Self-contained plugin added under `plugins/sessionplan/`
- [x] Remote Sessionplan MCP server configured
- [x] Plugin validated with the Claude Code CLI
- [x] MCP server connection verified
- [x] `create_session_link` and `decode_session_link` verified as available
- [x] Installation and development documentation added
- [x] Full end-to-end workflow tested against the live MCP server (see results below; a literal clean-install run is still open)
- [x] License selected (AGPL-3.0-or-later; see `LICENSE`, `plugins/sessionplan/.claude-plugin/plugin.json`, `.claude-plugin/marketplace.json`)
- [ ] Plugin submitted to the Claude community directory
- [ ] Remote MCP submitted to the Claude Connectors Directory

## 1. End-to-end acceptance test

Test these workflows in a clean Claude Code environment:

- Create a two-hour workshop agenda from a short description.
- Decode and summarize an existing Sessionplan link.
- Revise an existing plan and return a new link.
- Complete missing low-risk details with clearly stated assumptions.
- Avoid activating the workflow for generic facilitation advice without a concrete agenda.

Acceptance criteria:

- New links come only from `create_session_link`.
- Revisions decode the existing link first.
- Durations, IDs, and references are internally consistent.
- Claude never invents or reconstructs Sessionplan URLs.

### Results (24 August 2026)

All five workflows were exercised against the live `sessionplan` MCP tools (create/decode) in this environment; the "no generic advice" case was also exercised in a fresh subagent with no prior context to check real trigger behavior rather than self-assessment.

- Two-hour workshop from a short description: passed. `create_session_link` returned a link with `totalDuration: 120`, matching the target.
- Decode and summarize an existing link: passed. `decode_session_link` round-tripped the session exactly.
- Revise and return a new link: passed. Decoded first, IDs and structure preserved, only requested fields changed (added a person, rebalanced two durations), total duration still 120, new link differs from the original.
- Missing low-risk details completed with stated assumptions: passed. Built a plan for a vague request (no duration/audience given), assumed a duration and audience size, stated the assumption in the session description, left `date`/`startTime` as `null` rather than inventing values.
- No activation on generic facilitation advice: passed. A fresh subagent asked "What are some good icebreaker techniques for workshops in general?" answered directly and explicitly did not call the Sessionplan skill or tools, citing the "no concrete agenda" rule in `SKILL.md`.

**Bug found and doc fixed:** `create_session_link` rejects `children` on `type: "block"` items (`SESSION_VALIDATION_FAILED: Feld 'items[0].children' ist für block-Items nicht erlaubt`), even though the tool's own JSON-schema description lists `children` as a general `WorkshopItem` property. A model following the schema literally hits this on the first block item it builds. Added an operational rule to `agents/openai-plugin/references/sessionplan-json.md` (source of truth) and synced it to the plugin copy via `npm run agents:sync` / `node scripts/sync-agent-plugins.mjs --write`: "Only include `children` on `group` and `breakout` items. A `block` item must omit `children` entirely; sending it (even as an empty array) fails validation." Still worth tightening the live MCP tool schema itself (conditional/`additionalProperties` per `type`) so this is caught by schema validation rather than a runtime error, but that's a server-side change outside this plugin repo.

Not yet done: a run from an actual clean/fresh Claude Code install (separate config, plugin installed via the marketplace flow from scratch) — everything above ran with the plugin's MCP tools already available in this session.

## 2. Keep shared instructions synchronized

The installable plugin must remain self-contained. Development integrations that reuse the same `SKILL.md` and JSON reference should compare their copies automatically and fail CI when they diverge.

Do not use symlinks that resolve outside the plugin directory because installed plugins are copied into Claude's plugin cache.

## 3. Select a license

Choose a license before directory submission, add a `LICENSE` file, and add the SPDX identifier to the plugin manifest and marketplace entry.

Done (24 August 2026): AGPL-3.0-or-later. Verbatim license text from gnu.org added as `LICENSE`; `license: "AGPL-3.0-or-later"` added to `plugins/sessionplan/.claude-plugin/plugin.json` and to the plugin entry in `.claude-plugin/marketplace.json`; both fields confirmed present in the official `claude-code-plugin-manifest.json` / `claude-code-marketplace.json` schemas. README links to `LICENSE`.

## 4. Submit for discovery

After the acceptance test and license decision:

1. Submit the plugin to the Claude community plugin directory.
2. Submit `https://sessionplan.de/mcp/` separately to the Claude Connectors Directory.
3. Add directory installation links to the Sessionplan website after approval.

### Researched process (24 August 2026)

**Plugin → community marketplace** (`anthropics/claude-plugins-community`, installed by users as `@claude-community`):

- There is no application process for the Anthropic-curated `claude-plugins-official` marketplace — inclusion there is at Anthropic's sole discretion and the submission form does not add plugins to it. Only the community marketplace is actually reachable through a submission form.
- Two submission entry points depending on account type — both route to the same community-marketplace review:
  - `claude.ai/admin-settings/directory/submissions/plugins/new` — requires a Team/Enterprise org with directory management access (Owner by default).
  - `platform.claude.com/plugins/submit` (Console) — for individual authors without a Team/Enterprise org. **This is the path for this account** (Individual Pro on claude.ai, no org set up) — it needs a separate `platform.claude.com` (Anthropic Console) login, not the claude.ai Pro subscription.
- Before submitting: run `claude plugin validate ./plugins/sessionplan --strict` locally (the review pipeline runs the same check, plus automated safety screening).
- After approval: pinned to a commit SHA in `anthropics/claude-plugins-community`; CI bumps the pin automatically on new commits; the public catalog syncs nightly, so check https://github.com/anthropics/claude-plugins-community/blob/main/.claude-plugin/marketplace.json to confirm when it's live.

**Remote MCP → Connectors Directory** (`https://sessionplan.de/mcp/`):

- Submission portal (`claude.ai/admin-settings/directory/submissions/new`) requires a Team or Enterprise organization with directory management access — **no individual-plan path exists** ("Organization settings aren't available on individual plans"). Blocked for this account until a Team/Enterprise org is created; revisit later.
- Requirements to prepare regardless: every tool needs a `title` and `readOnlyHint`/`destructiveHint` annotation; OAuth 2.0 if the server is authenticated (Sessionplan's MCP server is unauthenticated, which the portal also supports); a documentation URL; a privacy-policy URL (candidate: `https://sessionplan.de/datenschutz`, confirmed live); server URL must be `https://` (confirmed: `https://sessionplan.de/mcp/` returns 200) and its transport type (streamable HTTP vs SSE) must be known for the "Connection" step.
- The portal walks through 10 steps: Introduction → Connection → Tools → Listing (name ≤100 chars, tagline ≤55 chars, description ≤2000 chars, categories, docs/privacy URLs, icon, permanent slug) → Use cases → Company → Authentication → Data handling → Test & launch (reviewer-usable test instructions; confirm every tool was run via MCP Inspector or as a custom connector) → Compliance (7 required acknowledgments) → Review.

## Release checklist

- [x] Strict Claude plugin validation successful
- [x] All end-to-end workflows successful (live MCP; see Results above — one schema/doc bug found, see below)
- [x] Synchronization check active in the development repository
- [x] README complete
- [x] License selected
- [x] Public marketplace installation path available
- [ ] Installation tested on a clean system
- [x] Version updated for the next release (`1.0.0` → `1.0.1` in `plugins/sessionplan/.claude-plugin/plugin.json`); no changelog file exists yet in this repo

## References

- [Claude Code: create plugins](https://code.claude.com/docs/en/plugins)
- [Claude Code: plugin marketplaces](https://code.claude.com/docs/en/plugin-marketplaces)
- [Claude Code: plugin reference](https://code.claude.com/docs/en/plugins-reference)
- [Claude connectors directory](https://claude.com/docs/connectors/directory)
