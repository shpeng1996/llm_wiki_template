# LLM Wiki — Personal Knowledge Base Template

> A personal knowledge base continuously maintained by a large language model (LLM), hosted in an Obsidian vault.

---

## What is LLM Wiki?

LLM Wiki is a personal knowledge management system where Claude and similar large language models serve as the primary editors. The user provides raw source material; the LLM organizes, classifies, and summarizes it according to a fixed schema (`CLAUDE.md`), storing the result as structured Markdown pages in an Obsidian vault.

**Key difference from RAG**: RAG systems extract information from raw documents at query time — knowledge never accumulates. LLM Wiki actively integrates knowledge into existing wiki pages at ingest time, so the knowledge base continuously grows.

---

## Quick Start

### 1. Install Obsidian

Download and install Obsidian from [obsidian.md](https://obsidian.md/).

### 2. Open this folder as a Vault

- Open Obsidian → "Open folder as vault"
- Select this `template/` folder (or rename it to your topic)

### 3. Install recommended plugins

In Obsidian Settings → Community plugins, install:

| Plugin | Purpose | Required |
|--------|---------|----------|
| [Dataview](https://github.com/blacksmithgu/obsidian-dataview) | Dashboard dynamic queries | Recommended |
| [Spaced Repetition](https://github.com/st3v3nmw/obsidian-spaced-repetition) | Spaced repetition flashcards | Optional |

### 4. Set up Claude Code

Install the [Claude Code](https://claude.ai/code) CLI, then run it from the vault root:

```bash
claude
```

Claude will automatically read `CLAUDE.md` and operate the wiki according to its rules.

### 5. Start using it

Place raw material you want to capture (articles, notes, paper contents) in the `raw/` directory, then run `/wiki ingest` in Claude Code.

---

## Directory Structure

```
vault/
├── CLAUDE.md          # AI behavior specification (core)
├── raw/               # Raw source material (Claude reads only, never writes)
└── wiki/
    ├── index.md       # Global directory (entry point for all pages)
    ├── log.md         # Operation log (append-only)
    ├── dashboard.md   # Dataview dashboard
    ├── flashcards.md  # Spaced repetition study cards
    ├── overview/      # General introductions
    ├── concept/       # Concepts / principles
    ├── tool/          # Tools / frameworks
    ├── paper/         # Paper summaries
    ├── technique/     # Technical methods
    ├── person/        # People / organizations
    ├── project/       # Products / research projects
    ├── presentations/ # Slides / decks
    ├── misc/          # Miscellaneous
    └── journal/       # Research journal entries
```

---

## Three Core Operations

| Command | Description |
|---------|-------------|
| `/wiki ingest` | Process raw material in `raw/` into wiki pages |
| `/wiki query <topic>` | Query the wiki for knowledge on a topic |
| `/wiki lint` | Health-check the vault for broken links and formatting issues |

---

## Customization

`CLAUDE.md` defines all of Claude's operational behavior. You can modify:

- **Section 1.1**: Add or adjust subdirectory categories
- **Section 2.1**: Adjust frontmatter fields
- **Section 2.3**: Standard second-level headings per page type
- **Language principle**: Defaults to English; change as needed

After making changes, update the schema version number per the rules in CLAUDE.md section 6.

---

## License

This template is released under the MIT License. Feel free to modify it for your needs.
