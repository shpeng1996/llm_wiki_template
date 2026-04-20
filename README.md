# LLM Wiki — 個人知識庫模板

> 一個由大型語言模型（LLM）持續維護的個人知識庫，以 Obsidian vault 為載體。

---

## 什麼是 LLM Wiki？

LLM Wiki 是一套以 Claude 等大型語言模型為主要編輯者的個人知識管理系統。使用者提供原始資料，由 LLM 依照固定的 Schema（`CLAUDE.md`）進行整理、分類、摘要，並以結構化的 Markdown 頁面儲存於 Obsidian vault 中。

**與 RAG 系統的核心差異**：RAG 系統每次問答時從原始文件即時提取資訊，知識不會累積；LLM Wiki 則在每次攝入新資料時，主動將知識整合進既有 wiki 頁面，讓知識庫持續增長。

---

## 快速開始

### 1. 安裝 Obsidian

前往 [obsidian.md](https://obsidian.md/) 下載並安裝 Obsidian。

### 2. 開啟此資料夾為 Vault

- 開啟 Obsidian → 「Open folder as vault」
- 選擇此 `template/` 資料夾（或將其重新命名為你的主題名稱）

### 3. 安裝建議插件

在 Obsidian 設定 → Community plugins 中安裝：

| 插件 | 用途 | 必要性 |
|------|------|--------|
| [Dataview](https://github.com/blacksmithgu/obsidian-dataview) | 儀表板動態查詢 | 建議 |
| [Spaced Repetition](https://github.com/st3v3nmw/obsidian-spaced-repetition) | 間隔重複學習卡片 | 選用 |

### 4. 設定 Claude Code

安裝 [Claude Code](https://claude.ai/code) CLI，在此 vault 根目錄執行：

```bash
claude
```

Claude 會自動讀取 `CLAUDE.md` 並依照規範操作 wiki。

### 5. 開始使用

將你想收錄的原始材料（文章、筆記、論文內容）放入 `raw/` 目錄，然後在 Claude Code 中執行 `/wiki ingest`。

---

## 目錄結構

```
vault/
├── CLAUDE.md          # AI 行為規範（核心）
├── raw/               # 原始資料（Claude 只讀不寫）
└── wiki/
    ├── index.md       # 全域目錄（所有頁面的入口）
    ├── log.md         # 操作日誌（append-only）
    ├── dashboard.md   # Dataview 儀表板
    ├── flashcards.md  # 間隔重複學習卡片
    ├── overview/      # 通論介紹
    ├── concept/       # 概念 / 原理
    ├── tool/          # 工具 / 框架
    ├── paper/         # 論文摘要
    ├── technique/     # 技術手法
    ├── person/        # 人物 / 組織
    ├── project/       # 產品 / 研究項目
    ├── presentations/ # 簡報 / 投影片
    ├── misc/          # 雜項
    └── journal/       # 研究日誌
```

---

## 三大操作

| 指令 | 說明 |
|------|------|
| `/wiki ingest` | 將 raw/ 中的原始材料整理為 wiki 頁面 |
| `/wiki query <主題>` | 從 wiki 查詢某主題的知識 |
| `/wiki lint` | 健檢整個 vault，找出斷連結、缺少格式的頁面 |

---

## 客製化

`CLAUDE.md` 定義了 Claude 的所有操作行為。你可以修改：

- **第 1.1 節**：新增或調整子目錄分類
- **第 2.1 節**：調整 frontmatter 欄位
- **第 2.3 節**：各類型頁面的標準二級標題
- **語言原則**：預設繁體中文，可改為其他語言

修改後，Schema 版本號請依規則更新（見 CLAUDE.md 第 6 節）。

---

## 授權

本模板以 MIT 授權開放使用。歡迎依需求自由修改。
