---
name: wiki
description: Operate this LLM Wiki. Used for wiki ingest to ingest raw material, wiki query to look up topic knowledge, and wiki lint to run health checks. Must read the repository root CLAUDE.md in full before execution and follow its specifications to update wiki/index.md and wiki/log.md.
---

# Wiki

This skill is the native entry point for Codex; actual specifications are governed by the repository root `CLAUDE.md`.

## Startup Rules

Before performing any wiki work:

1. Read `CLAUDE.md` in full.
2. Determine whether the user's intent is `ingest`, `query`, or `lint`.
3. Follow `CLAUDE.md` throughout; if this skill conflicts with `CLAUDE.md`, `CLAUDE.md` takes precedence.

## Supported Operations

### `wiki ingest`

Execute per `CLAUDE.md` §3.1:

- Read the raw material or sources in `raw/`
- Determine the category and slug
- Create or update the corresponding wiki page
- Update `wiki/index.md`
- Add an ingest record at the top of `wiki/log.md`
- Report changed files

### `wiki query <topic>`

Execute per `CLAUDE.md` §3.2:

- Read `wiki/index.md` first to locate the topic
- Read the 1–3 most relevant pages, following internal links as needed
- Answer using wiki content and cite the page sources
- If the wiki has no answer, state this clearly
- Add a query record at the top of `wiki/log.md`

### `wiki lint`

Execute per `CLAUDE.md` §3.3:

- Traverse all pages under `wiki/`, excluding `index.md` and `log.md`
- Check frontmatter, stubs, pages not updated in over 90 days, and broken links
- Check whether `wiki/index.md` is complete and its links are correct
- Report the issue list first; only fix after user confirmation
- Add a lint record at the top of `wiki/log.md`

## Non-Negotiable Rules

- Never modify any files under `raw/`
- Wiki content is primarily in English
- Use Obsidian link format `[[slug]]` or `[[slug|display text]]`
- Must follow frontmatter, naming, index, and log format specifications
- `wiki/log.md` is append-only; existing records must not be deleted or modified
