# Privacy Policy

## Definitions

- **The developer**: the developer of GridShell.
- **The app**: the GridShell Google Sheets add-on.
- **Personal data**: information that identifies or can be used to identify a person, such as a name, email address, phone number, or home address.
- **Google user data**: data the app is authorized to access through the Google permissions ("scopes") listed in the next section.

## Data the app accesses

The app is authorized for the following scopes:

- `spreadsheets.currentonly` - access to the contents of the one spreadsheet you have open when you run the app (cell values, formulas, formatting, charts, and so on), and only that spreadsheet. The app cannot see or touch any other file in your Google account. This content may include personal data, if you have put personal data in the spreadsheet. Google describes this scope as allowing a spreadsheet to be viewed, modified and shared with other users; the app itself does not share spreadsheets or change who has access to them.
- `script.container.ui` - permission to show the sidebar and dialog the app's interface is built from, to run the app's code in your browser, and to receive what you enter into them. What you enter in Settings is saved as described under "Data retention and deletion". What you type in the terminal goes from your browser directly to your own server; it does not pass through the developer.
- `userinfo.email` and `userinfo.profile` - basic account information (your name, email address, and profile picture). These scopes are included by default for every app listed on the Google Workspace Marketplace, which is why Google's install screen lists them. The app does not read, use, store, or transfer this information.

The app does not request access to any other Google data (such as Drive or contacts), and it does not collect personal data.

## How the app works with that data

The app gives you a terminal (a shell) inside Google Sheets. The shell runs on a terminal server that you install and run yourself, on a machine you choose; see [Introduction](../index.md) for the two-component architecture. The developer does not operate a shared backend, and no data passes through any server the developer controls.

- The `script.container.ui` scope is what displays the shell's output in the sidebar and dialog, whatever that output is.
- The `spreadsheets.currentonly` scope is what lets programs you run in the shell read or write the open spreadsheet, through a connection between the app and your own server. Whether a program touches the spreadsheet at all, and what it does with the data, depends entirely on the program you choose to run: a script you wrote, an AI agent or assistant, or any other tool. The app does not choose, inspect, or control those programs.
- The terminal's display library (xterm) is loaded by your browser directly from a public CDN (jsDelivr), and the interface also loads Google's own stylesheets and fonts. These requests are made by your browser and carry no spreadsheet data.

## Use, sharing, and transfer

- The app uses Google user data only to provide the features visible in its interface: relaying the commands you or your programs issue to the open spreadsheet, and showing you the results.
- The developer does not receive, store, sell, or share Google user data, and does not transfer it to any third party. It is not used for advertising.
- The developer does not use Google user data to develop, improve, or train any machine learning or AI model.
- Data leaves the spreadsheet only along the path you set up: to your own server, and from there to whichever programs you run in the shell. If one of those programs sends data on to another service, such as a hosted AI model, that is a decision made in the program you chose, not by the app or the developer, and that service's own terms apply.
- Humans at the developer do not read Google user data; the developer has no access to it.

## Data protection

The app does not store Google user data; it only relays it between the spreadsheet and your server. The connection goes to your server's address, which you enter in Settings: a server on your own machine, or a remote one (use a `wss://` address to encrypt the connection). The server requires an authentication token by default, so nothing can connect to it without that token. If you run a server that other machines can reach, see [Server: Security & Deployment](../library/server.md#security-deployment) for how to secure it.

## Data retention and deletion

- Spreadsheet contents are never copied into any storage operated by the developer, because there isn't any.
- Settings (server address, authentication token, theme, font size, etc.) are stored per user, in your own Google account's Apps Script user properties - not on any server operated by the developer. They can remain associated with your account after the app is uninstalled; to remove them, clear the server address and token in Settings and Save before uninstalling.
- The list of open shells for a document is stored per document, in that document's Apps Script properties.
- Terminal output is buffered in memory on your own self-hosted server for reattach purposes, and is not persisted beyond that server's own process lifetime (subject to whatever you, as its operator, choose to log).

## Google API Services User Data Policy

The app's use and transfer of information received from Google APIs to any other app will adhere to the [Google API Services User Data Policy](https://developers.google.com/terms/api-services-user-data-policy), including the Limited Use requirements.

## Contact

gridshell.app@gmail.com. If you email this address or open a GitHub issue, the details you choose to include are received by the developer only for the purpose of responding.
