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
- [ ] Full end-to-end workflow tested in a clean Claude Code installation
- [ ] License selected
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

## 2. Keep shared instructions synchronized

The installable plugin must remain self-contained. Development integrations that reuse the same `SKILL.md` and JSON reference should compare their copies automatically and fail CI when they diverge.

Do not use symlinks that resolve outside the plugin directory because installed plugins are copied into Claude's plugin cache.

## 3. Select a license

Choose a license before directory submission, add a `LICENSE` file, and add the SPDX identifier to the plugin manifest and marketplace entry.

## 4. Submit for discovery

After the acceptance test and license decision:

1. Submit the plugin to the Claude community plugin directory.
2. Submit `https://sessionplan.de/mcp/` separately to the Claude Connectors Directory.
3. Add directory installation links to the Sessionplan website after approval.

## Release checklist

- [x] Strict Claude plugin validation successful
- [ ] All end-to-end workflows successful
- [x] Synchronization check active in the development repository
- [x] README complete
- [ ] License selected
- [x] Public marketplace installation path available
- [ ] Installation tested on a clean system
- [ ] Version and changelog updated for the next release

## References

- [Claude Code: create plugins](https://code.claude.com/docs/en/plugins)
- [Claude Code: plugin marketplaces](https://code.claude.com/docs/en/plugin-marketplaces)
- [Claude Code: plugin reference](https://code.claude.com/docs/en/plugins-reference)
- [Claude connectors directory](https://claude.com/docs/connectors/directory)
