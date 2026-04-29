---
name: wiki
description: Operate the LLM Wiki: /wiki ingest to ingest raw material, /wiki query <topic> to look up knowledge, /wiki lint to run health checks
argument-hint: "[ingest | query <topic> | lint]"
---

Full specifications are in `CLAUDE.md`. Execute the corresponding operation based on `$ARGUMENTS`:

**ingest** → Per §3.1: Read raw material, determine type and slug, create or merge wiki pages, update `wiki/index.md`, add a record at the top of `wiki/log.md`, list changed files.

**query `<topic>`** → Per §3.2: Read `wiki/index.md` to locate relevant pages, read 1–3 pages and answer with source citations, add a record at the top of `wiki/log.md`.

**lint** → Per §3.3: Traverse `wiki/` to check frontmatter, broken links, stubs, and pages not updated in 90+ days; report the issue list first and wait for confirmation before fixing; add a record at the top of `wiki/log.md`.

---

Core rules: `raw/` must not be modified; pages use English; internal links use `[[slug]]`; log.md is append-only.
