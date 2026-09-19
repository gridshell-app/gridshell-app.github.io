# Terms of Service

## Agreement

By installing the GridShell Sheets add-on, running the `gridshell` server, or otherwise using either component, you agree to these terms. If you don't agree, don't use GridShell.

## The software

GridShell consists of two components: a Google Sheets add-on (installed from the Workspace Marketplace) and a self-hosted server (the `gridshell` Python package, installed separately, run by you). See [Introduction](../index.md) for how they relate.

## Your content and data

GridShell (the developer) does not operate a shared backend and does not see your spreadsheet's contents pass through any server it controls - see the [Privacy Policy](privacy-policy.md) for the full breakdown of what the add-on can access and how it's used. You retain all rights to your own spreadsheet data; nothing in these terms grants the developer any ownership or license over it.

## Acceptable use

You agree not to use GridShell to:

- Access, modify, or exfiltrate data you're not authorized to access, including another person's spreadsheet reached through a shared server or session.
- Violate Google's own terms governing Workspace Marketplace apps, Apps Script, or the Sheets API ([Google Apps Script Additional Terms](https://developers.google.com/apps-script/terms)), or any applicable law.
- Use the shell GridShell gives an agent access to for anything you wouldn't otherwise be authorized to do on the machine running the server, including attacking or gaining unauthorized access to other systems.

## Third-party services

Your use of the Sheets add-on is also governed by Google's own terms for Workspace Marketplace apps and Apps Script. GridShell doesn't control the Google Sheets/Workspace platform and isn't responsible for its availability, changes, or policies.

## Availability and changes

GridShell is self-hosted - the developer makes no uptime or availability guarantee for your own server, since the developer doesn't run it. The add-on and the `gridshell` Python package may be updated, changed, or discontinued at any time; reasonable care is taken not to break existing setups without notice, but this isn't a guarantee.

## Current version

GridShell is currently offered free of charge, with no paid tier available yet. This may change in the future - see the [Roadmap](../roadmap.md) for what's under consideration. Nothing here obligates the developer to keep any specific feature free indefinitely.

## Disclaimers

GridShell is provided as-is, with no warranty of any kind, express or implied - including, without limitation, warranties of merchantability, fitness for a particular purpose, and non-infringement. The developer doesn't promise GridShell (either component) will be error-free, uninterrupted, or fit for any particular use.

## Limitation of liability

GridShell is self-hosted software. You run the server yourself, on infrastructure you control, and you connect it to an AI agent of your choosing. **GridShell's developer assumes no liability for actions taken by an AI agent through GridShell, whether on the server side or the app side** - including, without limitation, actions that modify, delete, or expose data in your spreadsheet, or actions taken by a shell process the server spawned. You are solely responsible for what you grant the agent access to, what you ask it to do, and the outcome.

This is a direct consequence of the architecture, not a disclaimer of convenience: GridShell (the developer) has no visibility into, or control over, what happens on a server you run yourself.

To the maximum extent permitted by law, the developer's total liability for any claim arising out of or relating to GridShell is capped at **USD $0**, reflecting that GridShell is provided entirely free of charge with no payment collected from you. Some jurisdictions don't allow this kind of limitation, in which case it applies only to the extent permitted there.

GridShell does provide a set of guardrails on top of this boundary, see [App Settings & Limitations](../app/settings-and-limitations.md) for URL Guard and the current restriction list, and [Server: Security & Deployment](../library/server.md#security-deployment) for authentication and TLS guidance. These are best-effort, not a guarantee - they do not shift responsibility for the outcome away from you.

## Indemnification

You agree to indemnify and hold the developer harmless from any claim, damage, or expense (including reasonable legal fees) arising from your use of GridShell, your violation of these terms, or content/actions passed through the AI agent you connected to it.

## Termination

You may stop using GridShell at any time by uninstalling the add-on and/or stopping your server - there's no account to close, since GridShell doesn't operate one. The developer may remove the add-on from the Workspace Marketplace, or stop maintaining the `gridshell` package, at any time; your own already-downloaded copy keeps working regardless.

## Governing law

These terms are governed by the laws of England and Wales, without regard to conflict-of-law principles. The courts of England and Wales have exclusive jurisdiction over any dispute arising out of or relating to these terms or GridShell.

## Changes to these terms

These terms may be updated from time to time. Material changes will be reflected here with an updated effective date; continued use of GridShell after a change constitutes acceptance of the revised terms.

## Contact

gridshell.app@gmail.com
