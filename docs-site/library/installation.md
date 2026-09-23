# Installation

The `gridshell` Python package bundles everything you need to self-host a GridShell backend:

- **`gridshell-server`**: the server itself (PTY + Sheets bridge + MCP relay).
- **`gridshell-mcp`**: the MCP relay, for registering with an MCP-compatible AI coding agent.
- **`SheetsClient`**: a Python client for scripts and REPLs.

All three install together - there's no slimmed-down variant, by design.

## Requirements

- **Python 3.10 or later.** This floor is forced by the `mcp` SDK dependency, which has never supported anything older. If you're on macOS, check your system Python version first (`python3 --version`) - it's often older than 3.10.
- **Windows, Linux, and macOS are all supported** - the PTY backend is `pywinpty` (ConPTY) on Windows and `ptyprocess` on Linux/macOS, selected automatically. Testing coverage differs by platform and deployment shape - see [Platform support](server.md#platform-support) for the honest breakdown before picking one for a remote deployment.
- **This package is the *server-side* half only.** It doesn't include the Sheets add-on itself - install that separately from the [Google Workspace Marketplace](../app/quick-start.md).

## Install

```bash
pip install gridshell
```

This installs the library plus the `gridshell-server` and `gridshell-mcp` console commands.

## Verify

```bash
gridshell-server --port 3012
```

You should see:

```
Auth token copied to clipboard.
Terminal server (Python) running at ws://localhost:3012
  Sheets sidebar Server address: localhost:3012  (paste the auth token into the Auth token field - see above for how to retrieve it, or run `gridshell-server --copy-token`)
  /terminal  xterm.js I/O
  /host      Host command bridge
  /mcp       MCP server relay
```

The first line is your only cue that a token exists at all and is now on your clipboard - see [Running the server](server.md) for how auth works by default.

Next: [Running the server](server.md) for the full flag reference and deployment guidance, or [Registering the MCP relay](mcp.md) to connect an AI agent.
