# MoonClicker Script Workbench

English | [正體中文](https://github.com/kuomartin/moonclicker-vscode/blob/master/README.zh-TW.md)

Write, sync and debug [MoonClicker](https://github.com/kuomartin/MoonClicker) Lua scripts on an Android device from VS Code.

## Install

- **VS Code**: install **MoonClicker Script Workbench** from the [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=kuomartin.moonclicker-script-workbench), or search for `MoonClicker` in the Extensions view.
- **VSCodium, Cursor, and other editors that use Open VSX**: install it from [Open VSX](https://open-vsx.org/extension/kuomartin/moonclicker-script-workbench), or search for `MoonClicker` in the Extensions view.

Requires VS Code 1.90.0 or later. The extension and the app must share the same major and minor version (x.y); the patch version can differ. If VS Code reports a protocol mismatch, update the older one.

## Connect to a device

1. In the MoonClicker app, open **Settings → About** and tap the version 7 times to enable developer options, then turn on **Script Workbench** and pairing mode under **Developer options**.
2. In VS Code, open the **MoonClicker Scripts** view and choose **Connect to Device**. Devices on the same network are found via mDNS; you can also enter `ip:port` manually and pair with the PIN shown on the device.

## Scripts folder

The first time you connect to a device, VS Code asks you to choose a local folder; each script on the device is a subfolder of it. The folder opens in the current window, and later connections to the same device reuse it. You're asked again after choosing a different folder or clearing the app's data.

Each connection compares both sides first:

- A file changed on one side only is copied to the other side.
- A file deleted on one side and unchanged on the other is deleted on both.
- A file deleted on one side and changed on the other keeps the changed version.
- A file changed on both sides with different content keeps the local version and saves the device version next to it as `<file>.device`; the next save overwrites the device with the local version.
- If a whole script folder is deleted locally, you're asked before the script is deleted on the device.

While connected, local additions, changes and deletions are pushed to the device immediately, and changes made on the device through Script Workbench are pulled back. Changes made directly on the phone, in the app or a file manager, sync when you choose **Refresh and Sync** or reconnect.

A new local subfolder becomes a script on the device when it has a `main.lua` and its name uses only lowercase letters, digits, `.`, `_` and `-`. You can also create one with **New Script** in the view's title bar. The folder root holds `.moonclicker/` (sync state) and `.luarc.json` (Lua type hints); neither is synced to the device.

## Features

- **Scripts view**: lists the scripts on the device; click one to open its `main.lua`, or use the inline buttons to run or stop it. The title bar can create a script, open the Workbench panel, refresh and sync, and disconnect.
- **Run and stop**: press F5 in a script's `main.lua` to save, sync and run it on the device with live logs, and Shift+F5 to stop. While a script runs, the status bar shows its name; click it to stop. Several scripts can run at once on different displays.
- **Workbench panel**: open it from the connection status in the status bar or the view's title bar. It shows a low-latency H.264 mirror of any display, script logs and `data.set` values per script, template capture and cropping, and live template and OCR tests that generate Lua snippets. On the **Run & debug** step, clicking the mirror taps the device, holding long-presses, and dragging swipes along the same path with the same duration when you release. The panel opens the display you viewed last; if the physical screen isn't being mirrored yet, the panel shows a button to start it.
- **REPL**: press Shift+Enter in a Lua editor to run the selection (or the current line) in a REPL on the device, or run **Open REPL** to type into it in a terminal.
  - The REPL is separate from scripts but has the same API. Globals persist until **Reset REPL** or **Close REPL**; `local`s don't carry over between inputs. Return values and `log()` output appear in the terminal.
  - Ctrl+C interrupts only the running code and keeps the state; Ctrl+D on an empty line closes this REPL. Incomplete input continues on a `>>` line.
  - Up to four REPLs can be open on the device, each with its own globals. **Open REPL** can attach to one that is already open, such as one an AI agent opened through MCP, and then shows the code it runs. Each VS Code window attaches to one REPL at a time.
  - The script folder of the current editor is the root for `require` and template paths; outside a script it is the whole scripts folder.
  - A REPL doesn't occupy a display, so a script can run on the same display at the same time. Closing the terminal doesn't close the REPL on the device; you can attach to it again later.
- **MCP server config**: run **Copy MCP Server Config** to copy the app's built-in MCP server address and token as an `mcpServers` entry for an AI agent. See [MCP server](https://github.com/kuomartin/MoonClicker/blob/master/docs/mcp.md) (in Traditional Chinese).
- **Lua type hints**: the scripts folder gets a `.luarc.json` that loads the MoonClicker API definitions for the Lua language server, so scripts get completion and checking. See the [Lua API reference](https://github.com/kuomartin/MoonClicker/blob/master/docs/lua-api.md) (in Traditional Chinese).

## Exclude files

Put a `.moonclickerignore` in a script folder (for example `<scripts folder>/my-script/`) to keep files from being pushed to the device on save, create, rename or delete. It uses `.gitignore` syntax:

```gitignore
# source assets
*.psd
drafts/
!drafts/keep.png
```

`.git/`, `.vscode/`, `.luarc.json`, `*.device` and `.moonclickerignore` itself are excluded by default; negate them with `!` if needed.

Exclusions apply only to pushing from VS Code: files on the device still sync back. A file already on the device isn't deleted from it when you exclude it later; delete it in the app or a file manager if needed.

## Languages

English and Traditional Chinese, following the VS Code display language.
