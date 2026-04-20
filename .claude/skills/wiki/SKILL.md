---
name: wiki
description: 操作 LLM Wiki：/wiki ingest 攝入原始材料、/wiki query <主題> 查詢知識、/wiki lint 執行健檢
argument-hint: "[ingest | query <主題> | lint]"
---

完整規範見 `CLAUDE.md`。依 `$ARGUMENTS` 執行對應操作：

**ingest** → 依 §3.1：讀取原始材料，判斷類型與 slug，新建或合併 wiki 頁面，更新 `wiki/index.md`，在 `wiki/log.md` 頂部新增記錄，列出異動檔案。

**query `<主題>`** → 依 §3.2：讀 `wiki/index.md` 定位相關頁面，讀取 1–3 個頁面作答並標示來源，在 `wiki/log.md` 頂部新增記錄。

**lint** → 依 §3.3：遍歷 `wiki/` 檢查 frontmatter、broken links、stub、90 天未更新，先回報清單等確認再修復，在 `wiki/log.md` 頂部新增記錄。

---

核心規則：`raw/` 不可更動；頁面用繁體中文；內部連結用 `[[slug]]`；log.md append-only。
