---
name: wiki
description: 操作此 LLM Wiki。用於 wiki ingest 攝入原始材料、wiki query 查詢主題知識、wiki lint 執行健檢；執行前必須完整閱讀倉庫根目錄的 CLAUDE.md，並依其規範更新 wiki/index.md 與 wiki/log.md。
---

# Wiki

此技能是 Codex 的原生入口；實際規範以倉庫根目錄 `CLAUDE.md` 為準。

## 啟動規則

在執行任何 wiki 工作前：

1. 完整閱讀 `CLAUDE.md`。
2. 依使用者意圖判斷是 `ingest`、`query` 或 `lint`。
3. 全程遵守 `CLAUDE.md`；若此技能與 `CLAUDE.md` 衝突，以 `CLAUDE.md` 為準。

## 支援操作

### `wiki ingest`

依 `CLAUDE.md` §3.1 執行：

- 閱讀原始材料或 `raw/` 中來源
- 判斷分類與 slug
- 新建或更新對應 wiki 頁面
- 更新 `wiki/index.md`
- 在 `wiki/log.md` 頂部新增 ingest 紀錄
- 回報異動檔案

### `wiki query <主題>`

依 `CLAUDE.md` §3.2 執行：

- 先讀 `wiki/index.md` 定位主題
- 讀最相關的 1 至 3 個頁面，必要時追蹤內部連結
- 以 wiki 內容作答並標示頁面來源
- 若 wiki 內沒有答案，要明確說明
- 在 `wiki/log.md` 頂部新增 query 紀錄

### `wiki lint`

依 `CLAUDE.md` §3.3 執行：

- 遍歷 `wiki/` 頁面，排除 `index.md` 與 `log.md`
- 檢查 frontmatter、stub、超過 90 天未更新、broken links
- 檢查 `wiki/index.md` 是否完整且連結正確
- 先回報問題清單，等使用者確認後才修復
- 在 `wiki/log.md` 頂部新增 lint 紀錄

## 不可違反的規則

- 不可修改 `raw/` 下任何檔案
- wiki 內容以繁體中文為主
- 使用 Obsidian 連結格式 `[[slug]]` 或 `[[slug|顯示文字]]`
- 必須遵守 frontmatter、命名、index、log 的格式規範
- `wiki/log.md` 是 append-only，舊紀錄不可刪改
