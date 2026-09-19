# Quick Start

Before you start: GridShell's Sheets add-on is a client - it needs a running GridShell server to connect to. If you don't have one yet, see [Installing the Library](../library/installation.md) and [Running the server](../library/server.md) first; anyone on your team can run the server, it doesn't need to be you.

## Install the add-on

1. Open a Google Sheet.
2. Extensions → Add-ons → Get add-ons, search for **GridShell**, install it.
3. Reload the spreadsheet. A **GridShell** menu appears (under Extensions, or as its own top-level menu depending on your Workspace settings).

The first time you open GridShell, you'll see a one-time welcome dialog. It won't reappear.

## Connect to a server

1. GridShell menu → **Open** - opens the sidebar.
2. Expand **Settings**.
3. Under **Server address**, choose `ws://` (local server, no certificate) or `wss://` (remote server, requires a valid TLS certificate on the server side), and enter `host:port` - e.g. `localhost:3000` or `yourdomain.com:3000`.
4. A token is required by default - `gridshell-server` copies it to your clipboard on startup (run `gridshell-server --copy-token` to get it again later), so paste it into the **Auth token** field below it. You only need to do this once per Google account; it carries over into every other spreadsheet you use GridShell in.
5. Leave the other settings at their defaults for now (see [Settings & Limitations](settings-and-limitations.md) for what each one does) and click **Save**.

Settings take effect the next time you open a shell, not retroactively on one already open.

## Open, reattach, close, and kill a shell

- **Open a new shell**: click **+ New shell** in the sidebar. GridShell is capped at 2 concurrent shells per document in the free tier and the button disables once you hit the cap.
- **Reattach to a running shell**: click its row in the **Opened shells** list. This reopens the same session and whatever was running keeps running, nothing restarts.
- **Close the dialog without stopping the shell**: just close the dialog window. The shell keeps running on the server (subject to the server's idle-kill timer, 12 hours by default) - reopen it later from the same row.
- **Kill a shell**: hover its row and click the bin icon. This terminates the shell process immediately and removes it from the list - not reversible.
- **Close everything at once**: GridShell menu → **Close all sessions**.
- **Rows can be dragged to reorder**: a shell's row label updates automatically from the shell's own reported window title (e.g. it'll say "claude" once you launch Claude Code inside it), unless you've turned that off in Settings.

## What you can't do (yet)

A few things are worth knowing before you start:

- **Two shells per document, free tier.** Side-by-side sessions on different spreadsheets still work; this cap is on concurrent shells within one document.
- **URL Guard is on by default** and blocks an agent from inserting a handful of formulas that take an arbitrary URL, which can leak data or mislead (`IMPORTDATA`, `IMPORTXML`, `IMPORTHTML`, `IMPORTFEED`, `IMAGE`, `IMPORTRANGE`, `HYPERLINK`). Turn it off in Settings if you trust the document and the agent you're working with - it asks for confirmation every time.
- **One spreadsheet per session.** There's no way for the agent to reach a *different* open spreadsheet from within one shell today.
- **No native Sheets custom functions** (`=FUNCTION()` callable from a cell) - the agent can still compute custom logic and write results into the sheet, just not as a cell formula.
- **A single batch of operations is capped at roughly 4.5 minutes of execution time**, regardless of your Google account type.

Full detail on all of these, including what's deliberately restricted for a future Pro tier vs. what simply isn't built yet, is in [Settings & Limitations](settings-and-limitations.md).
