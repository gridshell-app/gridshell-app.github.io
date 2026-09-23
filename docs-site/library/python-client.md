# Python Client

`SheetsClient` speaks the same `/mcp` WebSocket protocol `gridshell-mcp` uses, directly from a script or REPL - no MCP SDK, no stdio, no LLM involved.

## Quickstart

```python
from gridshell import SheetsClient

grid = SheetsClient(port=3012)          # or host=, wss=True, session=<id>
grid.get_values("A1:C10")
grid.set_values([[1, 2], [3, 4]], range="A1:B2")
grid.set_values([1, 2, 3], range="A1:C1")      # a flat list is also accepted, as one row
grid.append_row(["Widget", 12, "2026-09-06"])
grid.close()
```

Or as a context manager:

```python
with SheetsClient(port=3012) as grid:
    rows = grid.get_values("A1:C10")
```

## Working with the current selection

Omit `range` to read or write whatever's currently selected in the sheet:

```python
grid.get_values()                 # current selection's values
grid.set_values([[1, 2, 3]])       # overwrite the current selection
```

`sheet=` isn't valid together with an omitted `range` - the selection is always whatever's active in the UI, on whichever sheet that is. Pass an explicit `range` (and optionally `sheet`) to target a specific sheet.

## Connecting to a server that requires a token

A token is required by default (see [Server](server.md)). Pass `token=` explicitly, or leave it unset and set `GRIDSHELL_AUTH_TOKEN` in the environment - already set automatically inside a document's own embedded shell, so a script launched from there needs nothing extra. A wrong or missing token surfaces as `GridShellConnectionError: Rejected by the server: invalid or missing token`. Check the value against `--copy-token`'s clipboard copy or `~/.gridshell/token`.

## Which spreadsheet does this connect to?

Each open spreadsheet runs its own, independent GridShell session - `/mcp` always relays to one specific document, never a mix of several.

- **Only one document currently connected, no `session` given**: the server falls back to it automatically.
- **More than one connected, no `session` given**: the server refuses the call (`GridShellError: Multiple clients connected and no session was specified`) rather than guessing.
- **Running from inside a document's own embedded terminal**: the library reads `GRIDSHELL_SESSION` from the environment automatically, the same way `gridshell-mcp`/Claude Code does, and always targets that document.
- **Running externally, targeting a specific document among several**: pass `session=<id>` explicitly; it always wins over the environment variable. Get the id by running `echo $env:GRIDSHELL_SESSION` (PowerShell) inside that document's own embedded shell. An explicit `session` also needs its matching `session_key` (the server rejects a session id on its own as unproven) - pass `session_key=<key>`, or leave it unset and set `GRIDSHELL_SESSION_KEY` in the environment instead, same `echo $env:GRIDSHELL_SESSION_KEY` pattern. Both env vars are already set together inside that document's own embedded shell, so a script launched from there needs neither passed explicitly.

## Beyond the built-in methods

`run_batch()` is the generic structured-chain escape hatch - the same grammar described in [MCP](mcp.md#runbatch):

```python
grid.run_batch([
    {"chain": [
        {"method": "getActiveSheet", "args": []},
        {"method": "getRange", "args": ["D14"]},
        {"method": "setFormula", "args": ["=D14*(1+Assumptions!$B$5)"]},
    ]},
])
```

`call(tool, params)` invokes any tool by name, including ones added to the server's tool list after this library shipped:

```python
grid.call("someNewTool", {"foo": "bar"})
```

## Reference

### `SheetsClient(...)`

| Parameter | Default | Notes |
|---|---|---|
| `host` | `"localhost"` | Server address. |
| `port` | `3000` | Must match the `gridshell-server` you're targeting. |
| `wss` | `False` | Connect over `wss://` instead of `ws://`. |
| `session` | `None` | Falls back to `$GRIDSHELL_SESSION`. Explicit value always wins. See [Which spreadsheet does this connect to?](#which-spreadsheet-does-this-connect-to) above. |
| `session_key` | `None` | Falls back to `$GRIDSHELL_SESSION_KEY`. Required alongside an explicit `session`. |
| `token` | `None` | Falls back to `$GRIDSHELL_AUTH_TOKEN`. Required whenever the target server was started with a token (the default). |
| `default_timeout` | `300` (seconds) | Narrowed automatically after connecting, to match the server's real `runBatch` time budget plus margin. |

### `get_values(range=None, sheet=None, timeout=None)`

Read a range's values, as a 2D list. `range=None` reads the current selection instead - `sheet` isn't valid together with `range=None` (see [Working with the current selection](#working-with-the-current-selection) above). `timeout` overrides `default_timeout` for this call only.

### `set_values(values, range=None, sheet=None, timeout=None)`

Write `values` (a 2D list of rows, or a flat list treated as one row) into `range`, or into the current selection if `range=None` (same `sheet` restriction as `get_values`).

### `append_row(values, sheet=None, timeout=None)`

Append one row (a flat list) after the end of `sheet`'s existing data, or the active sheet if `sheet=None`.

### `run_batch(ops, timeout=None)`

Run a list of `{"chain": [...]}` structured-op entries. See [Beyond the built-in methods](#beyond-the-built-in-methods) above and [MCP](mcp.md#runbatch) for the chain grammar.

### `call(tool, params=None, timeout=None)`

Raw escape hatch - invoke any tool by name with a params dict, including tools added to the server after this library shipped. Connects lazily on first use if not already connected.

### `close()`

Closes the connection. Also available as a context manager (`with SheetsClient(...) as grid:`), which connects on entry and closes on exit.

## Errors

| Exception | When |
|---|---|
| `GridShellConnectionError` | Couldn't connect, or the connection dropped mid-call. |
| `GridShellTimeoutError` | No response within the call's timeout. |
| `GridShellError` | The server (or the spreadsheet it relayed to) returned an error, e.g. a deny-list block, a failed `runBatch` step, etc. The message is passed through unchanged. |

All three subclass `GridShellException`.

## Scope

`SheetsClient` is deliberately narrow: one raw escape hatch (`call`) plus a handful of convenience methods, no plugin/registration system. Compose your own functions on top of these primitives for anything more specific.

Only Sheets is implemented today. `SheetsClient` is named explicitly (not a bare `Client`) so an Excel client can be added later on the same transport, without a breaking rename of this one.
