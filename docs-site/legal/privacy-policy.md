# Privacy Policy

## What GridShell's app can access

The GridShell Sheets add-on requests two scopes:

- `spreadsheets.currentonly` - access to the spreadsheet you have open when you run GridShell, and only that spreadsheet. GridShell cannot see or touch any other file in your Google account.
- `script.container.ui` - permission to show the sidebar and dialog GridShell's interface is built from.

## What GridShell does with that access

The add-on relays commands between the spreadsheet and a terminal server, which you, the user, run yourself (see [Introduction](../index.md) for the two-component architecture). GridShell (the developer) does not operate a shared backend, does not see your spreadsheet's contents pass through any server it controls, and does not persist your data anywhere beyond the live session between your browser and your own self-hosted server.

## Data retention

- Settings (server address, theme, font size, etc.) are stored per-user, in your own Google account's Apps Script user properties - not on any GridShell-operated server, because there isn't one.
- The list of open shells for a document is stored per-document, in that document's Apps Script properties.
- Terminal output is buffered in memory on your own self-hosted server for reattach purposes, and is not persisted beyond that server's own process lifetime (subject to whatever you, as its operator, choose to log).

## Contact

gridshell.app@gmail.com
