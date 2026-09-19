# MCP

`gridshell-mcp` is a stdio MCP server - it relays tool calls from any MCP-compatible AI agent to a running `gridshell-server`'s `/mcp` endpoint.

## Registering it

To add the `gridshell-mcp` server to your MCP client, follow the instructions for your client. Some register a stdio command directly - for example, for Claude Code:

```bash
claude mcp add gridshell-sheets -- gridshell-mcp --port 3010
```

Others only support registering through a config file - for example, for OpenCode's `opencode.json`:

```json
{
  "mcp": {
    "gridshell-sheets": {
      "type": "local",
      "command": ["gridshell-mcp", "--port", "3010"]
    }
  }
}
```

Either way, point it at `gridshell-mcp` with whatever flags match the server it should talk to.

## Flags

| Flag | Env var | Default | Meaning |
|---|---|---|---|
| `--port` | `PORT` | `3000` | Must match the `gridshell-server` you're relaying to. |
| `--wss` | - | off | Connect to the server over `wss://` instead of `ws://`. |
| `--auth-token` | `AUTH_TOKEN` | - | The target server's token - required whenever one is set, which is the default (`gridshell-server` auto-generates one unless started with `--no-auth-token`). Not needed when `gridshell-mcp` is launched from inside a document's own embedded shell, as it picks the token up automatically from `GRIDSHELL_AUTH_TOKEN`, the same way it picks up `GRIDSHELL_SESSION`. |

`gridshell-mcp` also reads `GRIDSHELL_SESSION` from its environment automatically when launched from inside a document's own embedded terminal (the same mechanism CLI agent itself uses), so it always targets that exact document, with no ambiguity, even when other documents are connected too. Running it standalone (e.g. from a global MCP config, not from inside a GridShell shell) falls back to whichever single document is currently connected. If more than one is connected, it returns an error asking you to disambiguate rather than guessing.

Naming a session isn't enough on its own. The server also requires `GRIDSHELL_SESSION_KEY`, a secret it mints for that specific shell and sets alongside `GRIDSHELL_SESSION`, so a connection has to prove it actually owns the session it's naming, not just guess or copy the id. `gridshell-mcp` reads this automatically from the same environment as `GRIDSHELL_SESSION`, so there's nothing to configure when it's launched from inside the shell it's targeting. This check doesn't apply to the no-session "sole connected client" fallback above, since that path never names a session at all.

**`gridshell-mcp` always connects to `localhost` - there's no flag to point it at a remote `gridshell-server`.** It must run on the same machine as the server it's relaying to. This is the normal case (an agent running inside a document's embedded terminal is already co-located with the server), but it also means a `gridshell-mcp` registered in a global MCP config on a *different* machine than your `gridshell-server`, e.g. pointing your laptop's AI agent at a remote/`wss` server, will not work today.

## Tools

| Tool | Use for |
|---|---|
| `getValues` | Read a range. Lighter-weight than `runBatch` for a plain read. |
| `setValues` | Write a 2D array of values into a range. |
| `appendRow` | Append one row to the end of a sheet's data, no separate lookup for "where does the data end" needed. |
| `runBatch` | Everything else: formulas, formatting, charts, conditional formatting, pivot tables, multiple operations in one call. |

**Prefer `runBatch`/the dedicated tools over repeated individual calls for bulk operations.** This isn't just about atomicity - each individual, non-batched call pays its own full round-trip to Sheets. Fifty one-by-one writes can take minutes; the same data as one `runBatch` call stays fast.

### `runBatch`

Runs one or more chains of method calls against the live spreadsheet, submitted as data - there's no code execution, no arbitrary JavaScript. Each op is a `chain`: a sequence resolved starting from the spreadsheet itself.

```json
{"ops": [{"chain": [
  {"method": "getActiveSheet", "args": []},
  {"method": "getRange", "args": ["D14"]},
  {"method": "setFormula", "args": ["=D14*(1+Assumptions!$B$5)"]}
]}]}
```

- **Building a value as an argument** (a conditional-format rule, a chart), rather than as the chain's own progressing target - use a `{"__chain": [...]}` marker in place of the argument. It's resolved as its own chain first, and the result is passed in.
- **Acting on a specific existing object** (a chart, pivot table, image already on the sheet) - a chain step can be `{"index": N}` (indexing into the array the previous step returned) instead of a method call. Charts specifically also support `{"chartId": "..."}`, which is stable across calls even if other charts are added, removed, or reordered.
- **If a batch is too large, hits a restriction, or a step fails partway through**, the response reports how far it got (`nextIndex`) - retry only what's left, not the whole batch. A chain's steps are real writes as they happen, so a later step failing doesn't undo earlier ones in the same chain; check `sideEffectsApplied`/`stepIndex` in the error rather than assuming nothing happened.

The full JSON Schema for `runBatch`'s input is generated directly from the tool's own definition and will be published as a standalone reference - until then, the shape above (plus `gridshell-mcp`'s own tool description, visible to any MCP client) is authoritative.

## Customizing

If you want to add your own tool, **don't edit the installed copy of `mcp_grid.py` in place** - a future `pip install --upgrade gridshell` will overwrite it and your tool disappears. Copy the file out to your own location, edit that copy, and point your MCP config entry at the copy instead. For example for Claude Code:

```bash
claude mcp add gridshell-sheets -- python /path/to/your/mcp_grid.py --port 3010
```

### Example: a custom color-coding tool for financial modeling

A worked example is planned but not published yet. The design, for reference if you want to build something similar in the meantime:

A naive per-cell "check the value, then color it" loop would hit the same round-trip cliff described above. Instead:

1. One `runBatch` call reads the whole target range's values *and* formulas in a single round trip (`getValues()` + `getFormulas()`) - enough to classify every cell (formula vs. input, cross-sheet reference or not) without a second read.
2. Group classified cells into contiguous A1 ranges per color, rather than one write per cell.
3. One write call per color group, using `getRangeList([...]).setBackground(color)` - which applies to many scattered ranges in a single call.

Net effect: a fixed, small number of Sheets round-trips (one read, a handful of writes) regardless of how large the range is - the same principle as preferring `runBatch` over individual calls above.
