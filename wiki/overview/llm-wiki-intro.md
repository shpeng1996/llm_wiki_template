---
title: "LLM Wiki Introduction"
aliases:
  - "LLM Wiki intro"
  - "llm wiki overview"
  - "second brain system"
tags:
  - overview
  - meta
  - wiki
  - pkm
date_created: 2026-04-20
date_updated: 2026-04-29
source_count: 0
sources: []
status: complete
---

# LLM Wiki Introduction

> A personal knowledge base continuously maintained by a large language model (LLM), hosted in an Obsidian vault, letting knowledge accumulate rather than being re-derived each time.

## Core Concepts

LLM Wiki is a personal knowledge management (PKM) system where Claude and similar large language models serve as the primary editors. The user provides raw source material; the LLM organizes, classifies, and summarizes it according to a fixed schema (see `CLAUDE.md` in the vault root), then stores it as structured Markdown pages within an Obsidian vault.

**Key difference from RAG systems**: A typical retrieval-augmented generation (RAG) system extracts information from raw documents at query time — knowledge never accumulates. LLM Wiki works the opposite way: each time new material is ingested, the LLM actively integrates that knowledge into existing wiki pages, updates cross-links, and flags contradictions, so the knowledge base continuously grows and becomes more complete.

The system's core philosophy is **"let the LLM be the knowledge worker, let the human be the knowledge decision-maker"**: the LLM handles repetitive tasks like formatting, cross-linking, and summarization; the user decides what's worth capturing, how to classify it, and when to change direction.

## Why It Matters

The biggest challenge with traditional personal knowledge bases is **maintenance cost** — collecting material is easy, but the sustained effort required to organize, link, and update it causes most systems to gradually collapse. LLM Wiki dramatically reduces cognitive load by outsourcing maintenance to the LLM:

- The LLM never forgets to update cross-links
- The LLM can touch 10–15 related pages in a single operation
- The LLM doesn't fatigue from repetitive work; maintenance cost approaches zero

With Obsidian as the frontend, users can browse the knowledge graph (Graph View), search full text, and view backlinks — no special tooling required. The working model: LLM edits on one side, Obsidian renders results on the other in real time.

## System Architecture

The vault has three layers:

**Raw data layer** (`raw/`): Source documents collected by the user — articles, papers, notes, screenshots. The LLM reads but never writes here; this is the system's single source of truth.

**Knowledge layer** (`wiki/`): Structured pages generated and maintained by the LLM. Organized into subdirectories by type; every page has complete YAML frontmatter and internal links. This layer is the system's core output.

**Schema layer** (`CLAUDE.md`): The schema document that tells the LLM how to operate. Defines directory structure, naming conventions, page format, and detailed workflows for all three operations.

**Two special navigation files**:
- [[index]]: Global directory; categorized index of all pages; the LLM's entry point
- [[log]]: Operation log; append-only record of all operations; provides the wiki's evolution timeline

## Common Applications

- **Reading notes**: Summaries of books, blog posts, and papers; per-source summary pages
- **Concept dictionary**: Structured explanations of AI/ML terminology and programming concepts
- **Tool evaluations**: Comparing frameworks and libraries; recording hands-on experience
- **Research tracking**: Exploring a topic over weeks or months; building up a well-argued synthesis
- **Person profiles**: Researchers' and engineers' key contributions and notable works
- **Personal growth**: Structured journaling, goal tracking, and psychological insights

## Three Core Operations

**Ingest**: Turn raw material into structured wiki pages. The LLM reads the source, extracts key points, creates or updates pages, and updates the index and log.

**Query**: Extract information from the wiki to answer questions. The LLM reads the index first, then relevant pages, and delivers a cited answer. Valuable responses can themselves be stored back into the wiki.

**Lint**: Periodic health check of the entire vault's consistency — finds broken links, pages missing frontmatter, and stale content, then offers repair suggestions.

## Further Reading

- Full operations specification: `CLAUDE.md` (vault root)
- All pages directory: [[index]]
- Operation history: [[log]]

## Changelog

- 2026-04-29: Converted to English (template conversion)
- 2026-04-20: Initial creation (template initialization)
