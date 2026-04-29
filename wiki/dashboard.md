---
title: "LLM Wiki Dashboard"
aliases:
  - "dashboard"
tags:
  - meta
  - dashboard
date_created: 2026-04-20
date_updated: 2026-04-29
source_count: 0
sources: []
status: complete
---

# LLM Wiki — Dashboard

> Dynamic query panel powered by the [Dataview](https://github.com/blacksmithgu/obsidian-dataview) plugin.

## Stub Pages

Skeleton pages that need content.

```dataview
TABLE date_updated AS "Last Updated", source_count AS "Sources"
FROM "wiki"
WHERE status = "stub"
SORT date_updated ASC
```

## Draft Pages

Pages still being refined.

```dataview
TABLE date_updated AS "Last Updated", source_count AS "Sources"
FROM "wiki"
WHERE status = "draft"
SORT date_updated ASC
```

## Recent Updates

The 15 most recently modified pages.

```dataview
TABLE tags, status, date_updated AS "Last Updated"
FROM "wiki"
WHERE file.name != "index" AND file.name != "log" AND file.name != "dashboard" AND file.name != "flashcards"
SORT date_updated DESC
LIMIT 15
```

## Pages with Most Sources

```dataview
TABLE source_count AS "Sources", tags, date_updated AS "Last Updated"
FROM "wiki/concept" OR "wiki/tool" OR "wiki/paper" OR "wiki/technique" OR "wiki/person" OR "wiki/project"
SORT source_count DESC
LIMIT 10
```

## Orphan Pages (No Inbound Links)

Pages that may not be referenced by other pages (requires manual review — Dataview cannot check inbound links directly).

```dataview
TABLE tags, date_updated AS "Last Updated"
FROM "wiki/concept" OR "wiki/tool" OR "wiki/paper" OR "wiki/technique" OR "wiki/person" OR "wiki/project" OR "wiki/overview" OR "wiki/misc"
WHERE length(file.inlinks) = 0
SORT date_updated ASC
```

## All Pages by Category

```dataview
TABLE tags, status, date_updated AS "Last Updated"
FROM "wiki/concept" OR "wiki/tool" OR "wiki/paper" OR "wiki/technique" OR "wiki/person" OR "wiki/project" OR "wiki/overview" OR "wiki/misc"
SORT file.name ASC
```

## Changelog

- 2026-04-29: Converted to English (template conversion)
- 2026-04-20: Initial creation (template initialization)
