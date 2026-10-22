# Changelog

## 0.6.0

- Lua stubs for `vision.find_all`, `vision.wait_until` and the `options` argument of `app.launch` (MoonClicker app 0.6.0).

## 0.5.0

- REPL: a persistent Lua session in a VS Code terminal, separate from scripts; run the selection in it, interrupt, reset, or close it with Ctrl+D on an empty line.
- Clicking the mirror taps the device and dragging swipes.
- `MoonClicker: Copy MCP Server Config` copies the settings an AI agent needs to reach the Workbench MCP server.
- Lua stubs for the `ui` module's view tree functions.

## 0.4.0

- Several scripts can run at once; logs and data are shown per script, and Stop asks which script to stop.
- The Workbench protocol is versioned; a mismatch between the app and the extension is reported instead of failing silently.
- English UI with Traditional Chinese translation.
- Switching the mirrored display keeps the panel in place, and the mirror recovers after a decode error.
