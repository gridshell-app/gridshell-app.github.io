# Introduction

!!! note
    The GridShell Sheets add-on is currently pending review for the Google Workspace Marketplace and isn't installable yet. The rest of this documentation, and the open-source library it depends on, are already available in the meantime - this note comes down once the add-on is approved.

## What GridShell is
GridShell gives you a real terminal, live-bound to an open Google Sheets spreadsheet. You can manipulate the sheet through structured calls, either using an [AI agent](library/mcp.md) or running [Python scripts](library/python-client.md) directly, and in contrast to "external" REST API or MCP integrations, you can also point the calls at whatever is currently selected. This way you can build a spreadsheet iteratively - try something, look at the result, and adjust - rather than relying on a one-shot script and hoping it's right.

Any MCP-compatible CLI agent can drive it, including a locally-hosted one, not just a single vendor's own integration, unlike most other AI-in-Sheets add-ins: switching models here is cheap, and nothing locks you into one. At the same time, you have all the flexibility of an API/MCP integration, including [building your own tools](library/mcp.md#customizing) on top of the ones [GridShell already provides](library/mcp.md#tools).

## How the pieces fit together

Using GridShell requires two components running together:

- **The GridShell app** (installed from the Google Workspace Marketplace) provides the in-Sheets terminal UI, session management, and settings.
- **A self-hosted server** (the GridShell Python library, installed via PyPI) provides the actual shell and the MCP relay the agent connects to.

The app installs in a few clicks, while the server is something you run yourself, on infrastructure you choose - one per user, never a shared backend GridShell operates on your behalf. It's also open source (MIT), so you can read exactly what it does, adapt it to your own security requirements, or build your own tools on top of it, rather than taking a privacy claim on faith. The app itself, distributed only through the Google Workspace Marketplace, is closed source at this point. See [License](legal/license.md) for the full terms of both, [GridShell App](app/quick-start.md) for installing and using the add-on, and [GridShell Library](library/installation.md) for installing and running the server.

## Security

A GridShell shell is a real one: the agent can run arbitrary code, not just call a fixed set of spreadsheet functions. Don't point it at a spreadsheet, document, or agent/MCP tool you don't otherwise trust - spreadsheet content is something the agent reads and can act on, so treat untrusted content the same way you'd treat any other untrusted input to a coding agent.

Running your own server also adds a small network-facing service on top of that; it requires an auth token by default, even for local use. Deployment specifics, hardening a remote server, and the app's own guardrails (URL Guard, the free-tier deny-list) are covered in [Server: Security & Deployment](library/server.md#security-deployment) and [App Settings & Limitations](app/settings-and-limitations.md).

GridShell is provided as-is, with no warranty of any kind - see the [Terms of Service](legal/terms-of-service.md) for the full terms.

## Where to go next

- [GridShell App](app/quick-start.md) - install from the Marketplace, connect to a server, run your first shell.
- [GridShell Library](library/installation.md) - install the `gridshell` Python package, run a server, register the MCP relay.
- [Troubleshooting / FAQ](troubleshooting.md)
- [Roadmap](roadmap.md) - what's planned, and what isn't available yet.
- [License](legal/license.md) - MIT for the server/library, closed source for the app.
- [GitHub](https://github.com/gridshell-app/gridshell) · [Contact / Support](support.md)
