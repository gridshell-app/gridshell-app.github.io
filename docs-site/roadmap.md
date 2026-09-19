# Roadmap

This page lists what's planned, not what's promised - there are no committed timelines here, and anything on this list can change or be reprioritized.

## Planned

Two items below are firmly reserved for a future paid tier already: cosmetic chart styling, conditional-formatting color scales, spreadsheet theming, and protection/sharing controls (see [App Settings & Limitations](app/settings-and-limitations.md) for the current list), and a higher (or removable) shell cap. Everything else on this list simply isn't built yet - whether any of it ends up free or reserved for a future paid tier hasn't been decided:

- **A configurable, Workspace-aware batch execution time budget.**
- **Configurable session resource limits** (idle-kill timing, output buffer size) - already configurable server-side today via flags; the roadmap item is exposing this per-session from the Sheets side.
- **Eval mode** - an optional, disclosed-risk mode for executing more general logic beyond structured `runBatch` calls.
- **Native Sheets custom functions** (`=FUNCTION()` callable from a cell).
- **Cross-workbook / multi-sheet sessions** - letting an agent work across more than one open spreadsheet from a single session.
- **Connected Sheets support** (BigQuery/Looker-backed data sources).
- **Excel support** - the transport and client library are already host-agnostic by design (`SheetsClient` shares its wire protocol with a future `ExcelClient`).

Have a feature you'd like to see prioritized? [Open an issue on GitHub](support.md).