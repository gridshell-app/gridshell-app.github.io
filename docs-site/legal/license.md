# License

GridShell has two components, under two different licenses.

## The server and Python library (open source)

The `gridshell` Python package - the self-hosted server and Python client library that acts as the app's backend - is open source, under the MIT License. Repeated here in full:

```
MIT License

Copyright (c) 2026 The GridShell developers

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

It's open source for three concrete reasons, not just as a formality:

- **Trust.** You can read exactly what the backend does with your spreadsheet data and your machine, rather than taking a privacy claim on faith.
- **Fit.** Adapt it to your own personal or organizational security requirements - self-host it however your own environment demands, without waiting on the developer to support your specific setup.
- **Build on it.** It's a real library, not just a binary - write your own tools against it, or your own alternative server speaking the same wire protocol (see [Writing your own server](../library/server.md#writing-your-own-server)).

The same terms are also included as a `LICENSE` file in the [`gridshell` package itself](https://pypi.org/project/gridshell/).

## The Sheets add-on (closed source)

The GridShell Sheets add-on - the in-Sheets terminal UI, distributed only through the Google Workspace Marketplace - is closed source at this point. Its code isn't part of the public GridShell repository.

```
Copyright (c) 2026 The GridShell developers. All rights reserved.

This software (the GridShell Google Sheets add-on) is proprietary. It is
not licensed for reuse, modification, redistribution, or derivative works,
and no rights are granted beyond ordinary use of the installed add-on
through the Google Workspace Marketplace, under the GridShell Terms of
Service.
```
