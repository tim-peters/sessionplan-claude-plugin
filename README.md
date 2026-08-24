# Sessionplan for Claude

Create and revise structured agendas for workshops, trainings, meetings, and events, then open them directly in [Sessionplan](https://sessionplan.de/).

This repository is a public Claude plugin marketplace. The bundled `sessionplan` plugin combines:

- a planning skill for concrete, timed agendas;
- the public Sessionplan MCP server at `https://sessionplan.de/mcp/`;
- tools for creating a new Sessionplan link and opening an existing one.

## Install from the marketplace

In Claude Code, run:

```text
/plugin marketplace add tim-peters/sessionplan-claude-plugin
/plugin install sessionplan@sessionplan
```

If Claude asks you to reload plugins, run:

```text
/reload-plugins
```

Use `/mcp` to verify that the `sessionplan` server is connected.

## Try it locally

From a checkout of this repository:

```bash
claude plugin validate . --strict
claude --plugin-dir ./plugins/sessionplan
```

Example prompts:

- “Create a two-hour workshop agenda for aligning a product team on next-quarter priorities.”
- “Open this Sessionplan link and summarize the agenda.”
- “Move the break ten minutes earlier and give me the revised Sessionplan link.”

## Direct MCP connection

Clients that support remote MCP can connect directly to:

```text
https://sessionplan.de/mcp/
```

The Claude plugin adds planning guidance and automatic workflow selection on top of the MCP tools.

## Privacy

Sessionplan links are designed to be shareable. Do not include personal or sensitive information unless it is necessary and appropriate for the intended audience.

See the current product and privacy information at [sessionplan.de](https://sessionplan.de/).

## Development

The marketplace catalog is stored in `.claude-plugin/marketplace.json`. The installable plugin is self-contained under `plugins/sessionplan/` so Claude can copy it into its plugin cache without depending on files outside the plugin directory.

Before publishing a change:

1. update the plugin version in `plugins/sessionplan/.claude-plugin/plugin.json`;
2. run `claude plugin validate . --strict`;
3. test creating, decoding, and revising a Sessionplan in Claude Code.

## License

AGPL-3.0-or-later. See [LICENSE](LICENSE).

## Support

- Product: https://sessionplan.de/
- Source: https://github.com/tim-peters/sessionplan-claude-plugin
- Issues: https://github.com/tim-peters/sessionplan-claude-plugin/issues
