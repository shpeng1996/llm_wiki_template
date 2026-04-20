---
title: "LLM Wiki 儀表板"
aliases:
  - "dashboard"
  - "儀表板"
tags:
  - meta
  - dashboard
date_created: 2026-04-20
date_updated: 2026-04-20
source_count: 0
sources: []
status: complete
---

# LLM Wiki — 儀表板

> 由 [Dataview](https://github.com/blacksmithgu/obsidian-dataview) 插件驅動的動態查詢面板。

## 待補充頁面（Stub）

需要補充內容的骨架頁面。

```dataview
TABLE date_updated AS "最後更新", source_count AS "來源數"
FROM "wiki"
WHERE status = "stub"
SORT date_updated ASC
```

## 草稿中頁面（Draft）

仍在整理中的頁面。

```dataview
TABLE date_updated AS "最後更新", source_count AS "來源數"
FROM "wiki"
WHERE status = "draft"
SORT date_updated ASC
```

## 最近更新

最近修改的 15 個頁面。

```dataview
TABLE tags, status, date_updated AS "最後更新"
FROM "wiki"
WHERE file.name != "index" AND file.name != "log" AND file.name != "dashboard" AND file.name != "flashcards"
SORT date_updated DESC
LIMIT 15
```

## 來源最多的頁面

```dataview
TABLE source_count AS "來源數", tags, date_updated AS "最後更新"
FROM "wiki/concept" OR "wiki/tool" OR "wiki/paper" OR "wiki/technique" OR "wiki/person" OR "wiki/project"
SORT source_count DESC
LIMIT 10
```

## 孤立頁面（無內連結）

可能缺少其他頁面引用的頁面（需人工確認，Dataview 無法直接檢查 inbound links）。

```dataview
TABLE tags, date_updated AS "最後更新"
FROM "wiki/concept" OR "wiki/tool" OR "wiki/paper" OR "wiki/technique" OR "wiki/person" OR "wiki/project" OR "wiki/overview" OR "wiki/misc"
WHERE length(file.inlinks) = 0
SORT date_updated ASC
```

## 各分類頁面一覽

```dataview
TABLE tags, status, date_updated AS "最後更新"
FROM "wiki/concept" OR "wiki/tool" OR "wiki/paper" OR "wiki/technique" OR "wiki/person" OR "wiki/project" OR "wiki/overview" OR "wiki/misc"
SORT file.name ASC
```

## 修改記錄

- 2026-04-20：初始建立（模板初始化）
