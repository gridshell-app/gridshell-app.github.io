# Troubleshooting / FAQ

**The sidebar says it can't connect, or hangs for a couple of seconds before connecting.**
On Windows/Chrome, `ws://localhost` connections can take a couple of seconds due to a browser IPv6-then-IPv4 fallback quirk - this is a known browser behavior, not a GridShell bug. If it never connects at all, confirm the server is actually running (`gridshell-server` should print a `Terminal server (Python) running at ...` line) and that the port in your Server address setting matches.

**The app refuses a `ws://` address for a remote server, with an error about a blocked connection.**
Browsers block a plain `ws://` connection from an `https://`-loaded page (Google Sheets is always loaded over HTTPS), except to `localhost`/`127.0.0.1`/`[::1]` - this is a browser-level "mixed content" restriction, not something GridShell can override. The app checks for this itself before even attempting to connect, so it surfaces as a clear error rather than a silent hang. Use `wss://` with a valid certificate on the server (either the server terminates TLS itself with `--wss --cert-path --key-path`, or sits behind a reverse proxy that does).

**`pip install gridshell` or `gridshell-mcp` fails, or import errors mention `mcp`.**
Check your Python version - `python3 --version`. The `mcp` SDK dependency has never supported Python below 3.10; this is especially common to hit on macOS, where the system Python is often older.

**"Address already in use" / the server won't start on the port I expected.**
Something else is already listening on that port. Pick a different one with `--port`.

**The MCP tool call fails with "Multiple clients connected and no session was specified."**
Registered from inside a document's own embedded terminal, `gridshell-mcp` always targets the right document automatically. If you run `gridshell-mcp` standalone (e.g. from a global MCP config, not launched from inside a GridShell shell) with more than one document's shell connected to the same server, it can't guess which one you mean. See [MCP](library/mcp.md) for what to set by hand in that case (session, token, and key are all required together, not just the session id).

**A shell says "Previous shell was no longer running - started a new one."**
The server's idle-kill timer (12 hours by default) closed the old session before you reattached - this is expected behavior, not an error. A fresh shell was started in its place.

**The dialog says "Rejected by the server: invalid or missing token."**
The server is running and reachable, but the token in Settings' **Auth token** field doesn't match what the server is actually using, or is missing entirely - a `gridshell-server` you started with no flags at all still requires a token by default (auto-generated and persisted at `~/.gridshell/token`, copied to your clipboard if one is available; run `gridshell-server --copy-token` to copy the current one again). Check it carefully - it's compared exactly, including case.

**`gridshell-server` prints a message about no clipboard being available.**
The auto-generated token is delivered via the clipboard when one is available, but it's always written to `~/.gridshell/token` first regardless - on a system with no clipboard (a headless box, an SSH session), the server starts normally and prints that file path instead, so read it from there. Only an actual failure to persist the token refuses to start; in that case, pass `--auth-token`/`AUTH_TOKEN` with a value you provide, or retry.

**A shell row shows a small orange dot next to its name.**
That shell's `/terminal` connection (what the dialog itself uses) is fine, but its separate `/host` bridge - the channel every MCP tool call actually travels over - isn't. Hover the dot for the specific reason. Most causes (a transient disconnect, or the shell not being running on the server yet) retry on their own - `getValues`/`setValues`/etc. from an agent will hang until the relay's timeout until it reconnects. A bridge rejected for a bad token or `hostKey` is the exception: it stops retrying by design, to avoid spamming the server's auth-rejection log, and only reconnects after you change the token or server address in Settings.
