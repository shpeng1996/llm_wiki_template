# LLM Wiki — 操作手冊 (CLAUDE.md)

> 本文件是 Claude 維護此 Obsidian vault 的唯一行為規範。  
> **每次操作前必須完整閱讀本文件。**

---

## 0. 系統概述

- **Vault 根目錄**：（依使用者設定）
- **語言原則**：所有 wiki 頁面內容以**繁體中文**為主；英文術語首次出現時加括號標注原文，例如「注意力機制（attention mechanism）」
- **不可修改區**：`raw/` 目錄下的所有檔案均為原始資料，**Claude 絕對不可更動**
- **操作類型**：ingest（攝入）、query（查詢）、lint（健檢）
- **Schema 版本**：1.0（2026-04-12）

---

## 1. 目錄結構規範

### 1.1 wiki/ 子目錄對照表

| 子目錄              | 用途        | 典型內容                                |
| ---------------- | --------- | ----------------------------------- |
| `overview/`      | 通論介紹      | 系統說明、「什麼是 X」型的入門頁面                  |
| `concept/`       | 概念 / 原理   | attention、embedding、tokenization    |
| `tool/`          | 工具 / 框架   | LangChain、Obsidian、FAISS            |
| `paper/`         | 論文摘要      | Attention Is All You Need（一篇一檔）     |
| `presentations/` | 簡報 / 投影片  | 由 wiki 內容產生的 Marp slide decks       |
| `technique/`     | 技術手法      | Chain-of-Thought、few-shot prompting |
| `person/`        | 人物 / 組織   | Andrej Karpathy、Anthropic           |
| `project/`       | 產品 / 研究項目 | GPT-4、Claude、Llama                  |
| `misc/`          | 雜項        | 跨類或暫時未分類的知識                         |

### 1.2 命名慣例（slug 規則）

- 全小寫英文（或拼音）
- 空格改為連字號（`-`），**不使用底線（`_`）或特殊字元**
- 人名：`firstname-lastname`（如 `andrej-karpathy`）
- 論文：`[年份]-[關鍵詞]-[關鍵詞]`（如 `2017-attention-transformer`）
- Slug 衝突時加類型後綴：`attention-concept.md` vs `attention-paper.md`
- 範例：`chain-of-thought.md`、`langchain.md`、`2023-rag-survey.md`

---

## 2. 頁面格式規範

### 2.1 YAML Frontmatter（必填）

每個 wiki 頁面（index.md 與 log.md 除外）必須包含：

```yaml
---
title: "頁面標題（繁體中文）"
aliases:
  - "英文別名"
  - "其他中文別名"
tags:
  - 主分類標籤        # 必須與子目錄名稱一致（overview/concept/tool 等）
  - 次要標籤          # 主題關鍵詞，小寫英文，不超過總計 6 個 tag
date_created: YYYY-MM-DD
date_updated: YYYY-MM-DD
source_count: 1       # 本頁面已攝入的原始來源數量
sources:
  - raw/檔案名.md     # 對應 raw/ 下的原始檔案；若無則填 []
status: draft         # draft | complete | stub
---
```

**status 定義**：
- `complete`：內容充實，不需要補充
- `draft`：已有基本內容，仍在整理中
- `stub`：骨架頁面，僅有標題與少量說明，待後續補充

### 2.2 頁面主體標準模板

```markdown
# 標題

> 一句話定義（25 字以內）

## 核心概念

（主要說明，2–5 段）

## [依類型選用的二級標題，見 2.3]

## 與其他概念的關係

- 相關：[[相關頁面]]
- 上位概念：[[父概念頁面]]
- 下位概念：[[子概念頁面]]

## 參考資料

- 原始來源：`raw/對應檔案`
- 外部連結：（若有）

## 修改記錄

- YYYY-MM-DD：初始建立（ingest from raw/xxx.md）
```

### 2.3 各類型頁面的二級標題慣例

| 類型 | 建議二級標題 |
|------|-------------|
| `overview` | 核心概念、為什麼重要、常見應用、延伸閱讀 |
| `concept` | 核心概念、工作原理、常見誤解、應用場景 |
| `tool` | 核心功能、安裝使用、優缺點、適用場景 |
| `paper` | 研究問題、核心貢獻、方法概述、實驗結果、影響與評價 |
| `technique` | 方法描述、使用時機、操作步驟、注意事項 |
| `person` | 背景簡介、主要貢獻、代表作品、相關連結 |
| `project` | 產品定位、核心能力、技術架構、發展歷程 |

### 2.4 Obsidian 內部連結規則

- 引用其他 wiki 頁面：`[[slug]]` 或 `[[slug|顯示文字]]`
- 連結至子標題：`[[slug#標題名稱]]`
- **不要使用完整路徑**，Obsidian 以最短唯一路徑自動解析
- 同一頁面中，某概念**首次**出現才加連結，之後重複提及無需重複加

---

## 3. 三大操作流程

### 3.1 Ingest（攝入）

**觸發**：使用者提供原始材料（文章、筆記、PDF 內容、對話記錄等）

**步驟**：

1. 讀取 `CLAUDE.md`（確認規範版本）
2. 閱讀原始材料（已存入 `raw/` 或使用者直接提供）
3. 與使用者討論關鍵重點（可選，依材料複雜度決定）
4. 判斷類型與目標路徑：
   - 依內容選擇子目錄（overview/concept/tool/paper/technique/person/project/misc）
   - 依 1.2 規則生成 slug
5. 讀取 `wiki/index.md`，確認是否已有同主題頁面：
   - **已存在** → 讀取現有頁面，合併新資訊，更新 `date_updated` 與 `source_count`
   - **不存在** → 新建頁面（完整 frontmatter + 主體）
6. 若新資料與現有頁面有矛盾：保留原有資訊，新增「另一說法」段落，標注來源差異
7. 更新 `wiki/index.md`：
   - 在對應分類下新增或確認條目：`- [[slug|中文標題]] — 一句話描述（≤30 字）`
   - 更新標頭的日期與頁面計數
8. 在 `wiki/log.md` **頂部**新增一筆記錄（格式見第 5 節）
9. 列出建立 / 修改的檔案清單，告知使用者

**衝突處理**：
- 不確定分類 → 歸入 `misc/`，標記 `status: draft`，在 log 記錄待分類
- Slug 衝突 → 加類型後綴（如 `-concept`、`-paper`）

### 3.2 Query（查詢）

**觸發**：使用者詢問某主題的知識

**步驟**：

1. 讀取 `wiki/index.md`，快速定位相關頁面
2. 讀取最相關的 1–3 個頁面
3. 若有必要，追蹤頁面中的 `[[內部連結]]` 繼續閱讀
4. 以 wiki 內容作答，明確標示引用來源（頁面名稱）
5. 若 wiki 中無相關資訊：明確告知，並建議攝入新來源
6. 在 `wiki/log.md` 頂部新增查詢記錄

**重要**：有價值的查詢回答（如比較分析、概念整合）可建議使用者將其存入 wiki，作為新頁面或現有頁面的補充。

### 3.3 Lint（健檢）

**觸發**：使用者要求健檢，或 wiki 規模較大時定期執行

**步驟**：

1. 遍歷 `wiki/` 下所有 `.md` 檔案（排除 `index.md`、`log.md`）
2. 逐頁檢查：
   - Frontmatter 是否完整（所有必填欄位齊全）
   - `status: stub` 的頁面（需補充）
   - `date_updated` 超過 90 天未更新的頁面
   - 內部連結是否指向不存在的頁面（broken links）
3. 確認 `index.md`：
   - 所有頁面是否都有條目
   - 是否有條目指向不存在的頁面
4. 產生 Lint 報告，依嚴重程度排序：
   - 🔴 Broken links
   - 🟠 Missing / incomplete frontmatter
   - 🟡 Stub 頁面
   - 🟢 超過 90 天未更新
5. **先呈報問題清單，等使用者確認後再修復**（不自動修改）
6. 在 `wiki/log.md` 頂部新增健檢記錄

---

## 4. index.md 格式規範

**路徑**：`wiki/index.md`

```markdown
---
title: "LLM Wiki 知識目錄"
tags:
  - meta
  - index
date_updated: YYYY-MM-DD
total_pages: N
---

# LLM Wiki — 知識目錄

> 最後更新：YYYY-MM-DD | 共 N 個頁面

---

## 概覽（Overview）

- [[slug|中文標題]] — 一句話描述

## 概念（Concept）

## 工具（Tool）

## 論文（Paper）

## 技術手法（Technique）

## 人物（Person）

## 專案（Project）

## 雜項（Misc）
```

**更新規則**：
- 每次 ingest 後立即更新
- 標頭的 `date_updated` 與 `total_pages` 必須準確
- 各分類下無頁面時填 `（尚無頁面）`，不要留空
- 排序：概念/工具/技術手法按字母，論文按年份倒序，人物按姓氏字母

---

## 5. log.md 格式規範

**路徑**：`wiki/log.md`  
**規則**：append-only，**禁止刪除或修改已有條目**，最新條目永遠在最上方

```markdown
---
title: "LLM Wiki 操作日誌"
tags:
  - meta
  - log
---

# LLM Wiki — 操作日誌

> 本日誌為 append-only。最新條目在最上方。禁止刪除或修改已有條目。

---

## [YYYY-MM-DD] ingest | 頁面標題

- **來源**：raw/檔案名.md 或「使用者直接提供」
- **動作**：新建 wiki/子目錄/slug.md ／ 更新 wiki/子目錄/slug.md
- **摘要**：一到兩句話說明攝入的內容
- **關聯頁面**：[[頁面1]]、[[頁面2]]

---

## [YYYY-MM-DD] query | 查詢主題

- **問題**：使用者問了什麼
- **使用頁面**：[[頁面1]]、[[頁面2]]
- **結果**：found（找到）/ not-found（未找到）/ partial（部分找到）

---

## [YYYY-MM-DD] lint | 健檢報告

- **檢查頁面數**：N
- **發現問題**：X 個（broken links: a, missing frontmatter: b, stale: c, stub: d）
- **已修復**：列出修復項目，或「本次僅回報，未修復」

---
```

---

## 6. 版本資訊

| 欄位 | 值 |
|------|----|
| Schema 版本 | 1.0 |
| 建立日期 | 2026-04-12 |
| 最後更新 | 2026-04-12 |

**版本更新時機**：
- 新增子目錄類型 → 更新第 1.1 節，版本號小版號 +1（1.0 → 1.1）
- 變更 frontmatter 必填欄位 → 版本號大版號 +1（1.x → 2.0）
- 每次更新同步修改本節的「最後更新」日期
