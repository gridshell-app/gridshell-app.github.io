# About

GridShell is an interactive terminal add-on for Google Sheets™ that connects any MCP-compatible AI CLI agent, or a Python script, directly to a live spreadsheet. It is developed and maintained independently.

GridShell started as a way to solve a narrower problem: building reliable AI tooling for financial modeling. That work needs two things at once - interactive, iterative back-and-forth between a user, an agent, and a live spreadsheet, not a one-shot script run against it, and enough flexibility in the agent's own harness (its instructions, skills, tools), not just a fixed set of API calls. A real terminal bound to the sheet, running a full CLI agent, is what supports both.

GridShell's scope isn't limited to financial modeling, though - the broader goal is supporting experimentation and tool development around advanced spreadsheet work generally.

The server/library is open source (MIT) and publicly readable on [GitHub](https://github.com/gridshell-app/gridshell); the add-on itself is closed source.

**Data handling**: GridShell has no backend of its own - your spreadsheet data only ever goes to the server you run yourself, never through any server the developer operates. See the [Privacy Policy](legal/privacy-policy.md) for the full breakdown.

**Learn more**: [GitHub](https://github.com/gridshell-app/gridshell) · [Support / Contact](support.md) · [License](legal/license.md) · [Privacy Policy](legal/privacy-policy.md) · [Terms of Service](legal/terms-of-service.md)
