# LLM Wiki — Operations Manual (CLAUDE.md)

> This document is the sole behavioral specification for Claude maintaining this Obsidian vault.  
> **Read this document in full before every operation.**

---

## 0. System Overview

- **Vault root**: (set by user)
- **Language principle**: All wiki page content primarily in **English**; non-English terms noted in parentheses on first occurrence where helpful
- **Read-only zone**: All files under `raw/` are raw source material — **Claude must never modify them**
- **Operation types**: ingest, query, lint
- **Schema version**: 1.0 (2026-04-12)

---

## 1. Directory Structure

### 1.1 wiki/ Subdirectory Reference

| Subdirectory         | Purpose                    | Typical content                                        |
| -------------------- | -------------------------- | ------------------------------------------------------ |
| `overview/`          | General introductions      | System docs, "What is X" beginner pages                |
| `concept/`           | Concepts / principles      | attention, embedding, tokenization                     |
| `tool/`              | Tools / frameworks         | LangChain, Obsidian, FAISS                             |
| `paper/`             | Paper summaries            | Attention Is All You Need (one file per paper)         |
| `presentations/`     | Slides / decks             | Marp slide decks generated from wiki content           |
| `technique/`         | Technical methods          | Chain-of-Thought, few-shot prompting                   |
| `person/`            | People / organizations     | Andrej Karpathy, Anthropic                             |
| `project/`           | Products / research projects | GPT-4, Claude, Llama                                 |
| `misc/`              | Miscellaneous              | Cross-category or temporarily unclassified knowledge   |

### 1.2 Naming Conventions (slug rules)

- All lowercase English (or romanization)
- Spaces replaced with hyphens (`-`); **no underscores (`_`) or special characters**
- People: `firstname-lastname` (e.g. `andrej-karpathy`)
- Papers: `[year]-[keyword]-[keyword]` (e.g. `2017-attention-transformer`)
- Slug conflicts: add type suffix — `attention-concept.md` vs `attention-paper.md`
- Examples: `chain-of-thought.md`, `langchain.md`, `2023-rag-survey.md`

---

## 2. Page Format Specification

### 2.1 YAML Frontmatter (required)

Every wiki page (except `index.md` and `log.md`) must include:

```yaml
---
title: "Page title"
aliases:
  - "alternative name"
  - "another alias"
tags:
  - primary-tag        # must match subdirectory name (overview/concept/tool etc.)
  - secondary-tag      # topic keywords, lowercase, max 6 tags total
date_created: YYYY-MM-DD
date_updated: YYYY-MM-DD
source_count: 1        # number of raw sources ingested into this page
sources:
  - raw/filename.md    # corresponding file under raw/; use [] if none
status: draft          # draft | complete | stub
---
```

**Status definitions**:
- `complete`: Content is thorough; no additions needed
- `draft`: Basic content present; still being refined
- `stub`: Skeleton page with title and minimal content; pending further work

### 2.2 Standard Page Body Template

```markdown
# Title

> One-sentence definition (25 words or fewer)

## Core Concepts

(Main explanation, 2–5 paragraphs)

## [Type-specific second-level heading — see 2.3]

## Relationships

- Related: [[related-page]]
- Parent concept: [[parent-page]]
- Child concepts: [[child-page]]

## References

- Source material: `raw/corresponding-file`
- External links: (if any)

## Changelog

- YYYY-MM-DD: Initial creation (ingest from raw/xxx.md)
```

### 2.3 Recommended Second-Level Headings by Type

| Type        | Suggested headings                                                                     |
|-------------|----------------------------------------------------------------------------------------|
| `overview`  | Core Concepts, Why It Matters, Common Applications, Further Reading                    |
| `concept`   | Core Concepts, How It Works, Common Misconceptions, Use Cases                          |
| `tool`      | Key Features, Installation & Usage, Pros & Cons, When to Use                          |
| `paper`     | Research Problem, Key Contributions, Method Overview, Experimental Results, Impact & Reception |
| `technique` | Method Description, When to Use, Step-by-Step, Caveats                                |
| `person`    | Background, Major Contributions, Notable Works, Related Links                          |
| `project`   | Product Overview, Core Capabilities, Technical Architecture, History                  |

### 2.4 Obsidian Internal Link Rules

- Reference other wiki pages: `[[slug]]` or `[[slug|display text]]`
- Link to a subsection: `[[slug#section-name]]`
- **Do not use full paths** — Obsidian resolves by shortest unique path automatically
- Within a page, only add a link on a concept's **first** occurrence; no need to repeat

---

## 3. Three Core Operations

### 3.1 Ingest

**Trigger**: User provides raw material (article, notes, PDF content, conversation transcript, etc.)

**Steps**:

1. Read `CLAUDE.md` (confirm schema version)
2. Read the source material (already in `raw/` or provided directly by the user)
3. Discuss key points with the user (optional, depending on material complexity)
4. Determine type and target path:
   - Select subdirectory based on content (overview/concept/tool/paper/technique/person/project/misc)
   - Generate slug per section 1.2 rules
5. Read `wiki/index.md` to check whether a page on this topic already exists:
   - **Exists** → read existing page, merge new information, update `date_updated` and `source_count`
   - **Does not exist** → create new page (full frontmatter + body)
6. If new information conflicts with existing content: keep original, add an "Alternative View" paragraph noting the source discrepancy
7. Update `wiki/index.md`:
   - Add or confirm entry under the appropriate category: `- [[slug|Title]] — one-sentence description (≤30 words)`
   - Update the header date and page count
8. Add a new record at the **top** of `wiki/log.md` (format in section 5)
9. List all created/modified files and report to the user

**Conflict handling**:
- Uncertain category → place in `misc/`, mark `status: draft`, note in log as pending classification
- Slug conflict → add type suffix (e.g. `-concept`, `-paper`)

### 3.2 Query

**Trigger**: User asks about a topic

**Steps**:

1. Read `wiki/index.md` to quickly locate relevant pages
2. Read the 1–3 most relevant pages
3. Follow `[[internal links]]` as needed for further reading
4. Answer using wiki content, clearly citing sources (page names)
5. If the wiki has no relevant information: state this clearly, and suggest ingesting a new source
6. Add a query record at the top of `wiki/log.md`

**Note**: Valuable query responses (e.g. comparative analyses, concept syntheses) may be suggested to the user for storage as a new page or addition to an existing page.

### 3.3 Lint

**Trigger**: User requests a health check, or periodically as the wiki grows

**Steps**:

1. Traverse all `.md` files under `wiki/` (excluding `index.md`, `log.md`)
2. Check each page for:
   - Complete frontmatter (all required fields present)
   - Pages with `status: stub` (need content)
   - Pages with `date_updated` older than 90 days
   - Internal links pointing to non-existent pages (broken links)
3. Review `index.md`:
   - All pages have an entry
   - No entries point to non-existent pages
4. Generate a Lint report sorted by severity:
   - 🔴 Broken links
   - 🟠 Missing / incomplete frontmatter
   - 🟡 Stub pages
   - 🟢 Pages not updated in 90+ days
5. **Present the issue list first; only fix after user confirmation** (no automatic changes)
6. Add a lint record at the top of `wiki/log.md`

---

## 4. index.md Format

**Path**: `wiki/index.md`

```markdown
---
title: "LLM Wiki Knowledge Index"
tags:
  - meta
  - index
date_updated: YYYY-MM-DD
total_pages: N
---

# LLM Wiki — Knowledge Index

> Last updated: YYYY-MM-DD | Total: N pages

---

## Overview

- [[slug|Title]] — one-sentence description

## Concept

## Tool

## Paper

## Technique

## Person

## Project

## Misc
```

**Update rules**:
- Update immediately after every ingest
- `date_updated` and `total_pages` in the header must be accurate
- Write `(no pages yet)` under empty categories; do not leave blank
- Sorting: concept/tool/technique alphabetically; paper by year descending; person by last name alphabetically

---

## 5. log.md Format

**Path**: `wiki/log.md`  
**Rules**: append-only — **never delete or modify existing entries**; newest entry always at the top

```markdown
---
title: "LLM Wiki Operation Log"
tags:
  - meta
  - log
---

# LLM Wiki — Operation Log

> This log is append-only. Newest entries at top. Do not delete or modify existing entries.

---

## [YYYY-MM-DD] ingest | Page Title

- **Source**: raw/filename.md or "provided directly by user"
- **Action**: created wiki/subdir/slug.md / updated wiki/subdir/slug.md
- **Summary**: One or two sentences describing what was ingested
- **Related pages**: [[page1]], [[page2]]

---

## [YYYY-MM-DD] query | Topic

- **Question**: What the user asked
- **Pages used**: [[page1]], [[page2]]
- **Result**: found / not-found / partial

---

## [YYYY-MM-DD] lint | Health Check

- **Pages checked**: N
- **Issues found**: X (broken links: a, missing frontmatter: b, stale: c, stub: d)
- **Fixed**: list of fixes applied, or "reported only, no fixes made"

---
```

---

## 6. Version Information

| Field          | Value      |
|----------------|------------|
| Schema version | 1.0        |
| Created        | 2026-04-12 |
| Last updated   | 2026-04-29 |

**When to update version**:
- Adding a new subdirectory type → update section 1.1, increment minor version (1.0 → 1.1)
- Changing required frontmatter fields → increment major version (1.x → 2.0)
- Always update "Last updated" date in this section when making changes
