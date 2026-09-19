# Settings & Limitations

## Settings reference

Open via the GridShell sidebar → **Settings**. URL Guard applies immediately, to every call - it's read fresh from settings on each one, not cached. Everything else applies the next time you open a shell.

| Setting | Default | Notes |
|---|---|---|
| Server address | *(blank)* | `ws://` or `wss://` + `host:port`. |
| Auth token | *(blank)* | Required by default - `gridshell-server` copies one to your clipboard on startup (auto-generated, or your own `--auth-token`) unless it was started with `--no-auth-token`. No clipboard available? It's saved to `~/.gridshell/token` regardless - read it from there, or run `gridshell-server --copy-token` to retrieve it again later once a clipboard is available. |
| Working directory | server default | Folder new shells start in, on the machine running the server. Leave blank to use the server's own default. |
| Terminal theme | Dark | Dark or Light. |
| Font size | 13 | 8–24 pt. |
| Terminal width | 900 px | 300–2000 px. |
| Terminal height | 600 px | 200–1200 px. |
| URL Guard | On | See below. |
| Show shell title info | On | Turn off if you hit Apps Script quota errors - this stops a periodic poll used to keep shell names current. |
| Screen reader mode | Off | Mirrors terminal output into a screen-reader-accessible region. Off by default because the mirroring has a real per-render cost, and GridShell's typical use (an AI agent producing frequent output) makes that cost real. See the accessibility notes below before turning it on. |

## Not yet available (planned, not restricted)

These aren't tiering decisions - they simply aren't built yet. See the [Roadmap](../roadmap.md) for where they stand.

- **Native Sheets custom functions.** Sheets' `=MYFUNCTION(A1)` mechanism requires writing new source code into the spreadsheet's own bound script project - GridShell's agent doesn't do that today. An equivalent capability is available a different way: the agent can compute whatever logic it needs in its own shell or via MCP tools, and write the result directly into cells. The difference is *how* it's invoked (on demand, agent-driven) rather than what's possible.
- **Multi-sheet / cross-workbook sessions.** Each GridShell session is bound to the single spreadsheet it was opened from - there's no way for the agent to read or write a *different* spreadsheet within that session today.

## Pro-tier-reserved restrictions

Unlike the gaps above, these are deliberate. The governing principle: **structure is always open, polish is gated.** GridShell's value is the live, iterative loop of building a sheet with an AI agent in the room - so anything inside that loop (creating and refining pivots, filters, data validation, named ranges, formulas, charts, conditional-formatting logic, and so on) stays free. Only cosmetic refinement layered on top of an already-built structure is reserved for a future Pro tier:

- **Chart cosmetic styling** - colors, opacity, fonts, stroke, fill, theme. Structural chart configuration (title, axes, stacking) stays open.
- **Conditional-formatting color scales / heatmap gradients.** The rule *logic* itself (the conditions) stays fully open, only the gradient-color styling is reserved.
- **Spreadsheet theme and branding.**
- **Protection and sharing** - protecting ranges/sheets, and adding or removing editors/viewers.

Two more limits are marketed the same way - Pro-tier-reserved, not just technical ceilings:

- **Shell cap: 2 concurrent shells per document.**
- **Batch execution budget: ~4.5 minutes per `runBatch` call**, fixed regardless of Google account type. Enforced between operations, not within one - a single very large operation can still run long enough to hit Apps Script's own 6-minute hard cap.

A separate, plain technical ceiling applies regardless of tier: **at most 500 operations per `runBatch` call.** Split a larger batch into multiple calls of up to 500 operations each.

If GridShell rejects a call for one of these reasons, it tells you which one and why - this isn't a silent failure.

## URL Guard

On by default. Blocks any formula call containing `IMPORTDATA`, `IMPORTXML`, `IMPORTHTML`, `IMPORTFEED`, `IMAGE`, `IMPORTRANGE`, or `HYPERLINK` - every one of them takes an arbitrary URL as an argument, which is exactly what URL Guard exists to limit: what an agent-written formula can silently fetch from or link to without you seeing it. (A few other built-in functions, like `GOOGLEFINANCE`, also reach outside the spreadsheet - these aren't blocked, since none of them take an arbitrary URL the way the functions above do.) This check applies to the formula text an agent writes directly *and* to a value it reads from the sheet and then writes elsewhere - copying a range whose formulas contain a guarded function is blocked the same way writing that formula directly would be. This is a **security** boundary, unrelated to the Pro-tier list above - it isn't something a paid tier removes.

URL Guard also blocks the structured-API equivalents of these same vectors - `insertImage()` and `setSourceUrl()` (an image fetched from an external URL) and `setLinkUrl()` (a hyperlink) - since these reach outside the spreadsheet the same way `IMAGE()`/`HYPERLINK()` do, without ever producing formula text for the check above to see.

Turning it off requires a confirmation every time:

> **Turn off URL Guard?**
> This allows formulas that can fetch external data or pull data from other spreadsheets (IMPORTDATA, IMPORTXML, IMPORTHTML, IMPORTFEED, IMAGE, IMPORTRANGE) or that create hyperlinks - these can be used to leak your data or mislead you. Only turn this off if you trust the content and formulas you're working with.

## Shell idle timeout

A shell's process survives closing its dialog - that's what makes it safe to close and declutter while an agent keeps working (see [Quick Start](quick-start.md)), but not indefinitely. The clock starts the moment its dialog **disconnects**, for any reason (closing it, your machine sleeping, a network drop, a crashed tab), and the server kills the shell once that reaches 12 hours by default (the person running your server can change this, see [Server: flags](../library/server.md)). Reopening the dialog within that window reattaches you to the still-running shell with its history intact; past it, reopening just starts a fresh one.

**Being listed in the sidebar does not, by itself, keep a shell alive.** The sidebar only tracks that a shell exists and keeps its own sheet-access connection open, independent of the dialog-side idle clock. For a long-running task to survive an extended gap (an overnight run, a laptop that'll sleep), the shell's dialog needs to reconnect within the idle window - being listed isn't enough on its own.

## Accessibility

- **Screen reader mode's Shift+Tab tradeoff.** With screen reader mode on, Shift+Tab always moves keyboard focus out of the terminal, rather than reaching the shell's own tab-completion. This is a deliberate accessibility tradeoff in the underlying terminal component, not a bug.
- **Alt-screen programs can leave your screen reader's view stale.** Switching to a full-screen program inside the shell (`vim`, `nano`, `claude`, etc.) while screen reader mode is on can leave your screen reader's read-out of the terminal looking stale or missing recent history, even though the terminal's actual visible content is unaffected. This is a known limitation of the underlying terminal component's accessibility handling of full-screen mode switches, not something GridShell can currently fix.
