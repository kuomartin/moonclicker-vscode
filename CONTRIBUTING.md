# Contributing

MoonClicker 的 App 在 [kuomartin/MoonClicker](https://github.com/kuomartin/MoonClicker)，issue 與 milestone 也在那裡；這個 repo 只有 VS Code 擴充套件。

## 本地測試

```bash
npm ci
npm test
```

`npm test` 會先從 `proto/` 產生 TypeScript 再編譯。

## 與 App 共用的部分

* `proto/workbench_stream_event.proto`：Workbench 串流事件的 schema，唯一來源在 App repo 的 `proto/`。App 改了 schema 就把檔案複製過來；不相容的改動要調 `ProtocolVersion`，連線時兩邊版本不同會直接拒絕。
* `lua-meta/moonclicker.lua`：Lua API 的 LuaLS stub。`Lua stub sync` workflow 比對 App master 的綁定與這份 stub 的函式名稱，每個 PR 與每天各跑一次；App 新增或刪除 Lua 函式後這裡要跟著改。

## 版本與發布

App 與擴充套件的版本號前兩碼（x.y）相同，patch 可以不同。從 master 手動執行 Release workflow 並輸入版本號（例如 `0.6.0`），或推 `v` 開頭的版本 tag，會打包 `.vsix`、建 GitHub release，有設 `OVSX_PAT` secret 時發布到 Open VSX；有設 `AZURE_CLIENT_ID`、`AZURE_TENANT_ID` variable 時以 Entra ID managed identity 發布到 VS Code Marketplace（`marketplace-publish` environment，Marketplace 已不建議用 PAT）。手動執行時 workflow 先檢查版本格式為 `MAJOR.MINOR.PATCH`、tag 尚不存在，build 成功後才在該 commit 上建立 tag；上架後同一版本不能重發，所以檢查放在 build 之前。
